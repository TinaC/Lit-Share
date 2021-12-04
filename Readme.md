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

- native DOM/Javascript API only

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
- No Typescript support

[Doc](https://github.com/SAP/ui5-webcomponents/tree/master/docs/5-development)

---

# Lit

[Homepage]()
