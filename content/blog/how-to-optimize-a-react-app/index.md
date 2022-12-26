---
title: How to optimize a react application performance
date: "2022-12-25T20:21:32.169Z"
description: Summary of the most and best used techniques to optimize your react application performance.
---

# React optimization

`Less is more when it comes to react apps`

- Send to the browser **the least amount of code** possible.
- Render **the least amount of components** in a page.
- **Re-render components the least amount** of time.

## 1. Debounce callbacks on DOM events

→ Add a [debounce](https://lodash.com/docs/4.17.15#debounce) on rapid DOM event calls e.g. search while typing or a mouse event... 

It will reduce the re-rendering and the number of calls done by the event/component. 

## 2. Use the production build

→ Make sure the build config is using the `production` mode. By double checking in your `webpack` or other bundlers that this mode is used while shipping your app.

## 3. Virtualise  long lists(>50) of data in React

→ Use a windowing technique e.g. the package [react-window](https://www.npmjs.com/package/react-window) will enable you to achieve an **infinite scroll like experience** and display only a chunk of items instead of loading all of them at once. 

## 4. Tree shake your react application modules

→ For example importing `lodash`  (article about [tree shaking](https://webpack.js.org/guides/tree-shaking/) 🗒️)

```jsx
import { debounce } from 'lodash' // (NO)

import debounce from 'lodash/debounce' // (YES)
```

- Make sure your work with import and exports.
- Check the imported packages & what do they export.
- Configure whatever bundler you are using to ignore transpiling modules to `CommonJs`.
- In the case of `webpack` see if you need to add a `side-effects` into your `package.json`.

> 💡
A **side effect** is defined as code that performs a special behavior when imported, other than exposing one or more exports. An example of this are polyfills, which affect the global scope and usually do not provide an export.
> 

## 5. **View Package Sizes in a React App with [Webpack Analyzer](https://www.npmjs.com/package/webpack-bundle-analyzer)**

→ Follow the steps of installation based on the used bundler. By having an overview on the size of packages, making a decision to optimize which package or which component becomes easier and based on facts.

 

## 6. **Containerisation of State within Child Components**

→ Keep, call, use `states` only where they are really needed, and not on the parent level. This will prevent you from having to drill the state down to the child component and causing unnecessary re-rendering.  

> 💡
When it’s unavoidable to define the state on the parent component, make sure to use `React.memo` ([documentation](https://beta.reactjs.org/reference/react/memo))
>