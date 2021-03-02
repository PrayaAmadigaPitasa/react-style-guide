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