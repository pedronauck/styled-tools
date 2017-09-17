# styled-tools 💅

[![Generated with nod](https://img.shields.io/badge/generator-nod-2196F3.svg?style=flat-square)](https://github.com/diegohaz/nod)
[![NPM version](https://img.shields.io/npm/v/styled-tools.svg?style=flat-square)](https://npmjs.org/package/styled-tools)
[![Build Status](https://img.shields.io/travis/diegohaz/styled-tools/master.svg?style=flat-square)](https://travis-ci.org/diegohaz/styled-tools) [![Coverage Status](https://img.shields.io/codecov/c/github/diegohaz/styled-tools/master.svg?style=flat-square)](https://codecov.io/gh/diegohaz/styled-tools/branch/master)

Useful interpolated functions for [styled-components](https://github.com/styled-components/styled-components) 💅

## Install

    $ npm install --save styled-tools

## Usage

Play with it on [WebpackBin](https://www.webpackbin.com/bins/-Kel3KgddZSrD5oK0fIk)

```jsx
import styled, { css } from 'styled-components'
import { prop, ifProp, switchProp } from 'styled-tools'

const Button = styled.button`
  color: ${prop('color', 'red')};
  font-size: ${ifProp({ size: 'large' }, '20px', '14px')};
  background-color: ${switchProp('theme', {
    dark: 'blue', 
    darker: 'mediumblue', 
    darkest: 'darkblue' 
  })};
`

// renders with color: blue
<Button color="blue" />

// renders with color: red
<Button />

// renders with font-size: 20px
<Button size="large" />

// renders with background-color: mediumblue
<Button theme="darker" />
```

A more complex example:

```jsx
const Button = styled.button`
  color: ${prop('theme.colors.white', '#fff')};
  font-size: ${ifProp({ size: 'large' }, prop('theme.sizes.lg', '20px'), prop('theme.sizes.md', '14px'))};
  background-color: ${prop('theme.colors.black', '#000')};
  
  ${switchProp('kind', {
    dark: css`
      background-color: ${prop('theme.colors.blue', 'blue')};
      border: 1px solid ${prop('theme.colors.blue', 'blue')};
    `,
    darker: css`
      background-color: ${prop('theme.colors.mediumblue', 'mediumblue')};
      border: 1px solid ${prop('theme.colors.mediumblue', 'mediumblue')};
    `,
    darkest: css`
      background-color: ${prop('theme.colors.darkblue', 'darkblue')};
      border: 1px solid ${prop('theme.colors.darkblue', 'darkblue')};
    `,
  })}
  
  ${ifProp('disabled', css`
    background-color: ${prop('theme.colors.gray', '#999')};
    border-color: ${prop('theme.colors.gray', '#999')};
    pointer-events: none;
  `)}
`
```

## API

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### prop

Returns the value of `props[path]` or `defaultValue`

**Parameters**

-   `path` **([string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) \| [Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)>)** 
-   `defaultValue` **any** 

**Examples**

```javascript
const Button = styled.button`
 color: ${prop('color', 'red')};
`
```

Returns **any** 

### ifProp

Returns `pass` if prop is truthy. Otherwise returns `fail`

**Parameters**

-   `needle` **([string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) \| [Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)> | [Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object))** 
-   `pass` **any** 
-   `fail` **any** 

**Examples**

```javascript
// usage with styled-theme
import styled from 'styled-components'
import { ifProp } from 'styled-tools'
import { palette } from 'styled-theme'

const Button = styled.button`
 background-color: ${ifProp('transparent', 'transparent', palette(0))};
 color: ${ifProp(['transparent', 'accent'], palette('secondary', 0))};
 font-size: ${ifProp({ size: 'large' }, '20px', ifProp({ size: 'medium' }, '16px', '12px'))};
`
```

Returns **any** 

### switchProp

Switches on a given prop. Returns the value or function for a given prop value.

**Parameters**

-   `needle` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 
-   `switches` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** 

**Examples**

```javascript
import styled, { css } from 'styled-components'
import { switchProp } from 'styled-tools'

const Button = styled.button`
 font-size: ${switchProp('size', {
   small: prop('theme.sizes.sm', '12px'),
   large: prop('theme.sizes.lg', '20px'),
   default: prop('theme.sizes.md', '16px'),
 })};
 ${switchProp('theme.kind', {
   light: css`
     color: LightBlue;
   `,
   dark: css`
     color: DarkBlue;
   `,
 })}
`

<Button size="large" theme={{ kind: 'light' }} />
```

Returns **any** 

### call

Calls a function passing properties values as arguments.

**Parameters**

-   `fn` **[Function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function)** 
-   `propFns` **...[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[Function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function)>** 

**Examples**

```javascript
// example with polished
import styled from 'styled-components'
import { darken } from 'polished'
import { call, prop } from 'styled-tools'

const Button = styled.button`
 border-color: ${call(darken(0.5), prop('theme.primaryColor', 'blue'))};
`
```

Returns **any** 

## Related

-   [styled-theme](https://github.com/diegohaz/styled-theme) - Extensible theming system for styled-components

## License

MIT © [Diego Haz](https://github.com/diegohaz)
