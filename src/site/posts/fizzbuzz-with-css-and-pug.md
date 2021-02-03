---
title: Fizzbuzz with CSS and Pug
date: 2021-02-02
---

Back in 2017, I entered a frontend interview and I was asked to write some code on a whiteboard. I was given the [Fizzbuzz](https://www.geeksforgeeks.org/fizz-buzz-implementation/) problem. Without thinking I immediately went for JavaScript. Recently I learned about CSS counters and that got me thinking. What if we can do it with CSS?

In this article, I am going to show you two easy ways to solve the Fizzbuzz problem with CSS.

## What is the Fizzbuzz question?

> Write a program that prints the numbers from 1 to 100.
> For multiples of `3` print `Fizz` instead of the number,
> for the multiples of `5` print `Buzz` and for
> multiples of both `5 and 3` print `FizzBuzz`

## Solution 1: Using CSS Counters

### We can loop in [Pug](https://pugjs.org/api/getting-started.html)

Loop for 1 to 100 and create an HTML element. In this example, we are creating a div.
We will use each div to hold the counter.

```html
- const range = 100; - for(let i = 1; i <= range; i++) .fizz
```

We are using a for loop here. We could also write an [each](https://pugjs.org/language/iteration.html)
iteration such as `each _ in Array(100)` instead.

### Using CSS counters

To get started, we must first create a `counter-reset`. This will initialize our CSS counter, giving it a name and setting its value. By default, the value is 0.

```css
body {
	counter-reset: fizz;
	// counter initialized with value of 0, same as saying "counter-reset: fizz 0;"
}
```

Once a counter has been created, we can use `counter-increment` to increment its value.
For every `fizz` div, let's update the counter.

```css
.fizz {
	counter-increment: fizz;
}
```

Next, we will set the content for our divs. We can utilize CSS `before` and `after` [pseudo-elements](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements)
to set the content of the div.

If the div is a multiple of 3, set its `::before` content to 'Fizz '. If the div is a multiple of 5, set is `::after`
content to 'Buzz'. If a div is a multiple of both 3 and 5, the before and after pseudo-elements will combine to form `Fizz Buzz`.

```css
.fizz:nth-child(3n)::before {
	content: "fizz ";
}

.fizz:nth-child(5n)::after {
	content: "buzz";
}
```

Now we want to set the counter value for those other divs that are not a multiple of 3 or 5.
To set the `::before` content of the div to use the counter value, we use `counter`.

```css
.fizz::before {
	content: counter(fizz);
}

.fizz:nth-child(3n)::before {
	content: "fizz ";
}

.fizz:nth-child(5n)::after {
	content: "buzz";
}
```

This gives us the following:

![Screenshot 2021-02-03 at 12.18.44.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1612354739834/hpFgTA0nq.png)

Note how the divs with a multiple of 5 and not a multiple of 3 still get the number applied.
To fix this we can use the [not](https://developer.mozilla.org/en-US/docs/Web/CSS/:not) pseudo-class.
When the div is `not` a multiple of 5, then set the before content to the counter number.

```css
.fizz:not(:nth-child(5n))::before {
	content: counter(fizz);
}
.fizz:nth-child(3n)::before {
	content: "fizz ";
}

.fizz:nth-child(5n)::after {
	content: "buzz";
}
```

Here is a working example in codepen.

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="scss,result" data-user="garyb1" data-slug-hash="LYbpRbG" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Fizzbuzz 3 - Ordered List">
  <span>See the Pen <a href="https://codepen.io/garyb1/pen/zYovrVB">
  Fizzbuzz 1</a> by Gary Byrne (<a href="https://codepen.io/garyb1">@garyb1</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>

### Refactoring our Pug

In some cases, an interviewer might ask to change your code to work for a different range instead of 1 to 100. We can write a Pug mixin to take a min and max value.

```pug
mixin fizzbuzz(min, max)
	- for(let i = min; i <= max; i++)
		.fizz

+fizzbuzz(1, 100)

```

Here is a working codepen example:

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="scss,result" data-user="garyb1" data-slug-hash="LYbpRbG" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Fizzbuzz 3 - Ordered List">
  <span>See the Pen <a href="https://codepen.io/garyb1/pen/RwoWRNz">
  Fizzbuzz 2</a> by Gary Byrne (<a href="https://codepen.io/garyb1">@garyb1</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>

### Browser Support

At the time of writing, CSS Counters are supported in most major browsers. See [Can I Use](https://caniuse.com/css-counters) for more information.

## Solution 2: Using an Ordered List

Let's copy the same pug from the last example.

```pug
mixin fizzbuzz(min, max)
	ol
		- for(let i = min; i <= max; i++)
			li

+fizzbuzz(1, 100)

```

So we have an [ordered list](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/ol) here which returns a list of numbered elements. So if we render 100 list items in that ordered list, we have the numbers already available to view. Although it makes no sense semantically, it is just to show how we might do this.

In this ordered list, we can set the [list-style-position](https://developer.mozilla.org/en-US/docs/Web/CSS/list-style-position) of each list item to `inside`. This will set the position of everything inside, relative to the list which created the nice alignment. We then just set the list style to none to remove the numbering on those items that need the text.

```scss
//scss

li {
	list-style-position: inside;

	&:nth-child(3n),
	&:nth-child(5n) {
		list-style-type: none;
	}

	&:nth-child(3n)::before {
		content: "Fizz";
	}

	&:nth-child(5n)::after {
		content: " Buzz";
	}
}
```

Here is a working example:

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="html,result" data-user="garyb1" data-slug-hash="LYbpRbG" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Fizzbuzz 3 - Ordered List">
  <span>See the Pen <a href="https://codepen.io/garyb1/pen/LYbpRbG">
  Fizzbuzz 3 - Ordered List</a> by Gary Byrne (<a href="https://codepen.io/garyb1">@garyb1</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>
