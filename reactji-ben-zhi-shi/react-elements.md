# JSX简介

![](/assets/Screenshot %283%29.png)

上面的代码在React 中称之为JSX.用来描述UI

> JSX produces React “elements”.

![](/assets/Screenshot %284%29.png)

In the DevTools

The element is just a plain javascript object with a bunch of different properties

It has key, props,type.

The React createElement\(\) method has the following signature

> React.createElement\(/\* **type** \*/, /\* **props** \*/, /\* **content** \*/\)

* **type** either a string or a React Component\(this can be a string with any existing HTML \(p, span, or header\) or pass a React Component\)
* **props** either null or an object\(this is an object of HTML attributes and custom data about the element\)
* **content** null , a string ,a React element or a React Component

# VirtualDom

来自一篇文章的定义

> **It's just objects that describe real Dom nodes.**
>
> **So when we call React.createElement, we haven't actually created anything in the Dom yet.**
>
> **It's not until we say render that the browser actually creates real Dom elements**

关于React Elements

> **React elements are **[**immutable**](https://en.wikipedia.org/wiki/Immutable_object)**. Once you create an element, you can’t change its children or attributes. An element is like a single frame in a movie: it represents the UI at a certain point in time.**



