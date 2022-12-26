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


## Table of Contents
1. [Debounce callbacks on DOM events](#1-debounce-callbacks-on-dom-events)
2. [Use the production build](#2-use-the-production-build)
3. [Virtualise  long lists(>50) of data in React](#3-virtualise-long-lists50-of-data-in-react)
4. [Tree shake your react application modules](#4-tree-shake-your-react-application-modules)
5. [View Package Sizes in a React App with Webpack Analyzer](#5-view-package-sizes-in-a-react-app-with-webpack-analyzer)
6. [Containerisation of State within Child Components](#6-containerisation-of-state-within-child-components)
7. [Profile React Components with the Devtools Profiler](#7-profile-react-components-with-the-devtools-profiler)
8. [Optimize Function Components with React.memo](#8-optimize-function-components-with-reactmemo)
9. [Memoized Values with React useMemo](#9-memoized-values-with-react-usememo)
10. [Memoize a Function with useCallback in React](#10-memoize-a-function-with-usecallback-in-react)
11. [Add the why-did-you-render package](#11-add-the-why-did-you-render-package)
12. [Code Split Components with React Lazy OR Loadable Components](#12-code-split-components-with-react-lazy-or-loadable-components)
13. [Conclusion](#conclusion)

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

- Make sure you use import and exports.
- Check the imported packages & what do they export.
- Configure whatever bundler you are using to ignore transpiling modules to `CommonJs`.
- In the case of `webpack` see if you need to add a `side-effects` element into your `package.json`.

> 💡
A **side effect** is defined as code that performs a special behavior when imported, other than exposing one or more exports. An example of this are polyfills, which affect the global scope and usually do not provide an export.
> 

## 5. **View Package Sizes in a React App with [Webpack Analyzer](https://www.npmjs.com/package/webpack-bundle-analyzer)**

→ Follow the steps of installation based on the used bundler. By having an overview on the size of packages, making a decision to optimize which package or which component becomes easier and based on facts.

 

## 6. **Containerisation of State within Child Components**

→ Keep, call, use `states` only where they are really needed, and not on the parent level. This will prevent you from having to drill the state down to the child component and causing unnecessary re-rendering.  

> 💡**Tip**
When it’s unavoidable to define the state on the parent component, make sure to use `React.memo` ([documentation](https://beta.reactjs.org/reference/react/memo))
> 

## 7. **Profile React Components with the Devtools Profiler**

→ Install **[React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en),** run your app in production build to reproduce real-world scenario. You can also try it in development mode.

```bash
#To have a production build in profile mode set up for you 
# (if you are using create react app otherwise google it)
npx react-scripts build --profile
```

Open your devtool and start profiling, then check the **flamegraph chart** and the **ranked chart** to see which components are taking the most time to load and how many reloads. 

## 8. **Optimize Function Components with React.memo**

→ When you have a state on the parent component, the update of that state will cause a re-render for the child components.

To avoid that wrap your **functional component** in a `React.memo` function. 

🗒️ `React.memo` works only on functional components.

🗒️ `React.memo` only does a **shallow compare on the props** that it gets, so if you have a nested props object you need to use the second param of `React.memo` to specify your comparison rule.

```jsx
---Product (Component) 
-----ProductCard (Component) <--- // add the React.memo in here to prevent 
// changes on the parent component triggering a change on all the product cards.
```

🗒️ `React.memo` check only the props changes if the component that has React.memo has a `useState`, `useReducer` or `useContext` it will still re-render properly once state or context changes.

## 9. **Memoized Values with React useMemo**

→ To prevent computing a value on every re-render (sorting, looping, reducing…) wrap your value into a function and use `useMemo` hook to prevent a re-calculation every time, unless some dependencies changed, then it will recalculate. 

## 10. **Memoize a Function with useCallback in React**

→ Add useCallback on functions that are passed as a prop, because on component re-render, they will cause an additional re-render despite the fact that their implementation never changed.

```jsx
const [count1, setCount1] = React.useState(0);
// No dependencies because the implimentation will never change. 
const likeIncrement = React.useCallback(() => setCount1((c) => c + 1), []); 
```

## 11. **Add the [why-did-you-render](https://github.com/welldone-software/why-did-you-render) package**

→ Follow the installation process, then import the created file `wdyr.js or .ts` in the target component you want to check. 
Open your console and you will find the logs regarding the reason of re-render.

## 12. **Code Split Components with [React Lazy](https://reactjs.org/docs/code-splitting.html#reactlazy) OR [Loadable Components](https://loadable-components.com/docs/getting-started/)**

→ **React lazy if for create react apps** and **loadable is for server side render** apps.

In your **routes** for example

```jsx
import React, { Suspense } from 'react';

const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <div>
			<!-- Suspense is needed in the case of React.lazy -->
      <Suspense fallback={<div>Loading...</div>}>
        <OtherComponent />
      </Suspense>
    </div>
  );
}
```

Same for loadable (server side e.g. **Next.js**)

```jsx
import loadable from '@loadable/component';

const OtherComponent = loadable(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <div>
      <OtherComponent />
    </div>
  )
}
```

## Conclusion

→ There is many ways to optimize your react app, I recommend to use most of them while developing component by component. Test your component/page performance before shipping it to review, do the improvements as much as possible and not after the app is out of control.

→ For the already existing apps focus on page per page until your full app is on an optimal performance.