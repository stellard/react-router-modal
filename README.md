<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### Table of Contents

-   [react-router-modal](#react-router-modal)
-   [Accessibility](#accessibility)
-   [Examples](#examples)
-   [ModalContainer](#modalcontainer)
-   [ModalRoute](#modalroute)
-   [Modal](#modal)
-   [ModalLink](#modallink)

## react-router-modal

A simple way to handle showing modals with react-router version 4.

Component docs: <https://github.com/davidmfoley/react-router-modal/blob/master/docs/react-router-modal.md>

Examples: <https://davidmfoley.github.io/react-router-modal-examples>

### Installation

Install using yarn or npm.

`npm install react-router-modal --save`

or

`yarn add react-router-modal`

You will also need to install some other modules as peers.

TBH, if you are looking at this package you probably already have these, but you might want to check for version compatibility.

`react-router-dom` _version 4_

`react` & `react-dom`, version 15 or higher

For ex: `yarn add react-router-dom react react-dom`.

### Getting started

To add react-router-modal to your app:

1.  Include the CSS for react-router-modal, found in this package at `css/react-router-modal.css`

If you are using webpack, you can do this:

`import 'react-router-modal/css/react-router-modal.css';`

Note that you can also copy this file or create your own css and specify your own class names.

2.  Add a `<ModalContainer />` to your react application. This is where any shown modals are added to the DOM.

See also: <https://github.com/davidmfoley/react-router-modal-examples/blob/master/src/App.js#L42>

3.  Add a `<ModalRoute />` to test your setup:

```javascript
<ModalRoute path='/modal-test' parentPath='/'>
  Hello
</ModalRoute>
```

4.  Navigate to /modal-test in your app. You should see a Modal with the contents "Hello".

### Gotchas

#### My modals are not showing at all

1.  Did you render a ModalContainer?

2.  Did you include the CSS to style the modals and their backdrops?

#### I see my modal content but the component "behind" it is not rendering.

To display a modal component "on top" of another component, _both_ routes (the ModalRoute and the Route that renders the other component) must match.

If you are seeing modal content but the component that you expect to see "behind" the modal is not rendering, you should check for the following:

1.  Did you put both routes inside a `<Switch />`, so only one of them matches?

2.  Did you use `exact` on the `<Route />` that contains the component that is meant to render "under" the modal?


## Accessibility

Modals are rendered with the following attributes:

`role="dialog"`

`aria-modal="true"`

### role="dialog"

The role of modals defaults to `dialog`. You can specify a different `role`, for example `alertdialog`:

      <Modal role='alertdialog' aria-label='Important Notice!>
        Something important here!
      </Modal>`

The role can also be set on  `<ModalRoute />` and `<ModalLink />`.

### aria-\*

You should use the following props to describe your modal content:

-   [aria-labelledby](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_aria-labelledby_attribute)
-   [aria-describedby](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_aria-describedby_attribute)
-   [aria-label](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_aria-label_attribute)

Any props set on `<Modal />`, `<ModalRoute />`, or `<ModalLink />` that start with `aria-` will be rendered on the modal element.

For example:

    <Modal aria-labelledby="modal-title" aria-describedby="modal-description">
      <h3 id="modal-title">Important Information</h3>
      <p id="modal-description">
        A description of the purpose of this modal. 
      </p>
      ... additional modal content here ...
    </Modal>

See: [W3 Modal Example](https://www.w3.org/TR/wai-aria-practices-1.1/examples/dialog-modal/dialog.html)


## Examples

### TL;DR Example

```javascript
import { ModalContainer, ModalRoute } from 'react-router-modal';
import { BrowserRouter, Link } from 'react-router-dom';

// assumes webpack loader for css
// ... or include this css however you do that in your project ...
import 'react-router-modal/css/react-router-modal.css'

function FooModal() {
  return <div>FOO</div>;
}

function BarModal() {
  return <div>BAR</div>;
}

function Example() {
 return (
   <BrowserRouter>
     <div>
       <Link to='/foo'>show foo</Link>
       <Link to='/bar'>show bar</Link>

       <ModalRoute component={FooModal} path='/foo' className='test-modal test-modal-foo'/>
       <ModalRoute component={BarModal} path='/bar' className='test-modal test-modal-bar'/>

       <ModalContainer />
     </div>
   </BrowserRouter>
 );
}
```


## ModalContainer

Container for rendered modals.

This should be included in your react app as one of the last elements before the closing body tag.
When modals are rendered, they live inside this container.
When no modals are shown, nothing is rendered into the DOM.

**Parameters**

-   `props` **Props** 
    -   `props.modalClassName` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** class name to apply to modals (optional, default `react-router-modal__modal`)
    -   `props.backdropClassName` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** class name to apply to modal backdrops (optional, default `react-router-modal__backdrop`)
    -   `props.containerClassName` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** class name to apply to the container itself (optional, default `react-router-modal__container`)
    -   `props.bodyModalClassName` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** class name to apply to the <body /> when any modals are shown (optional, default `react-router-modal__modal-open`)
    -   `props.onFirstModalMounted` **[Function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function)?** handler invoked when first modal is shown
    -   `props.onLastModalUnmounted` **[Function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function)?** handler invoked when last modal is hidden
    -   `props.autoRestoreScrollPosition` **[boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Automatically restore the window scroll position when the last modal is unmounted. This is useful in cases where you have made the body position fixed on small screen widths, usually to work around mobaile browser scrolling behavior. Set this to false if you do not want this behavior. (optional, default `true`)
    -   `props.modalInClassName` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** class name applied to modal immediately after it is shown to allow for css transitions (optional, default `react-router-modal__modal--in`)
    -   `props.modalOutClassName` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** class name applied to modal before modal is hidden to allow for css transitions (optional, default `react-router-modal__modal--out`)
    -   `props.backdropInClassName` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** class name applied to backdrop immediately after it is shown to allow for css transitions (optional, default `react-router-modal__backdrop--in`)
    -   `props.backdropOutClassName` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** class name applied to backdrop before modal is hidden to allow for css transitions (optional, default `react-router-modal__backdrop--out`)
    -   `props.modalWrapperClassName` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** class name applied to backdrop before modal is hidden to allow for css transitions (optional, default `react-router-modal__wrapper`)
    -   `props.outDelay` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** delay, in milliseconds to wait when closing modal, to allow for css transitions to complete before ripping it out of the DOM (optional, default `0`)

**Examples**

_Using default class names_

```javascript
<ModalContainer />
```

_Overriding the default class names_

```javascript
<ModalContainer
  bodyModalOpenClassName='modal-open'
  containerClassName='modal-container'
  backdropClassName='modal-backdrop'
  modalClassName='modal'
/>
```

_DOM structure_

```javascript
// Note that modals are made "modal" via CSS styles, and end up rendered like the following in the DOM (with two modals, for example):
<div className={containerClassName}>
  <div>
    <div className={backdropClassName} />
    <div className={modalClassName}>
      .. bottom-most modal contents ..
    </div>
  </div>
  <div>
    <div className={backdropClassName} />
    <div className={modalClassName}>
      .. top-most modal contents ..
    </div>
  </div>
</div>
```

## ModalRoute

A react-router Route that shows a modal when the location pathname matches.

**Parameters**

-   `routeProps`  
-   `props` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** 
    -   `props.path` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** path to match
    -   `props.exact` **[Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** If set, only show modal if route exactly matches path.
    -   `props.parentPath` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** path to navigate to when backdrop is clicked
    -   `props.onBackdropClick` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** Handler to invoke when backdrop is clicked. If set, overrides the navigation to parentPath, so you need to handle that yourself.
    -   `props.className` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** class name to apply to modal container
    -   `props.children` **Children** modal content can be specified as chld elements
    -   `props.component` **ReactComponent** modal content can be specified as a component type. The component will be passed `parentPath` and `closeModal` props, in addition to the specified props, and the withRouter props.
    -   `props.props` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** Props to be passed to the react component specified by the component property.
    -   `props.inClassName` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** class name applied to modal immediately after it is shown to allow for css transitions (optional, default `react-router-modal__modal--in`)
    -   `props.outClassName` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** class name applied to modal before modal is hidden to allow for css transitions (optional, default `react-router-modal__modal--out`)
    -   `props.backdropClassName` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** class name applied to backdrop (optional, default `react-router-modal__backdrop`)
    -   `props.backdropInClassName` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** class name applied to backdrop immediately after it is shown to allow for css transitions (optional, default `react-router-modal__backdrop--in`)
    -   `props.backdropOutClassName` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** class name applied to backdrop before modal is hidden to allow for css transitions (optional, default `react-router-modal__backdrop--out`)
    -   `props.outDelay` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** delay, in milliseconds to wait when closing modal, to allow for css transitions to complete before ripping it out of the DOMWhen the route matches, the modal is shown.
        If multiple routes match, the modals will be stacked based on the length of the path that is matched.The component rendered in the modal will receive the following props: (optional, default `0`)
-   `parentPath` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** Either the parentPath specified in the ModalRoute, or a calculated value based on matched url
-   `closeModal` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** A convenience method to close the modal by navigating to the parentPath

## Modal

Renders its contents in a modal div with a backdrop.
Use Modal if you want to show a modal without changing the route.

The content that is shown is specified by _either_ the "component" prop, or by
child elements of the Modal.

**Parameters**

-   `props` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** 
    -   `props.stackOrder` **[Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** order to stack modals, higher number means "on top"
    -   `props.children` **Children** Modal content can be specified as chld elements
    -   `props.component` **Component** React component to render in the modal.
    -   `props.props` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** props to pass to the react component specified by the component property
    -   `props.onBackdropClick` **[Function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function)** handler to be invoked when the modal backdrop is clicked
    -   `props.className` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** class name to apply to modal container
    -   `props.inClassName` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** class name applied to modal immediately after it is shown to allow for css transitions
    -   `props.outClassName` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** class name applied to modal before modal is hidden to allow for css transitions
    -   `props.backdropInClassName` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** class name applied to backdrop immediately after it is shown to allow for css transitions
    -   `props.backdropOutClassName` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** class name applied to backdrop before modal is hidden to allow for css transitions
    -   `props.outDelay` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** delay, in milliseconds to wait when closing modal, to allow for css transitions to complete before ripping it out of the DOM

**Examples**

_Modals using a component and props, vs. child elements_

```javascript
const Hello = ({ who }) => (<div>Hello {who}!</div>);

// component and props
const ComponentExample = () => (
  <Modal
   component={Hello}
   props={{ who: 'World' }}
  />
);

// using child elements
const ChildrenExample = () => (
  <Modal>
    <Hello who='World' />
  </Modal>
);
```

_Specifying stack order_

```javascript
<div>
  <Modal
    className='top-component-modal'
    component={MyTopComponent}
    props={ { foo: 'bar'} }
    stackOrder={2}
  />
  <Modal
    component={MyBottomComponent}
    props={ { bar: 'baz'} }
    stackOrder={1}
  />
</div>
```

## ModalLink

Link and ModalRoute in one convenient component
Renders a link that, when clicked, will navigate to the route that shows the modal.

**Parameters**

-   `props` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** 
    -   `props.path` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** path to match
    -   `props.exact` **[Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** If set, only show modal if route exactly matches path.
    -   `props.parentPath` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** path to navigate to when backdrop is clicked
    -   `props.linkClassName` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** class name to apply to <Link />
    -   `props.modalClassName` **[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** class name to apply to modal container
    -   `props.children` **Children** Link contents. Note that Modal content must be specified by the component property.
    -   `props.component` **ReactComponent** Component to render in the modal.
    -   `props.props` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** Props to be passed to the react component specified by the component property.

**Examples**

_Example ModalLink_

```javascript
<ModalLink path='/hello' component={HelloComponent}>
  Say Hello
</ModalLink>
```
