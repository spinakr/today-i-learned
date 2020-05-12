# Manipulating child element in the DOM

You can pass a reference to an element (e.g. a <div>), which makes it possible to manipulate the DOM of the child from the containing element.

```typescript
....

const divRef = React.createRef<HTMLDivElement>();
const setScroll = (scrollPosition: number) => { divRef.current.scrollLeft = scrollPosition }

return (<div ref={divRef}> </div>)

```

Calling the setScroll function from the parent, would set the scroll position of the child div, if it is scrollable.


