# Handling Events

来自官方文档

Handling events with React elements is very similar to handling events on DOM elements. There are some syntactic differences:

* React events are named using camelCase, rather than lowercase.
* With JSX you pass a function as the event handler, rather than a string.

Another difference is that you cannot return `false`to prevent default behavior in React. You must call `preventDefault`

explicitly. For example, with plain HTML, to prevent the default link behavior of opening a new page, you can write:

![](/assets/Screenshot %2817%29.png)

In React

![](/assets/Screenshot %2818%29.png)

When you define a component using an [ES6 class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) , a common pattern is for an event handler to be a method on the class. For example, this`Toggle`component renders a button that lets the user toggle between “ON” and “OFF” states:

![](/assets/Screenshot %2819%29.png)

In JavaScript, class methods are not [bound](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind) by default. If you forget to bind `this.handleClick`

and pass it to `onClick`,`this`will be`undefined`when the function is actually called.

If you aren’t using class fields syntax, you can use an [arrow function](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) in the callback:

![](/assets/Screenshot %2820%29.png)

官方文档对这种模式的说明

> The problem with this syntax is that a different callback is created each time the`LoggingButton`renders. In most cases, this is fine. However, if this callback is passed as a prop to lower components, those components might do an extra re-rendering. We generally recommend binding in the constructor or using the class fields syntax, to avoid this sort of performance problem.



