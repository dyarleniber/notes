[Back](../README.md)

# HTML

## Introduction

HTML, or HyperText Markup Language, is a markup language used to describe the structure of a web page.

HTML5 introduces more descriptive HTML tags. These include `main`, `header`, `footer`, `nav`, `video`, `article`, `section` and others.

These tags give a descriptive structure to your HTML, make your HTML easier to read, and help with Search Engine Optimization (SEO) and accessibility. The main HTML5 tag helps search engines and other developers find the main content of your page.

## Declare the Doctype of an HTML Document

There are a few elements that give overall structure to your page, and should be included in every HTML document.

At the top of your document, you need to tell the browser which version of HTML your page is using. HTML is an evolving language, and is updated regularly. Most major browsers support the latest specification, which is HTML5. However, older web pages may use previous versions of the language.

You tell the browser this information by adding the `<!DOCTYPE ...>` tag on the first line, where the ... part is the version of HTML. For HTML5, you use `<!DOCTYPE html>`.

> The ! and uppercase DOCTYPE is important, especially for older browsers. The html is not case sensitive.

Next, the rest of your HTML code needs to be wrapped in html tags. The opening `<html>` goes directly below the `<!DOCTYPE html>` line, and the closing `</html>` goes at the end of the page.

## Example

```html
<!DOCTYPE html>
<html>
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Document</title>
  </head>
  <body>
    <h2>CatPhotoApp</h2>
    <main>
      <p>Click here to view more <a href="#">cat photos</a>.</p>
      <a href="#"><img src="https://bit.ly/fcc-relaxing-cat" alt="A cute orange cat lying on its back."></a>
    <div>
      <p>Things cats love:</p>
      <ul>
        <li>cat nip</li>
        <li>laser pointers</li>
        <li>lasagna</li>
      </ul>
      </div>
      <div>
      <p>Top 3 things cats hate:</p>
      <ol>
        <li>flea treatment</li>
        <li>thunder</li>
        <li>other cats</li>
      </ol>
    </div>
      <form action="/submit-cat-photo">
        <label for="indoor"><input id="indoor" type="radio" name="indoor-outdoor" value="indoor" checked> Indoor</label>
        <label for="outdoor"><input id="outdoor" type="radio" name="indoor-outdoor" value="outdoor"> Outdoor</label><br>
        <label for="loving"><input id="loving" type="checkbox" name="personality" value="loving" checked> Loving</label>
        <label for="lazy"><input id="lazy" type="checkbox" name="personality" value="lazy"> Lazy</label>
        <label for="energetic"><input id="energetic" type="checkbox" name="personality" value="energetic"> Energetic</label><br>
        <input type="text" placeholder="cat photo URL" required>
        <button type="submit">Submit</button>
      </form>
    </main>
  </body>
</html>
```

## Simple boilerplate

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    
</body>

</html>
```

- [HTML5 new elements](http://w3schools-fa.ir/html/html5_new_elements.html)

- [HTML5 professional boilerplate](https://github.com/h5bp/html5-boilerplate)
