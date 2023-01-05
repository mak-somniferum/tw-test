# Twin + Create React App + Emotion

It is based on ts, but it is also possible on jsx.

## Getting started

### installation

Install Create React App

```
npx create-react-app my-app --template typescript
```

Install the dependencies

```
npm install twin.macro tailwindcss @emotion/react @emotion/styled
```

### Add the global styles

```tsx
// index.tsx
import React from "react";
import ReactDOM from "react-dom/client";
import { GlobalStyles } from "twin.macro";
import App from "./App";
import reportWebVitals from "./reportWebVitals";

const root = ReactDOM.createRoot(
  document.getElementById("root") as HTMLElement
);
root.render(
  <React.StrictMode>
    <GlobalStyles />
    <App />
  </React.StrictMode>
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();
```

### Add the twin config in `package.json` (optional)

```json
// package.json

"babelMacros": {
  "twin": {
    "preset": "emotion",
    "config": "tailwind.config.js"
  }
},
```

Or you can add it to `babel-plugin-macros.config.js`:

```js
// babel-plugin-macros.config.js

module.exports = {
  twin: {
    preset: "emotion",
    config: "tailwind.config.js",
  },
};
```

Note: The preset gets set to 'emotion' by default, so adding the config is only useful if you want to adjust [Twin’s other options](#twin-options).

### Add the jsx pragma

To use the `tw` and `css` props, emotion must first extend jsx with a [jsx pragma](https://emotion.sh/docs/css-prop#jsx-pragma).

When styling with the tw or css prop, add the pragma at the top of your file. This also replaces the react import, unless you’re using fragments `<>`.

```js
/** @jsxImportSource @emotion/react */
import tw from "twin.macro";

const Input = () => <input tw="bg-black" />;
```

If you don't want to use jsx pragma, follow below:

Install `react-app-rewired` and `customize-cra`

```
npm i react-app-rewired customize-cra
```

Install babel's plugins

```
npm install @emotion/babel-plugin-jsx-pragmatic @babel/plugin-transform-react-jsx
```

Create `config-overrides.js` and `.babelrc` in root

```js
// config-overrides.js

const {
  useBabelRc,
  removeModuleScopePlugin,
  override,
} = require("customize-cra");

module.exports = override(useBabelRc(), removeModuleScopePlugin());
```

```json
// .babelrc

{
  "plugins": [
    "babel-plugin-macros",
    [
      "@emotion/babel-plugin-jsx-pragmatic",
      {
        "export": "jsx",
        "import": "__cssprop",
        "module": "@emotion/react"
      }
    ],
    [
      "@babel/plugin-transform-react-jsx",
      {
        "pragma": "__cssprop",
        "pragmaFrag": "React.Fragment"
      }
    ]
  ]
}
```

Modify the `package.json`'s scripts

```json
// package.json

"scripts": {
  "start": "react-app-rewired start",
  "build": "react-app-rewired build",
  "test": "react-app-rewired test",
},
```

Finally, you can use twin without jsx pragma.

## Resourse

- https://github.com/ben-rogerson/twin.macro
- https://github.com/ben-rogerson/twin.examples/tree/master/cra-emotion
- https://velog.io/@joabyjoa/create-react-app-typescript-emotion%EC%9C%BC%EB%A1%9C-React-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0
