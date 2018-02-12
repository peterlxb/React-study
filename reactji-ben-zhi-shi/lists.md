# Lists and Keys

Doc中第一个示例

![](/assets/Screenshot %2821%29.png)

在li中要添加key ,否则在devtools中会出现warning

## Keys {#keys}

> Keys help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity:

Keys only make sense in the context of the surrounding array.

if you [extract](https://reactjs.org/docs/components-and-props.html#extracting-components) a`ListItem`component, you should keep the key on the`<ListItem />`

elements in the array rather than on the`<li>`element in the`ListItem`itself.

**Example: Incorrect Key Usage**

![](/assets/Screenshot %2822%29.png)

**Example: Correct Key Usage**

![](/assets/Screenshot %2823%29.png)

### Embedding map\(\) in JSX {#embedding-map-in-jsx}

JSX allows[embedding any expressions](https://reactjs.org/docs/introducing-jsx.html#embedding-expressions-in-jsx)in curly braces so we could inline the`map()`result:

![](/assets/Screenshot %2824%29.png)  












