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


