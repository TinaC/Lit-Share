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

> Web Components is a suite of different technologies allowing you to create **reusable** custom elements — with their functionality **encapsulated** away from the rest of your code — and utilize them in your web apps.

Web Components consists of three main technologies

- Custom elements
- Shadow DOM - Avoid collision [Demo](https://codepen.io/tinac/pen/LYzVBzG)
- HTML templates: `<template>` `<slot>` - Reusable [Demo](https://codepen.io/tinac/pen/YzrXjEN)

---

# Why We Choose Lit?

---

# Glossary

## Fiori

> Design Guideline / Design System

[Homepage](https://www.sap.com/sea/products/fiori.html)
[Internal Spec](https://wiki.wdf.sap.corp/wiki/pages/viewpage.action?pageId=2020615073)

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

> A variety of card types can be configured by a simple JSON configuration (schema) without the need to write code for UI rendering.
> But Component Card allows the integration of UI5 Components as content.

Used in Work Zone / CEP Page Editor

[Homepage](https://ui5.sap.com/test-resources/sap/ui/integration/demokit/cardExplorer/webapp/index.html#/overview/introduction)

---

As SAPUI5 is too heavy...

# UI5 Web Component

- Fiori-compliant

- Light-weight **Web Component** control library

- Native DOM/Javascript API only

Heavily used in CEP

[Homepage](https://sap.github.io/ui5-webcomponents/playground/components)

---

# UI5 Web Component for React

> Wrapper for UI5 Web Components in React
> Returns `React Elements` instead of `HTML Elements`

Used in WorkZone: AI Bots, Profile Avatar...

[Github](https://github.com/SAP/ui5-webcomponents-react)

---

# Summary

- **Fiori**

- SAPUI5

  - OpenUI5
    - UI Integration Cards

- Web Component
  - UI5 Web Component
    - UI5 Web Component for React

---

# @ui5/webcomponents-tools

> UI5 Web Components' standard build tools

- Based on Lit
- No Typescript Support

[Doc](https://github.com/SAP/ui5-webcomponents/tree/master/docs/5-development)

---

# Lit

- Make it easier to build a web component
- Successor of Polymer Project, launched in 2015 by Google.

Core technologies:

- `lit-element`: a quick way to define web components
- `lit-html`: HTML templating library

[Homepage](https://lit.dev/)
[Sample](https://lit.dev/playground/#sample=examples/full-component)

---

# Template / lit-html

> Lit templates can include JavaScript expression.

> [React JSX](https://reactjs.org/docs/introducing-jsx.html): JSX may remind you of a [template language](https://stackoverflow.com/questions/13654009/executing-javascript-inside-handlebars-template)(such as Handlebars), but it comes with the full power of JavaScript.

[Expressions in Template, DEMO](https://lit.dev/playground/#sample=examples/expressions)

---

# Expressions

| Type                                                                                                      | Example                                                                             |
| --------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| Child nodes                                                                                               | `` html`<main>${bodyText}</main>` `` (TemplateResult, DOM, Primitive values, Array) |
| Attributes                                                                                                | `` html`<div class=${highlightClass}></div>` ``                                     |
| [Boolean Attributes](https://html.spec.whatwg.org/multipage/common-microsyntaxes.html#boolean-attributes) | `` html`<div ?hidden=${!show}></div>` ``                                            |
| Properties                                                                                                | `` html`<input .value=${value}>` ``                                                 |
| Event listeners                                                                                           | `` html`<button @click=${this.\_clickHandler}>Go</button>` ``                       |

HTML attribute is case-insensitive, always string typed.
[Further Reading: HTML attribute vs DOM Object properties](https://javascript.info/dom-attributes-and-properties)

---

# Props and State

- Reactive updates
- Attribute handling
- Superclass properties
- Element upgrade

```js
class MyElement extends LitElement {
  @property()
  name: string;

  @state()
  protected _active = false;
}
```

[Property change DEMO](https://lit.dev/playground/#sample=examples/properties-has-changed)

---

# React props and state

> **Props** are Read-Only, must never modify its own props. (Not in lit)

> **State** is similar to props, but it is private and fully controlled by the component.
> **State** Updates May Be Asynchronous
> React may batch multiple `setState()` calls into a single update for performance.

# Lit

When a reactive property changes, the component schedules an update.
**Internal reactive state** refers to **reactive properties** that aren't part of the component's API.
~~**State properties** don't have corresponding attributes.~~
**Internal reactive state** works just like public reactive properties, except that there is no attribute associated with the property.

---

# Downside of Lit

- We abandoned `shadow DOM` in CEP. [Sample](https://lit.dev/docs/components/shadow-dom/#implementing-createrenderroot)
  - UI Integration Cards
  - TinyMCE Editor

---

# Render the template into the main DOM tree

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

# Downside of Lit

- As the app gets complicated, Lit could not satisfied the requirement.
  - Router
  - Redux, State Store

---

# Thanks
