---
title: Font Loading with Font Face Ob­server
date: 2021-02-02
---

# Font Loading with Font Face Ob­server

## What is Font Face Observer

[Font Face Observer](https://fontfaceobserver.com/) is a really great web font loader created by [Bram Stein](https://www.bramstein.com/) that provides us with a promised based way to control our font loading. It will know when web fonts have been loaded which gives us complete control to customize the font loading experience as we want.

With web fonts, we decide if we want to self-host or load from external services, so it can be difficult to control our browser's font loading behavior. We need to be careful with problems in our loading like [FOIT](https://www.zachleat.com/web/webfont-glossary/#foit) or [FOUT](https://www.zachleat.com/web/webfont-glossary/#fout).

## Font-Display Swap

```css
@font-face {
	font-family: "Font Family";
	src: url("....url.to.font") format("format");
	font-display: swap;
}
```

According to the MDN [Docs](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/font-display):

> If the font face is not loaded, any element attempting to use it must render a fallback font face. If the font face successfully loads during this period, it is used normally. This introduces [FOUT](https://www.zachleat.com/web/webfont-glossary/#fout) which may cause a significant layout shift.

We want to have greater flexibility with our fallback font. With FOUT, it gives us that fallback font but doesn't give us a way to tweak the harsh viewing when layout shift occurs. By using Font Face Observer, it can provide us with a way of controlling this.

## Installation of Font Face Observer

Using [npm](https://www.npmjs.com/package/fontfaceobserver)

```bash
   npm install fontfaceobserver -S
```

Using yarn

```bash
   yarn add fontfaceobserver
```

If you are not using [node](https://nodejs.org/en/), you can add it by linking the script file into the head of the document.

```html
// locally
<script src="js/vendor/fontfaceobserver.js"></script>
//or from CDN
<script src="https://cdnjs.cloudflare.com/ajax/libs/fontfaceobserver/2.1.0/fontfaceobserver.js"></script>
```

## Roboto Example

Let's grab the Roboto 'Regular', 'Medium' and 'Bold' from [Google Fonts](https://fonts.google.com/specimen/Roboto?sidebar.open=true&selection.family=Roboto:wght@300;400;700).

Next, let's load our fonts in our CSS and point to their directory:

```css
@font-face {
	font-family: "Roboto";
	font-weight: 400;
	src: url("../fonts/Roboto-Regular.ttf") format("truetype");
	font-display: swap;
}

@font-face {
	font-family: "Roboto";
	font-weight: 500;
	src: url("../fonts/Roboto-Medium.ttf") format("truetype");
	font-display: swap;
}

@font-face {
	font-family: "Roboto";
	font-weight: 700;
	src: url("../fonts/Roboto-Bold.ttf") format("truetype");
	font-display: swap;
}
```

Now we can start using FontFace Observer.
Create a script tag in the head of our document below where we brought in FontFace Observer.

```js
document.documentElement.className += " roboto-inactive";
const RobotoFont = new FontFaceObserver("Roboto", {});

RobotoFont.load().then(function () {
	document.documentElement.classList.remove("roboto-inactive");
	document.documentElement.classList.add("roboto-active");
	sessionStorage.foutFontsLoaded = true;
});
```

What we are doing here is appending some classes to the root of our document whenever our RobotoFont promise resolves. The promise will resolve when the font has loaded. We can use the `roboto-inactive` class in our CSS to style our fallback font however we want. This class will only be present when the font fails to load.

If we wanted to load multiple fonts we use `Promise.all` which will make sure we wait for both promises to resolve before executing our important code.

```js
document.documentElement.className += " wf-inactive";
const robotoFont = new FontFaceObserver("Roboto", {});
const poppinsFont = new FontFaceObserver("PoppinsFont", {
	weight: 700, // we can be more precise
	style: italic,
});

Promise.all([robotoFont.load(), poppinsFont.load()]).then(function () {
	// Important code here.... add a class or remove, etc.
});

// We can also provide a second function to
// run when the font is not available

Promise.all([robotoFont.load(), poppinsFont.load()]).then(
	function () {
		console.log("font is available");
		// Important code here.... add a class or remove, etc.
	},
	function () {
		console.log("font is not available");
		// do something here ...
	}
);
```

In our CSS, we can now add some styles to clean up our fallback font or add existing styles to our
loaded font.

```css
body {
	font-family: "Roboto", Arial, Helvetica, sans-serif;
}

.wf-inactive body {
	font-family: Arial, Helvetica, sans-serif;
}

.wf-inactive h1,
.wf-inactive h2,
.wf-inactive h3 {
	// you could also apply the font-family to specific
	// elements if we had a heading font for example.
}

.wf-inactive p {
	// apply these styles to a pargraph using our fallback font
	line-height: 1.2;
	letter-spacing: -0.5px;
}

// ... more styles here
```

## Support

### Promise Support

In the FontFace Observer [README](https://www.npmjs.com/package/fontfaceobserver) it says:

> FontFace Observer uses Promises in its API, so for browsers that do not support promises, you'll need to include a polyfill. If you use your own Promise polyfill you just need to include fontfaceobserver.standalone.js in your project. If you do not have an existing Promise polyfill you should use fontfaceobserver.js which includes a small Promise polyfill. Using the Promise polyfill adds roughly 1.4KB (500 bytes gzipped) to the file size.

### Browser Support

You can see the browser support within the package [README](https://www.npmjs.com/package/fontfaceobserver#browser-support).

## References

- [FontFace Observer NPM](https://www.npmjs.com/package/fontfaceobserver)

-

[Github](https://github.com/bramstein/fontfaceobserver)

- [FontFace Observer Website](https://fontfaceobserver.com/)

- [Jason Pamental Frontend Masters Course](https://frontendmasters.com/courses/responsive-typography-v2)

- [Bram Stein](https://www.bramstein.com/writing/web-font-loading-patterns.html)
