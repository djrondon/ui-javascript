# Maple Inside React Style Guide

> Efficient and readable React.

Other style guides:
 - [UI JavaScript](https://github.com/mapleinside/ui-javascript)
 - [CSS & Sass](https://github.com/mapleinside/css)

## Table of Contents

  1. [Basic Rules](#basic-rules)
  1. [Naming](#naming)
  1. [Declaration](#declaration)
  1. [Alignment](#alignment)
  1. [Quotes](#quotes)
  1. [Spacing](#spacing)
  1. [Props](#props)
  1. [Parentheses](#parentheses)
  1. [Tags](#tags)
  1. [Methods](#methods)
  1. [Ordering](#ordering)

## Basic Rules

  - Only include one React component per file;
  - always use JSX syntax;
  - do not use `React.createElement` unless you're initializing the app from a file that is not JSX.

## Class vs React.createClass

  - Use `class extends React.Component`. Mixins are not an argument.

  ```javascript
  // Bad.
  const Listing = React.createClass({
    render() {
      return <div />;
    }
  });
  
  // Good.
  class Listing extends React.Component {
    render() {
      return <div />;
    }
  }
  ```

## Naming

  - **Extensions**: use `.js` extension for React components as well;
  - **Filename**: use PascalCase for filenames. E.g., `ReservationCard.js`;
  - **Reference naming**: use PascalCase for React components and camelCase for their instances:
  
  ```javascript
  // Bad.
  const reservationCard = require('./ReservationCard');
  
  // Good.
  const ReservationCard = require('./ReservationCard');
  
  // Bad.
  const ReservationItem = <ReservationCard />;
  
  // Good.
  const reservationItem = <ReservationCard />;
  ```

  **Component naming**: use the filename as the component name. For example, `ReservationCard.js` should have a reference name of `ReservationCard`. However, for root components of a directory, use `index.js` as the filename and use the directory name as the component name:
    
  ```javascript
  // Bad.
  const Footer = require('./Footer/Footer.js');
  
  // Bad.
  const Footer = require('./Footer/index.js');
  
  // Good.
  const Footer = require('./Footer');
  ```

## Declaration

  - Do not use displayName for naming components. Instead, name the component by reference.

  ```javascript
  // Bad.
  export default React.createClass({
    displayName: 'ReservationCard',
    ...
  });
  
  // Good.
  export default class ReservationCard extends React.Component {
  }
  ```

## Alignment

  - Follow these alignment styles for JSX syntax:

  ```javascript
  // Bad.
  <Foo superLongParam="bar"
       anotherSuperLongParam="baz" />
  
  // Good.
  <Foo
    superLongParam="bar"
    anotherSuperLongParam="baz"
  />
  
  // If props fit in one line then keep it on the same line.
  <Foo bar="bar" />
  
  // Children get indented normally.
  <Foo
    superLongParam="bar"
    anotherSuperLongParam="baz"
  >
    <Spazz />
  </Foo>
  ```

## Quotes

  - Always use double quotes (`"`) for JSX attributes, but single quotes for all other JS.

  > Why? JSX attributes [can't contain escaped quotes](http://eslint.org/docs/rules/jsx-quotes), so double quotes make conjunctions like `"don't"` easier to type.
  > Regular HTML attributes also typically use double quotes instead of single, so JSX attributes mirror this convention.

  ```javascript
  // Bad.
  <Foo bar='bar' />
  
  // Good.
  <Foo bar="bar" />
  
  // Bad.
  <Foo style={{left: "20px"}} />
  
  // Good.
  <Foo style={{left: '20px'}} />
  ```

## Spacing

  - Always include a single space in your self-closing tag.
  
  ```javascript
  // Bad.
  <Foo/>
  
  // Very bad.
  <Foo                 />
  
  // Bad.
  <Foo
   />
  
  // Good.
  <Foo />
  ```

## Props

  - Always use camelCase for prop names.
  
  ```javascript
  // Bad.
  <Foo
    UserName="hello"
    phone_number={12345678}
  />
  
  // Good.
  <Foo
    userName="hello"
    phoneNumber={12345678}
  />
  ```

## Parentheses

  - Wrap JSX tags in parentheses when they span more than one line:
  
  ```javascript
  // Bad.
  render() {
    return <MyComponent className="long body" foo="bar">
             <MyChild />
           </MyComponent>;
  }
  
  // Good.
  render() {
    return (
      <MyComponent className="long body" foo="bar">
        <MyChild />
      </MyComponent>
    );
  }
  
  // Good, when single line.
  render() {
    const body = <div>hello</div>;
    
    return <MyComponent>{body}</MyComponent>;
  }
  ```

## Tags

  - Always self-close tags that have no children.
  
  ```javascript
  // Bad.
  <Foo className="stuff"></Foo>
  
  // Good.
  <Foo className="stuff" />
  ```

  - If your component has multi-line properties, close its tag on a new line.
  
  ```javascript
  // Bad.
  <Foo
    bar="bar"
    baz="baz" />
  
  // Good.
  <Foo
    bar="bar"
    baz="baz"
  />
  ```

## Methods

  - Do not use underscore prefix for internal methods of a React component.
  
  ```javascript
  // Bad.
  React.createClass({
    _onClickSubmit() {
      ...
    }
  
    ...
  });
  
  // Good.
  class MyComponent extends React.Component {
    onClickSubmit() {
      ...
    }
  
    ...
  });
  ```

## Ordering

  - Ordering for class extends React.Component:
  
  1. Static properties
  1. Private properties
  1. Public properties
  1. constructor
  1. Optional static methods
  1. getChildContext
  1. componentWillMount
  1. componentDidMount
  1. componentWillReceiveProps
  1. shouldComponentUpdate
  1. componentWillUpdate
  1. componentDidUpdate
  1. componentWillUnmount
  1. *clickHandlers or eventHandlers* like onClickSubmit() or onChangeDescription()
  1. *Getter methods for render* like getSelectReason() or getFooterContent()
  1. *Optional render methods* like renderNavigation() or renderProfilePicture()
  1. render

  - How to define propTypes, defaultProps, contextTypes, etc.

  ```javascript
  import React, {Component, PropTypes} from 'react';
  
  export default class Link extends Component {
    static propTypes = {
      id: PropTypes.number.isRequired,
      url: PropTypes.string.isRequired,
      text: PropTypes.string,
    };
    
    static defaultProps = {
      text: 'Hello World',
    };
  
    static methodsAreOk() {
      return true;
    }
  
    render() {
      return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>
    }
  }
  ```

**[Back to top](#table-of-contents)**

## License

(The MIT License)

Copyright (c) 2015 Maple Inside

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[Back to top](#table-of-contents)**

## Amendments

We encourage you to fork this guide and change the rules to fit your team's style guide.
