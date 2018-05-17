原文地址

[https://reactjs.org/blog/2015/12/18/react-components-elements-and-instances.html\#fn-1](https://reactjs.org/blog/2015/12/18/react-components-elements-and-instances.html#fn-1)

## Elements Describe the Tree {#elements-describe-the-tree}

**An element is a plain object **_**describing **_**a component instance or DOM node and its desired properties.**

An element is not an actual instance. Rather, it is a way to tell React what you _want _to see on the screen. It’s just an immutable description object with two fields:

type: \(string \| ReactClass\) and props: Object

### DOM Elements {#dom-elements}

```
{
  type: 'button',
  props: {
    className: 'button button-blue',
    children: {
      type: 'b',
      props: {
        children: 'OK!'
      }
    }
  }
}
```

This element is just a way to represent the following HTML as a plain object:

```
<button class='button button-blue'>
  <b>
    OK!
  </b>
</button>
```

### Component Elements {#component-elements}

**An element describing a component is also an element, just like an element describing the DOM node. They can be nested and mixed with each other.**



