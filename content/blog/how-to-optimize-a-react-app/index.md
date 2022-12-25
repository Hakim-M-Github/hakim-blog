---
title: How to optimize a react application performance
date: "2022-12-25T20:21:32.169Z"
description: Summary of the most and best used techniques to optimize your react application performance.
---

# React optimization

`Less is more when it comes to react apps`

- Send to the browser **the least amount of code** possible
- Render **the least amount of components** in page as necessarily
- **Re-render components the least amount** of time as possible

## 1. Debounce callbacks on DOM events

→ By adding a debounce on fast calls like search while typing or a mouse event. This will reduce the re-rendering and the number of calls done by the component. 

## 2. Use the production build

→ Make sure the build config is using production mode and also in your webpack or other bundlers double check which mode is used to ship your app.

## 3. Virtualise  long lists(>50) of data in React

→ Windowing technique by using the package [react-window](https://www.npmjs.com/package/react-window) you can achieve **infinite scroll like experience** and display only a chunk of items instead of loading all of them at once. 

## 4. Tree shake your react application modules

→ For example importing `lodash`  (article about [tree shaking](https://webpack.js.org/guides/tree-shaking/) 🗒️)

```jsx
import { debounce } from 'lodash' // (NO)

import debounce from 'lodash/debounce' // (YES)
```

- Make sure your work with import and exports
- Check the imported packages what do they export
- Configure whatever bundler you are using to ignore transpiling  modules  to `CommonJs`
- In the case of `webpack` see if you need to add a `side-effects` into your `package.json`

<aside>
💡 ****Tip**** 
A **side effect** is defined as code that performs a special behavior when imported, other than exposing one or more exports. An example of this are polyfills, which affect the global scope and usually do not provide an export.

</aside>

## 5. **View Package Sizes in a React App with [Webpack Analyzer](https://www.npmjs.com/package/webpack-bundle-analyzer)**

→ Adding it as a plugin into your bundler will enable you to view the packages used and their size

By having an overview it help you keep under-control the big components and packages and make mindful decisions.

 

## 6. **Containerisation of State within Child Components**

→ Only keep/call/use the state where it’s  needed instead of drilling it down to the child to prevent unnecessary re-rendering.

→ If you cannot avoid this use `React.memo`
