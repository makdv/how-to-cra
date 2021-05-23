# How to Create React Application

An opinionated version of CRA template with some exrta tools added by default.

## Tools

- Uses [react-scripts v4](https://github.com/facebook/create-react-app/releases/tag/v4.0.0) with enabled fast-refresh and web-vitals support out of the box
- .vscode/settings.json to use prettier as a default formatter and enable formatting on save
- .vscode launch.json to enable debugging in vscode. Also it's encouraged to use popular [Jest](https://github.com/jest-community/vscode-jest) extension
- added [husky](https://typicode.github.io/husky/#/) with [prettier](https://prettier.io/docs/en/install.html) and [lint-staged](https://github.com/okonet/lint-staged#readme) for pre-commit hook with respective configurations inside package.json
- customized linter config (refer to package.json)
- added [storybook](https://storybook.js.org/docs/react/get-started/install) for faster component development
- added "analyze" script to analyze a bundle size

## Performance recommendations and best practices:

- Use memoization ( pure components, useMemo, useCallback, reselect, ...):

  - Remember that functional components are not pure by default. You should wrap them with React.memo to make "pure".
  - You might not need to make all components "pure" by default and first investigate which components are frequently re-rendered,
    but very often you can foresee where better to add React.memo wrapper, for example to lists items, parent components for a large component tree, etc.
  - If you are not sure you will have enough time to investigate and improve the performance, you might want to use "pure" components by default as "shallow" comparison
    will be much cheaper than the component react component "render".

- Keep an eye on properties you pass to component to avoid additional re-renders:

  bad:

  ```
  <MyComponent myComponentProps={{ …someProps, newProp : ‘test’ }} />
  ```

  good:

  ```
  <MyComponent {{ …someProps }} newProp={‘test’} }} />
  ```

  </br>
   bad:

  ```
  <MyComponent myCallback={() => {...}} />
  ```

  good:

  ```
  <MyComponent myCallback={myMemoizedCallback} />
  ```

  </br>

- Same for properties passed ot a context:
  bad:

```
<MyContext.Provider value={{ ...someProps}}>...</MyContext.Provider>
```

good:

```
const memoizedContextProps = useMemo(() => {...deps}, [...deps]);
<MyContext.Provider value={ memoizedContextProps}>...</MyContext.Provider>
```

- Use "renderSomething" method pattern with a causion inside components. It might be better to create a separate component
  to let react control rendering and avoid additional re-renders
- Component "key" property should be unique, but stable. Don't use uuid() as a key property as soon as it generates new key every time.
- Use chrome developer tools "Performance" tab and react developer tools "Profiler" tab to inspect slow places
- Use code splitting with React.lazy,Suspense and redux-injector functions from "utils" to dynamically inject sagas and reducers (examples at https://www.npmjs.com/package/redux-injectors)

## Customization

- As an "eject" alternative use [craco](https://github.com/gsoft-inc/craco) package to customize the webpack build

## Testing in VSCode

- You can debug tests in VSCode: launch configuration already added
- Recommended to use [Jest](https://github.com/jest-community/vscode-jest) extension with enabled "show coverage" option to track test coverage immediately
- Recommended to use [Jest runner](https://marketplace.visualstudio.com/items?itemName=firsttris.vscode-jest-runner) extension to run and debug individual tests easily: above each "describe" and "test" the repsective "Run|Debug" links will appear. Refer to this issue wit the latest react-scripts: https://github.com/firsttris/vscode-jest-runner/issues/136

## Usage with redux

- Use redux chrome extension
- Use [@reduxjs/toolkit](https://redux-toolkit.js.org/tutorials/basic-tutorial) to create actions and reducers
