---
title: Creating a Xylophone using CSS and Pug
date: 2021-02-02
---

Let's create a simple xylophone with some CSS and Pug. The xylophone sounds I will be using in this blog can be found [here](https://freesound.org/people/DANMITCH3LL/sounds/). The resulting codepen can be found [here](https://codepen.io/garyb1/pen/f59c52a7c9d00dd1d230491908d1f0e5).

## What will it look like?

We will use an unordered list of buttons to represent the xylophone. The finished product will look like [this](https://cdn.hashnode.com/res/hashnode/image/upload/v1612281745582/pqxy41fe_.png).

## Creating the Xylophone

To get up and running quickly with [Pug](https://pugjs.org/api/getting-started.html) you can open up a [codepen](https://codepen.io). In your HTML settings, click Pug as your HTML preprocessor.

### Writing our Pug

Let's create an unordered list of buttons using an array of xylophone notes.

```html
- const notes = ['c', 'd', 'e', 'f', 'g', 'a', 'b', 'c2']; main
ul.xylophone(role="list") each note, index in notes li button #{note}
```

This produces the following HTML:

```html
<main>
	<ul class="xylophone" role="list">
		<li>
			<button>c</button>
		</li>
		<li>
			<button>d</button>
		</li>
		<li>
			<button>e</button>
		</li>
		// ..... the rest
	</ul>
</main>
```

I added `role="list"` to the `ul` to overcome a semantics [issue](https://www.scottohara.me/blog/2019/01/12/lists-and-safari.html) in voiceover and safari.

### Let's style our xylophone with CSS.

First, let's reset `box-sizing` and position the content to the center of the page.

Alternatively, you can just import a CSS reset. I recommend the [modern CSS reset](https://piccalil.li/blog/a-modern-css-reset) by [Andy Bell](https://piccalil.li) but it's not necessary for this project.

```css
*,
*:before,
*:after {
	box-sizing: border-box;
}

body {
	min-height: 100vh;
	margin: 0;
	display: grid;
	place-items: center;
}
```

We can style our `ul` to be a [flex](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) container. Using the [attribute selector](https://developer.mozilla.org/en-US/docs/Web/CSS/Attribute_selectors) here just to open our CSS to other types of lists.

```css
[role="list"] {
	list-style: none;
	display: flex;
	justify-content: space-between;
	padding: 0;
}
```

This gives us:

![Screenshot 2021-02-02 at 14.29.02.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1612276157190/XUVu98SXr.png)

Now we can add some responsive sizing to our xylophone.
We will apply the [vmin](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Values_and_units) relative length unit using CSS [custom properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties).

```css
:root {
	--desired-size: 60;
	--coefficient: 1vmin;
	--size: calc(var(--desired-size) * var(--coefficient));
}
```

Let's update our list with the new sizing.

```css
[role="list"] {
	list-style: none;
	display: flex;
	justify-content: space-between;
	padding: 0;
	height: calc(1.5 * var(--size));
	width: calc(2.5 * var(--size));
}

li {
	width: 10%;
}

button {
	width: 100%;
	height: 100%;
}
```

Let's apply the backboards of the xylophone. We will be `absolutely` positioning these against our xylophone. To do this, we must first set `position: relative;` in our `[role="list"]` CSS.

```css
.xylophone:before,
.xylophone:after {
	content: "";
	position: absolute;
	z-index: -1; // hide these to the back, allow our buttons to appear in front
	background: black;
	height: 15%; // 15% of the overall xylophone height
	width: 100%;
}

.xylophone:before {
	top: 10%;
	transform: rotate(3deg);
}

.xylophone:after {
	bottom: 10%;
	transform: rotate(-3deg);
}
```

This gives us the following:

![Screenshot 2021-02-02 at 14.46.36.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1612277217481/iqd-cWp34.png)

### Hooking up our audio

Before we continue to style our xylophone, let's add some audio to it.

```html
main ul.xylophone(role="list") each note, index in notes li
button(onclick=`playNote('${note}')`) audio( data-key=`${note}`,
src=`https://s3-us-west-2.amazonaws.com/s.cdpn.io/1312918/${note}.wav` )
```

We have added a hidden audio element to be a child of the button. We can hook into this to play each note sound. I have added a `src` attribute to point to the different `wav` files. The `data-key` attribute will be used within our querySelector to help us find an audio element for each individual note. In this example, I have stored them on my codepen s3 bucket. Next, I will need to add some JavaScript to handle the `on click` logic.

At the bottom of your `pug` file, add the following script.

```js
script.
  function playNote(note) {
    const audioElement = document.querySelector(`audio[data-key="${note}"]`);
    audioElement.currentTime = 0;
    audioElement.play();
  }

```

### Cleaning up our xylophone

Let's add some color to our buttons:

```css
li:nth-child(1) button {
	background-color: pink;
}
li:nth-child(2) button {
	background-color: orange;
}
li:nth-child(3) button {
	background-color: yellow;
}
li:nth-child(4) button {
	background-color: lightgreen;
}
li:nth-child(5) button {
	background-color: green;
}
li:nth-child(6) button {
	background-color: skyblue;
}
li:nth-child(7) button {
	background-color: blue;
}
li:nth-child(8) button {
	background-color: rebeccapurple;
}
```

![Screenshot 2021-02-02 at 15.00.31.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1612278046628/QUX2QpIlx.png)

Next, let's add the knobs for each button:

```css
button {
	width: 100%;
	height: 100%;
	position: relative;
	border-radius: 5px;
}

button::before,
button::after {
	content: "";
	position: absolute;
	left: 50%;
	transform: translateX(-50%);
	height: 5%;
	width: 35%;
	border-radius: 50%;
	background-color: white;
	box-shadow: 0 10px 20px rgba(0, 0, 0, 0.19), 0 6px 6px rgba(0, 0, 0, 0.23);
}

button::before {
	top: 5%;
}

button::after {
	bottom: 5%;
}
```

Now we have a working xylophone. Here is a working version:

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="css,result" data-user="garyb1" data-slug-hash="eYBNyQp" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Xylophone with Sounds - Without Cleanup">
  <span>See the Pen <a href="https://codepen.io/garyb1/pen/eYBNyQp">
  Xylophone with Sounds - Without Cleanup</a> by Gary Byrne (<a href="https://codepen.io/garyb1">@garyb1</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

## Cleaning up our Xylophone

There is a number of things we can do to tidy up our component.

When we click a button, we can apply a class to show the sound is playing.
For the same button, we can also add an event listener to remove the class
when the [transitionend](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/transitionend_event) event is fired.
For this, we will remove the class when the `box-shadow` transition has ended.

Let's add a transition to our button and a nice `box-shadow` when the button is playing.

```css
button {
	//..other styles
	transition: all 1s ease; //we can listen for the box shadow end
}

button.playing {
	border-color: #ffc600;
	box-shadow: 0px -10px 1rem #ffc600;
}
```

Add the `data-key` attribute with the value of the note to our button.

```js
button((onclick = `playNote('${note}')`), (data - key = `${note}`));
```

Then apply the `.playing` class when we click the button.

```js
script.
  function playNote(note) {
    const audioElement = document.querySelector(`audio[data-key="${note}"]`);
    const buttonElement = document.querySelector(`button[data-key="${note}"]`);
    buttonElement.classList.add('playing');
    audioElement.currentTime = 0;
    audioElement.play();
  }
```

Add our `transitionend` event listener:

```js
script.
  function removeStyles(e) {
    if (e.propertyName !== 'box-shadow') return;
    e.target.classList.remove('playing');
  }

  function playNote(note) {
    const audioElement = document.querySelector(`audio[data-key="${note}"]`);
    const buttonElement = document.querySelector(`button[data-key="${note}"]`);
    buttonElement.classList.add('playing');
    buttonElement.addEventListener('transitionend', removeStyles);
    audioElement.currentTime = 0;
    audioElement.play();
  }

```

Now we have a nice transition on our xylophone:

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="css,result" data-user="garyb1" data-slug-hash="VwmLxgr" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Xylophone with Sounds - With Cleanup">
  <span>See the Pen <a href="https://codepen.io/garyb1/pen/VwmLxgr">
  Xylophone with Sounds - With Cleanup</a> by Gary Byrne (<a href="https://codepen.io/garyb1">@garyb1</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>

We can do a lot more with Pug. I created another example to show how we can pass values from pug
into our CSS to use as custom properties. I randomly generate the hue for our background color each time, and I can pass the index which I use to make each button smaller in height and create a nice horizontal rhythm. In the pen below, you can also see how I used the `kbd` element instead of the `button` element to listen for keyboard events.

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="css,result" data-user="garyb1" data-slug-hash="XWmLXgd" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Xylophone with Sounds">
  <span>See the Pen <a href="https://codepen.io/garyb1/pen/XWmLXgd">
  Xylophone with Sounds</a> by Gary Byrne (<a href="https://codepen.io/garyb1">@garyb1</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>

## References

- [Jhey Tompkins Twitch](https://www.twitch.tv/jh3yy/about)

- [Jhey Tompkins Codepen](https://codepen.io/jh3y)

- [My Codepen](https://codepen.io/garyb1)
