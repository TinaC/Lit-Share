---
marp: true
theme: default
---

<style>
section {
  background: #1a1626;
}

h1, h2, h3 {
  color: #fff;
}

text,
p,
li {
  color: #fff;
}

a {
  color: rgba(237,100,166);
}
</style>

# Web Component with Lit

Tina Chen
2021.12.15

---

# Web Component

> Web components are a set of **web platform APIs** that allow you to create new custom, reusable, encapsulated HTML tags to use in web pages and web apps
> [Defined in the HTML Living Standard specification](https://html.spec.whatwg.org/multipage/custom-elements.html#custom-elements)

Web Components consists of three main technologies

- Custom elements - Custom HTML tag name
- Shadow DOM - Avoid collision [Demo](https://codepen.io/tinac/pen/LYzVBzG)
- HTML templates: `<template>` `<slot>` - Reusable [Demo](https://codepen.io/tinac/pen/YzrXjEN)

---

# Why We Choose Lit?

~~[Libraries](https://www.webcomponents.org/libraries)~~
[Libraries](https://open-wc.org/guides/community/base-libraries/)

---

# Glossary

## Fiori

> Design Guideline / Design System

[Homepage](https://www.sap.com/sea/products/fiori.html)
[Internal Spec](https://wiki.wdf.sap.corp/wiki/pages/viewpage.action?pageId=2020615073)
[Theme Parameter Toolbox](https://ui5.sap.com/test-resources/sap/m/demokit/theming/webapp/index.html)
[Sematic Color](https://wiki.wdf.sap.corp/wiki/pages/viewpage.action?pageId=2144577672)

---

# SAPUI5

> MVC Framework + Rich Control Library

[Homepage](https://sapui5.hana.ondemand.com/)

# OpenUI5

> OpenUI5 is Open Source, released under the Apache 2.0 license.
> Subset of SAPUI5, no charts, oData related "Smart controls"...

[Homepage](https://openui5.hana.ondemand.com/)
[GitHub](https://github.com/SAP/openui5)

---

# UI Integration Cards

> Card is configured by a simple JSON configuration (schema) without the need to write code for UI rendering.
> But Component Card allows the integration of UI5 Components as content.

Used in Work Zone / CEP Page Editor

[Homepage](https://ui5.sap.com/test-resources/sap/ui/integration/demokit/cardExplorer/webapp/index.html#/overview/introduction)

---

As SAPUI5 is too heavy and hard to integrate with others...
Here comes the Web Component:

# UI5 Web Component

- Fiori-compliant

- Light-weight **Web Component** control library

- Native DOM/Javascript API only

- Less controls than SAPUI5 :(

Heavily used in CEP

[Homepage](https://sap.github.io/ui5-webcomponents/playground/components)

---

# UI5 Web Component for React

> Wrapper for UI5 Web Components in React
> Returns `React Elements` instead of `HTML Elements`

Used in WorkZone new features: AI Bots, Profile Avatar...

[Github](https://github.com/SAP/ui5-webcomponents-react)

---

# Summary

- **Fiori** : Design Guideline

- SAPUI5 : MVC + Rich Controls

  - OpenUI5
    - UI Integration Cards: Special Control

- Web Component - Native API
  - UI5 Web Component : Light Controls
    - UI5 Web Component for React

---

# @ui5/webcomponents-tools

> UI5 Web Components' standard build tools

- Based on Lit
- No Typescript Support

[Documentation](https://github.com/SAP/ui5-webcomponents/tree/master/docs/5-development)

---

# Lit

- Make it easier to build a web component
- Successor of Polymer Project, launched in 2015 by Google.

Core technologies:

- `lit-element`: a quick way to define web components
- `lit-html`: a powerful HTML templating library

[Homepage](https://lit.dev/)
[Sample](https://lit.dev/playground/#sample=examples/full-component)

---

# Template / lit-html

> Lit templates can include JavaScript expression.

> [React JSX](https://reactjs.org/docs/introducing-jsx.html): JSX may remind you of a [template language](https://stackoverflow.com/questions/13654009/executing-javascript-inside-handlebars-template)(such as Handlebars), but it comes with the full power of JavaScript.

> Handlebars and Mustache are LOGICLESS by design.

[Expressions in Template, DEMO](https://lit.dev/playground/#sample=examples/expressions)

---

# Expressions

| Type                                                                                                      | Example                                                      |
| --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| Child nodes                                                                                               | `` html`<main>${bodyText}</main>` ``                         |
| Attributes                                                                                                | `` html`<div class=${highlightClass}></div>` ``              |
| [Boolean Attributes](https://html.spec.whatwg.org/multipage/common-microsyntaxes.html#boolean-attributes) | `` html`<div ?hidden=${!show}></div>` ``                     |
| Properties                                                                                                | `` html`<input .value=${value}>` ``                          |
| Event listeners                                                                                           | `` html`<button @click=${this._clickHandler}>Go</button>` `` |

---

# Properties vs. Attribute (Native Concept)

- Attribute

Attributes are key-value pairs defined in **HTML** on an element.
HTML attribute is **case-insensitive**.
Because HTML is string-based, attributes can **only be strings**.

```js
<div id="myDiv" foo="bar"></div>;

const myDiv = document.getElementById("myDiv");

console.log(myDiv.attributes);
console.log(myDiv.getAttribute("foo"));
console.log(myDiv.hasAttribute("foo"));

myDiv.setAttribute("foo", "not-bar");
```

---

# Properties vs. Attribute

- Properties

Properties are key-value pairs defined on a javascript object(Possibly DOM object).
A great benefit of properties is that they can accept any javascript value, including **complex objects and arrays**.

```js
const myObject = {};
// a property is set
myObject.foo = "bar";

const myDiv = document.getElementById("myDiv");
// set properties on DOM elements
myDiv.foo = "bar";
```

Reference:
[HTML attribute vs DOM Object properties](https://javascript.info/dom-attributes-and-properties)
[Web Component: Attributes and properties](https://open-wc.org/guides/knowledge/attributes-and-properties/)

---

# Transformation

When the browser parses the HTML to create DOM objects for tags, it recognizes standard attributes and creates DOM properties from them.
But that doesn’t happen if the attribute is **non-standard**.

在 parse HTML 的时候， attribute -> property
非标准属性不会转化

```html
<body id="test" something="non-standard">
  <script>
    alert(document.body.id); // test
    // non-standard attribute does not yield a property
    alert(document.body.something); // undefined
    // way to access non-standard attributes
    alert(document.body.getAttribute("something")); // non-standard
  </script>
</body>
```

---

# Attribute Reflection

Many native elements reflect their properties as attributes, and vice versa, like for example the `type` attribute on an input.

在修改 attribute 的时候，某一些标准的属性，会同步/反射到 properties 上去，但是不是所有的属性都会

```html
<input type="text" />
```

```js
console.log(myInput.type); // text
console.log(myInput.getAttribute("type")); // text

// If we change the type attribute to number, it will be synced with the property:
myButton.setAttribute("type", "number");
console.log(myInput.type); // number

// If we set the property to date, it will be synced with the attribute:
myButton.type = "date";
console.log(myButton.getAttribute("type")); // date
```

---

# Attribute Reflection

Most **native**(standard) attributes are synced to the javascript object, but **not all** native properties are reflected up to attributes.

> Reason: custom elements properties can update often and triggering a DOM change for each update can impact performance.

---

# Comparison

## Transformation

- When: Parse html
- HTML Attribute -> DOM Object Property
- Target: All standard attribute

## Attribute Reflection

- When: setAttribute(), change object property
- HTML Attribute <-> DOM Object Property
- Target:

---

# Lit Property Options

```js
class MyElement extends LitElement {
  @property()
  name: string;

  @property({ type: Boolean, reflect: true })
  active: boolean = false;
}
```

---

# Lit Reflection

Attribute -> Property
Property -> Attribute (Not recommend, Attribute as the initial state)

[Reflection DEMO](https://lit.dev/playground/#sample=properties/attributereflect)

```js
// Value of property "active" will reflect to attribute "active"
@property({type: Boolean, reflect: true})
active: boolean = false;
```

---

# Property and Attributes converter

[Default converter: type option](https://lit.dev/docs/components/properties/#conversion-type)

```js
// Use the default converter
@property({ type: Number })
```

Custom converter:

```js
prop1: {
  converter: {
    fromAttribute: (value, type) => {
      // `value` is a string
      // Convert it to a value of type `type` and return it
    },
    toAttribute: (value, type) => {
      // `value` is of type `type`
      // Convert it to a string and return it
    }
  }
}
```

---

# Property and State

```js
class MyElement extends LitElement {
  @property()
  name: string;

  @state()
  protected _active = false;
}
```

---

# React props and state

> **Props** are Read-Only, must never modify its own props. (Not in lit)

> **State** is similar to props, but it is private and fully controlled by the component.
> **State** Updates May Be Asynchronous
> React may batch multiple `setState()` calls into a single update for performance.

# Lit

When a reactive property changes, the component schedules an update.
**Internal reactive state** refers to **reactive properties** that aren't part of the component's API.
**Internal reactive state** works just like public reactive properties, except that there is no attribute associated with the property.

[Property change DEMO](https://lit.dev/playground/#sample=examples/properties-has-changed)

---

# [Trap](https://open-wc.org/guides/knowledge/lit-element/rendering/)

LitElement's property system only observes changes to the object reference.
Recursively listening for changes to child properties would be prohibitively expensive, especially for large nested objects.

```js
myEl.items; // [{ name: 'foo' }, { name: 'bar' }]
// Won't trigger an update
myEl.items[0].name = "baz";

// example of updating the `changed` property on `myEl.items`
myEl.items = {
  ...myEl.items,
  changed: newVal,
};

// Or
this.requstUpdate();
```

---

# Omission

- [Lifecycle](https://lit.dev/docs/components/lifecycle/)
- Events
- Decorators, `@query`, `@property` ...
- Template Directives, `classMap`, `until` ...

---

# Downside of Lit

- We abandoned `shadow DOM` in CEP. [Sample](https://lit.dev/docs/components/shadow-dom/#implementing-createrenderroot)
  - UI Integration Cards `this.byId()`

```js
this.shadowRoot.getElementById('target');

document.querySelector('popup-info').shadowRoot.querySelector('.wrapper');

document.querySelector('popup-info').shadowRoot.querySelector('.wrapper')
  .shadowRoot.querySelector('.grandChildren')...;
```

---

# Render the template into the main DOM tree

[Remove shadow Root](https://lit.dev/docs/components/shadow-dom/#implementing-createrenderroot)

```js
// Default
protected createRenderRoot(): Element|ShadowRoot {
  return this.attachShadow({mode: 'open'});
}

// Note, styling leaks in
override createRenderRoot(): LitElement {
  return this;
}
```

---

# How we handle Style Collision now?

```css
/* Case-insensitive CSS Type selectors*/
SAP-SHELL-SECTION .container {
  --ui5_padding_left_right: calc(2rem + 0.1875rem);
}

SAP-SHELL-PAGE .page-heading {
  display: none;
  width: var(--shell-container-width);
}
```

Q: What's the potential risk of this approach?

---

# Answer

```css
PARENT-COMPONENT .container {
  color: blue;
}

CHILD-COMPONENT .container {
  margin: 0;
}

// Type selector is Forbidden
PARENT-COMPONENT span {
  color: blue;
}
```

---

# [Styled Component](https://styled-components.com/docs)

```css
.hEYqLS {
  margin: 0;
}

.jzwLKe {
  float: right;
}
```

---

# Downside of Lit - Ecosystem

- As the app gets complicated, Lit could not satisfied the requirement.
  - Router(Vanilla JS)
  - State Container(Redux)

---

# Thanks
