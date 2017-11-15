---
layout: post
title: "They say stop using var. But why"
author: "Liren Yeo"
date: 2017-11-15 10:20:15 +0800
tags: [javascript, es6]
---

# They say stop using `var`. But why?
[ES6 or EcmaScript 6 or EcmaScript 2015](https://codeburst.io/javascript-wtf-is-es6-es8-es-2017-ecmascript-dca859e4821c) are the same thing. It is a significant update to JavaScript with many useful feature. Among the notable ones include keywords `const` and `let`.

The [difference between `const` and `let`](http://wesbos.com/let-vs-const/) is simple.

`let` allows you to reassign a variable:
```
let me = "awesome";
me = "awesome and smart";
// doesn't throw error
```

While `const` variable cannot be reassigned:
```
const me = "awesome";
me = "pretty lame";
// TypeError: Assignment to constant variable.
// FactError: Assignment of wrong fact
```

The /TL;DR/ of this article is to forget about `var` and use `const` and `let` instead. However if you care about the well-being of your JS journey, please continue reading to find out why.

Moving forward I will only refer to `let`, but you should know that `const` has the same nature applied.

---

### Hoisting
> Hoisting is a JavaScript mechanism where variables and function declarations are moved to the top of their scope before code execution.

In JavaScript, a variable can be used before it has been declared thanks to (or not?) *hoisting*.

Usually, calling an undeclared variable will throw /ReferenceError/
```
console.log(milo); // 'ReferenceError: milo is undefined'
```

But not the following:
```
console.log(milo); // Logs 'Undefined'
var milo = "taste good with biscuits";
```

This is because JavaScript interpreter "looks ahead" to find all the variable declarations and "hoists" them to the top of the function. Under the hood, the code looks like this:
```
var milo; // Hoisted up here secretly
console.log(milo); // Logs 'Undefined', which makes sense now
milo = "whatever";
```

It could lead to an unpleasant surprise:
```
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

So how do we avoid the unpleasant surprise from above examples? Use `let`! Unlike `var`, `let` is [in fact not hoisted](https://dmitripavlutin.com/variables-lifecycle-and-why-let-is-not-hoisted/) (Also depending on how you define ‘hoisting’) [1]:
```
let x = 1;
(function() {
  console.log(x); //
})();
```

[1] Hoisting is a made-up concept to describe behavior of JS compiler. Conceptually you can still say `let` gets hoisted, but the “hoisted” `let` variables are just declared, not initialized. Therefore compiler will not silently give us an unexpected `undefined`, but throws error at us instead.
[Github user getify explains it perfectly well here](https://github.com/getify/You-Dont-Know-JS/issues/767#issuecomment-227946671)


### Function vs Block Scoping
```
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
```
function createButtons() {
  // var number, button, size;
  // They all get hoisted up here!
  for (var number = 1; number < 5; number++) {
    var button = document.createElement("button");
    var size = (number * 100) + "%";
    button.innerText = size;
    document.body.appendChild(button);

      button.addEventListener("click", function() {
      document.body.style.fontSize = size;
      })
  }
  //var size = "10%";
}

document.write('The brown fox jumped over the lazy dog <br>')
createButtons()
```

As the result, all `number`, `button` and `size` variables are always pointing to the same reference! Every button becomes 400%!

With that in mind, can you predict the consequence if  `var size=“10%"` was to be uncommented? Yep, all button changes font size to 10%!

#### Solution?
Use `let` or `const`:
```
for (…) {
  let button = ...
  let size = ...
}
```
Block-scoped `button` and `size` variables will not be known outside the for-loop, and they will not be hoisted out of the for-loop as well!

And the result is as expected:


### Conclusion
