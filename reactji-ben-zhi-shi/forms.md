# Forms

HTML form elements work a little bit differently from other DOM elements in React, because form elements naturally keep some internal state. For example, this form in plain HTML accepts a single name:

![](/assets/Screenshot %2825%29.png)

## Controlled Components {#controlled-components}

> **In HTML, form elements such as &lt;input&gt; &lt;textarea&gt; and &lt;select&gt; typically maintain their own state and update it based on user input. In React, mutable state is typically kept in the state property of components, and only updated with **[`setState()`](https://reactjs.org/docs/react-component.html#setstate)
>
> **An input form element whose value is controlled by React in this way is called a “controlled component”.**

## The textarea Tag {#the-textarea-tag}

In React, a`<textarea>`uses a`value`attribute instead. This way, a form using a

`<textarea>`can be written very similarly to a form that uses a single-line input:

## ![](/assets/Screenshot %2826%29.png)The select Tag

React, instead of using this`selected`attribute, uses a`value`attribute on the root`select`tag. This is more convenient in a controlled component because you only need to update it in one place. For example:![](/assets/Screenshot %2827%29.png)Handling Multiple Inputs

When you need to handle multiple controlled`input`elements, you can add a`name`attribute to each element and let the handler function choose what to do based on the value of`event.target.name`

.

