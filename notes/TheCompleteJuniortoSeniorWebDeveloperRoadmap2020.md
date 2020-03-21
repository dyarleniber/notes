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

- Steps:
	- Process HTML markup and build the DOM tree.
	- Process CSS markup and build the CSSOM tree.
	- Combine the DOM and CSSOM into a render tree.
	- Run layout on the render tree to compute geometry of each node.
	- Paint the individual nodes to the screen.

Optimizing the critical rendering path is the process of minimizing the total amount of time spent performing steps 1 through 5 in the above sequence.

When a user requests a page for your site the page HTML starts streaming to the browser. As soon as a browser encounters a tag for an external imagem, script or css file, it will start downloading the file simultaneously. When the browser receives thge HTML it does something called parsing.

After understanding the document this is what it does it starts creating the DOM, and again as it's building that as soon as it sees an external resource it goes ahead and starts downloading all of those.

Usually the css and javascript files take high priority and other files like images take lower priority.

- So how do we optimize this process?

- Step 1 (HTML):

Well the first thing you want to do is to load styles that is CSS files as soon as possible and script that is javascript files as late as possible with a few exceptions here and there.
So if you put javascript in the head tag in HTML, the problem with that positioning if it's at the top is that it blocks page rendering scripts historically blocked additional resources from being downloaded more quickly.
By replacing them at the bottom or by placing them at the bottom (After end body tag) you're style content and media could start downloading more quickly given the perception of improved performance.

> Load style tag in the <head>
> Load script right before </body>

- Step2 (CSS):

CSS is called render blocking because in order to construct the rendered tree and print something onto the screen we're waiting for the CSS DOM to complete and combine it with the Dom to create the render tree, so CSS is rendered blocking.

Internal ou inline styleshees are more faster than external css files
