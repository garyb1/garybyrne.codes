---
title: Getting Started with Variable Fonts
date: 2021-02-02
---

Before we check out some code, let us briefly discuss what variable fonts are.

## OpenType Font Variations

[OpenType font variations](https://docs.microsoft.com/en-us/typography/opentype/spec/otvaroverview) allow us to create a single font file that contains a variety of fonts from a single typeface.
Variable fonts are fonts that use [OpenType]((https://docs.microsoft.com/en-us/typography/opentype/spec/otvaroverview) font variations. By using variable fonts, we now have access to every variation of a given typeface in a single file, reducing the number of font files considerably. We no longer need a font file for every single style, weight, or even width.

### Font Variation Settings and the Variation Axis

With variable fonts, one of the main concepts is an [axis of variation](https://developer.mozilla.org/en-US/docs/Web/CSS/font-variation-settings). The [axis of variation](https://developer.mozilla.org/en-US/docs/Web/CSS/font-variation-settings)
is a range of values to describe a specific aspect of a typeface.
For example, a _'slant' axis _ describes how much slant should exist in letterforms, or a _'wght' axis_ describes how much weight exists in letterforms.

Typeface designers have the ability to use five registered axes of variations.

These are:

-

1. Weight
2. Width
3. Slant
4. Italic
5. Optical size.

These five axes were added to the [specification](https://drafts.csswg.org/css-fonts-4).
Designers also have the ability to add custom axes for their variable font.

### Loading a Variable Font.

I [wrote](Link) about loading a variable font that uses [FontFaceObserver](https://github.com/bramstein/fontfaceobserver). This can be used to optimize your font loading.

#### Specify a font using @ [**_font-face_**](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face) CSS rule

```css
@font-face {
	font-family: "Vollkorn"; // typeface we are using
	src: url("https://s3-us-west-2.amazonaws.com/s.cdpn.io/1312918/Vollkorn-VariableFont_wght.ttf")
		format("truetype"); // we have a .ttf font file, support Safari, Android, iOS
	font-display: swap; //browser use fallback font to display content until custom font has downloaded
}
```

#### Use font-family and set **font-variation-settings**

Let us change the the font-weight by using font-variation setttings. If we set the weight to 400,
it will look like the following:

```css
h1 {
	font-family: "Vollkorn", serif;
	font-variation-settings: "wght" 400; // see wakamaifondue to get min and max values
}
```

![Screenshot 2021-01-29 at 17.51.06.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1611942677052/ydEiRrHdJ.png)

Now if we decide to change it to the max weight, which is 900, we will see:

```css
h1 {
	font-family: "Vollkorn", serif;
	font-variation-settings: "wght" 900; // see wakamaifondue to get min and max values
}
```

![Screenshot 2021-01-29 at 17.54.04.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1611942854007/iQkBoDFUN.png)

We can perform animation by changing the font-variation-settings:

```css
@keyframes scale {
	0% {
		font-variation-settings: "wght" 400;
	}
	50% {
		font-variation-settings: "wght" 900;
	}
	100% {
		font-variation-settings: "wght" 400;
	}
}
```

The result is a fun animating variable font:

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="html,result" data-user="garyb1" data-slug-hash="qBOMmNV" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Vollkorn VF loaded with FontFaceObserver">
  <span>See the Pen <a href="https://codepen.io/garyb1/pen/qBOMmNV">
  Vollkorn VF loaded with FontFaceObserver</a> by Gary Byrne (<a href="https://codepen.io/garyb1">@garyb1</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

#### Support for Older Browsers

Check [Can I Use](https://caniuse.com/?search=variable%20fonts) for variable font support.

You can check if the browser supports font variation settings:

```css
@supports (font-variation-settings: normal) {
	html {
		font-family: "SourceSansVariable", sans-serif;
		font-variation-settings: "wght" 900;
	}
}
```

You can also check if the browser does not support font variation settings:

```css
@supports not (font-variation-settings: normal) {
	html {
		font-family: sans-serif;
	}
}
```

### Finding Variation Axes - Wakamaifondue

A service that I use is [wakamaifondue](https://wakamaifondue.com/), which provides us with tonnes of information about an uploaded font file. When I uploaded a **DecovarAlpha** variable font to [wakamaifondue](https://wakamaifondue.com/), I could see it provides me with a short summary about that font. It also provides an interactive area where you can change the variations of axes and see the available characters for that font.

> This is a TrueType variable font with 114 characters. It has 15 axes and 0 named instances. It has no layout features.

I wrote a [tweet](https://twitter.com/garybyrne1995/status/1278632855699103745) showing how useful this can be. You can see it below:

%[https://twitter.com/garybyrne1995/status/1278632855699103745]

## Benefits of Variable Fonts

**1. Single File**

As we are only loading one single variable font file which includes all variations of a typeface, it reduces the number of HTTP requests considerably.

Let us pretend we are dealing with static fonts and we decided to use the [Poppins typeface](https://fonts.google.com/specimen/Poppins). If we needed every font variation of Poppins, we need to make a request for every file. If we need 'Poppins Regular', 'Poppins Bold', and 'Poppins Bold Italic', then we have three font files to deal with.

**2. Full Variation Access**

With variable fonts, we have access to the full variation settings for that typeface, meaning we can do more with our type and really make the most out of our designs.

**3. Fluid Type Animations **

We have the ability to animate between the different variations in our file, which means we have endless possibilities for animation.

### Codepen Examples

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="css,result" data-user="garyb1" data-slug-hash="LYpJVPO" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Variable Fonts - Animating Plex Sans VF">
  <span>See the Pen <a href="https://codepen.io/garyb1/pen/LYpJVPO">
  Variable Fonts - Animating Plex Sans VF</a> by Gary Byrne (<a href="https://codepen.io/garyb1">@garyb1</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>

_A codepen example by Gary Byrne showing how to animate a variable font._

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="css,result" data-user="mandymichael" data-slug-hash="dJZQaz" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="CSS only Variable font demo using Decovar Regular">
  <span>See the Pen <a href="https://codepen.io/mandymichael/pen/dJZQaz">
  CSS only Variable font demo using Decovar Regular</a> by Mandy Michael (<a href="https://codepen.io/mandymichael">@mandymichael</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>

_A codepen example by Mandy Michael showing how to animate a variable font._

### References

A list of useful links that really helped me understand variable fonts.

[OpenType Specification](https://docs.microsoft.com/en-us/typography/opentype/spec/otvaroverview)

[Frontend Masters Course By Jason Pamental](https://frontendmasters.com/courses/responsive-typography-v2/)

[Mandy Michael Codepen](https://codepen.io/mandymichael)

[MDN Variable Fonts Guide](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Fonts/Variable_Fonts_Guide)

[Variable Fonts List](https://v-fonts.com/)

---
