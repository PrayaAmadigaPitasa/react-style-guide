# React Style Guide

This style guide was created to provide standards based on the fundamentals of React, Javascript and several tools used to make development easier (eg prettier, eslint, typescript). This style guide was also developed based on several existing style guides such as Khan and AirBnB React Style Guide.

## Basic Rules
- **Export Component**: Only export one react element per file.
- **Format Component**: Use functional component ([hooks](https://reactjs.org/docs/hooks-intro.html)) instead of class component.
- **Code Formatter**: Use [`prettier`](https://prettier.io/docs/en/index.html) for code formatter.
- **Code Rules**: Use [`eslint`](https://eslint.org/) for code rules.
- **Static Typing**: Use [`typescript`](https://www.typescriptlang.org/) for static typing.
- **Aliases**: Use [`babel-plugin-module-resolver`](https://www.npmjs.com/package/babel-plugin-module-resolver) to create aliases for the domain.

## Files
- **Extensions**: Use `.tsx` extension for file contains React Element and `.ts` extension for file javascript without React Element.

## Component
- **Location**: All module components located in `./src/components/`
- **Inheritance**: Extends the component properties from its parrent.
  ```typescript
  import React, {ReactNode} from 'react';
  import {Text as TextRN, TextProps as TextPropsRN} from 'react-native'

  export interface TextProps extends TextPropsRN {
    extensionKeyProps: string;
    children: ReactNode;
    // more extension properties
    ...
  }

  export default function Text({extensionKeyProps, children, ...props}: TextProps) {
    // handle the extension properties
    ...

    return <Text {...props}>{children}</Text>
  }
  ```

## Modules
- **Aliases**: Use `@` prefix for the module aliases. E.g., `@components`
- **Export**: All functions and components must exported and handled in index.ts until reach it's domain. Because, to implement aliases we need to access all exported functions and components from it's domain.
  ```typescript
  // File Input Component (./src/components/inputs/Input.tsx)

  export default function Input() {

  }

  // File index in inputs folder (./src/components/inputs/index.ts)

  export * from './Input';
  export * from './InputSearch';

  export {default as Input} from './Input';
  export {default as InputSearch} from './InputSearch';

  // File index in components folder (./src/components/index.ts)

  export * from './inputs';
  export * from './buttons';
  export * from './modals';
  ```
- **Import**: Sort import module based on precedence. (react -> react-native (mobile) -> library -> aliases -> folder -> file).
  ```typescript
  import React from 'react';
  import {Input} from 'react-native';
  import Modal from 'react-native-modal';
  import {Button} from '@components';
  import {getUserData} from '@services';
  import {LocationListProps} from './location';
  import {MapDetail} from './MapDetail';
  ```

## Variable
- **Constant**: Use `UPPERCASE` for exported constant field and `camelCase` for the otherwise. *(Field is variable located in root files)*
  ```typescript
    // good
    export const BUTTON_PADDING = 10;

    // bad
    export const buttonPadding = 10;

    // bad
    export const ButtonPadding = 10;

    // bad
    const COLUMNS = 3;

    // good
    const columns = 3;

    // bad 
    const Columns = 3;
    ```
- **Let**: Use `camelCase` for let variable. And don't export let variable. Use getter and setter or context for accessing let variable outside file.
  ```typescript
  // bad
  let COUNT = 0;

  // bad
  let Count = 0;

  // good
  let count = 0;
  ```
- **Var**: Don't use `var` for the variable. Because, var variable can be accessed from outside the block and hard to track and cause confusion.