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
- **Import**: Sort import module based on precedence. (react -> external library -> aliases -> folder -> file).
  ```typescript
  import React from 'react';
  import {ModalProps} from 'react-native-modal';
  import {Button} from '@components';
  import {getUserData} from '@services';
  import {LocationListProps} from './location';
  import MapDetail from './MapDetail';
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
- **Var**: Don't use `var` for the variable. This is because the var variable can be accessed from outside the block which causes confusion and is difficult to trace.

## Types
- **String, Number and Boolean**: Use `string|number|boolean` instead of `String|Number|Boolean`
  ```typescript
  // good
  const name = "Charlotte"

  // good
  // declaring the type is optional, because typescript will
  // automatically declare the type same as the type of initial value.
  const name: string = "Charlotte"

  // bad
  const name: String = "Charlotte"
  ```
- **Type**: 
  - **Used for**: Abstract Type, Static String Values, Combine Multiple Type Data.
  - **Basic**: Use `PascalCase` for the syntax. And use `snake_case` for type string values.
    ```typescript
    // good
    type NavigationKey = ...;

    // bad
    type NAVIGATION_KEY = ...;

    // good
    type NavigationKey = 'auth_login' | 'auth_register';

    // bad
    type NavigationKey = 'authLogin' | 'authRegister';
    ```
  - **Abstraction**: Create type data abstract.
    ```typescript
    // good
    type Keys = 'key_1' | 'key_2' | 'key_3';

    type KeyMap<V> = {[K in Keys]: V};

    const maps: KeyMap<string> = {
      key_1: 'value_1',
      key_2: 'value_2',
      key_3: 'value_3',
    }
    ```
- **Array**: 
  - **Basic**: Use literal syntaxt to create array and declare the type data array if the initial value is empty.
    ```typescript
    // bad
    const values = new Array();

    // bad
    const values = [];

    // good
    const values = [0];

    // good 
    const values = ['', 0];

    // good
    const values: number[] = [];
    ```
  - **Populate Values**: Use `Array.from` to populate array values. 
    ```typescript
    // good
    const values = Array.from(['Charlotte', 'Celdric'], (item, index) => `${index}: ${item}`);

    // good
    const values = Array.from({length: 5}, (_, index) => index);

    // bad
    const values = [];

    for (let index = 0; index < 5; index++) {
      values.push(index);
    }
    ```
- **Object**: Use literal syntax to create objects and object must declare the type data from the interface or type. Because, typescript cannot define the type data used of the object except if it's handled by generic. The other reason is also to provide tab-completer from the editor.
  ```typescript
  // bad 
  const user = new Object();

  // bad 
  const user = {};

  // bad
  const user = {id: 0, name: 'Charlotte'};

  // good
  const user: User = {id: 0, name: 'Charlotte'};
  ```

## Function
- **Used For**: Components, Helpers, Custom Hooks and Services.
- **Naming Convention**: use `camelCase` for function.
  ```typescript
  // bad
  function DoSomething() {

  }

  // good
  function doSomething() {

  }
  ```

## Arrow Function
- **Used For**: Callbacks.
- **Single Line**: Don't use block scope for arrow function that only have one line.
  ```typescript
  // bad
  return <Input {...props} onFocus={() => {
    callSomething();
  }} />

  // good
  return <Input {...props} onFocus={() => callSomething()} />
  ```
- **Immediate Return**: Don't use block scope to return value if only have one line logic or return.
  ```typescript
  // bad
  return <MyComponent {...props} value={() => {
    return sum;
  }} />

  // good
  return <MyComponent {...props} value={() => sum} />

  // bad
  return <MyComponent {...props} value={(count) => {
    if (timestamp < now) {
      return previousCount;
    } else {
      return count;
    }
  }} />

  // good
  return <MyComponent {...props} value={(count) => timestamp < now ? previousCount : count} />
  ```

## Translations
- **Keys**: Keys must be defined first through type or interface. Because it will be used for all locale and.
- **Structures**: Key are grouped based on scope, feature and function. (general -> category feature -> feature -> function -> specific function -> properties).
  ```typescript
  // good 
  global_error: "Error"
  global_login: "Login"

  // good
  input_search_label: "Search"
  input_search_placeholder: "ex. Chocolate" 

  // bad
  // label and placeholder is part property of input. search is a specific function of input.
  input_label_search: "Search"
  input_placeholder_search: "ex. Chocolate"

  // good
  // auth is category feature where login is the specific feature of auth.
  auth_login_input_username_placeholder: "Username"
  auth_login_input_password_placeholder: "Password"

  // good
  // in this example login is dissapeared, this mean all input username and password for auth will use this translation as default.
  auth_input_username_placeholder: "Username"
  auth_input_password_placeholder: "Password"
  ```