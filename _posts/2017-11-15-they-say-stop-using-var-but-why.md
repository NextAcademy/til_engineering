---
layout: post
title: "They say stop using var. But why?"
author: "Liren Yeo"
date: 2017-11-15 10:20:15 +0800
tags: [javascript, es6]
preview: http://cactusthemes.com/blog/wp-content/uploads/2017/10/es6-var-let-and-const-explained-940x588.jpg
---

[ES6 or EcmaScript 6 or EcmaScript 2015](https://codeburst.io/javascript-wtf-is-es6-es8-es-2017-ecmascript-dca859e4821c) are the same thing. It is a significant update to JavaScript with many useful feature. Among the notable ones include keywords `const` and `let`.

The [difference between `const` and `let`](http://wesbos.com/let-vs-const/) is simple.

`let` allows you to reassign a variable:
```js
let me = "awesome";
me = "awesome and smart";
// doesn't throw error
```

While `const` variable cannot be reassigned:
```js
const me = "awesome";
me = "pretty lame";
// TypeError: Assignment to constant variable.
// FactError: Assignment of wrong fact
```

The _TL;DR_ of this article is to forget about `var` and use `const` and `let` instead. However if you care about the well-being of your JS journey, please continue reading to find out why.

Moving forward I will only refer to `let`, but you should know that `const` has the same nature applied.

---

## Hoisting
> Hoisting is a JavaScript mechanism where variables and function declarations are moved to the top of their scope before code execution.

In JavaScript, a variable can be used before it has been declared thanks to (or not?) **hoisting**.

Usually, calling an undeclared variable will throw *ReferenceError*
```js
console.log(milo); // 'ReferenceError: milo is undefined'
```

But not the following:
```js
console.log(milo); // Logs 'Undefined'
var milo = "taste good with biscuits";
```

This is because JavaScript interpreter "looks ahead" to find all the variable declarations and "hoists" them to the top of the function. Under the hood, the code looks like this:
```js
var milo; // Hoisted up here secretly
console.log(milo); // Logs 'Undefined', which makes sense now
milo = "whatever";
```

It could lead to an unpleasant surprise:
```js
var height = "100cm";
(function () {
    // There is an invisible "var height;" here
    console.log("Last year I was " + height);
    var height = "170cm";
    console.log("This year I am " + height);
})();

/* Output */
// Last year I was undefined (SUPRISE!)
// This year I am 170cm
```

So how do we avoid the unpleasant surprise from above examples? Use `let`! Unlike `var`, `let` is [in fact not hoisted](https://dmitripavlutin.com/variables-lifecycle-and-why-let-is-not-hoisted/) (Also depending on how you define ‘hoisting’)*

>Hoisting is a made-up concept to describe behavior of JS compiler. Conceptually you can still say `let` gets hoisted, but the “hoisted” `let` variables are just declared, not initialized.
Therefore compiler will not silently give us an unexpected `undefined`, but throws error at us instead.
[Github user getify explains it perfectly well here](https://github.com/getify/You-Dont-Know-JS/issues/767#issuecomment-227946671)


## Function vs Block Scoping
```js
const someFunction = () => {
  // function scope
  if (true) {
    // block scope
  }
  // function scope again
}
```

`var` is function scoped.
`let` is block scoped.

Take a look at the following function. It is supposed to loop 4 times to create 4 different buttons that can change your font size to 100%, 200%, 300% and 400% respectively.

Due to function-scoping, all `var` variables inside the for-loop get hoisted to the top as well:

<p data-height="400" data-theme-id="0" data-slug-hash="QOMQoM" data-default-tab="js,result" data-user="lirenyeo" data-embed-version="2" data-pen-title="var Function Scoping" class="codepen">See the Pen <a href="https://codepen.io/lirenyeo/pen/QOMQoM/">var Function Scoping</a> by Liren (<a href="https://codepen.io/lirenyeo">@lirenyeo</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script src="https://production-assets.codepen.io/assets/embed/ei.js"> </script>


As the result, all `number`, `button` and `size` variables are always pointing to the same reference! Every button becomes 400%!

With that in mind, can you predict the consequence if  `var size=“10%"` was to be uncommented?
Yep, all button changes font size to 10%!

#### Solution?
Use `let` or `const`:

<p data-height="400" data-theme-id="0" data-slug-hash="VrzXKe" data-default-tab="js,result" data-user="lirenyeo" data-embed-version="2" data-pen-title="let Block Scoping" class="codepen">See the Pen <a href="https://codepen.io/lirenyeo/pen/VrzXKe/">let Block Scoping</a> by Liren (<a href="https://codepen.io/lirenyeo">@lirenyeo</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script src="https://production-assets.codepen.io/assets/embed/ei.js"> </script>


With that, block-scoped `button` and `size` variables will not be known outside the for-loop, and they will not be hoisted out of the for-loop as well.
If you're wondering how was this problem solved before ES6 was released, [through Closure](https://codepen.io/lirenyeo/pen/OOjZNo).

## Conclusion
>`let`/`const` is the new `var`

Function scoping along with hoisting properties can cause unpredictable behavior in our code.
Always use `const` if possible, consider `let` if variable reassignment is required.
