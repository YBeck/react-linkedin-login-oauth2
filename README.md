# React Linked In Login Using OAuth 2.0

[![npm package][npm-badge]][npm]

[npm-badge]: https://img.shields.io/npm/v/react-linkedin-oauth2.png
[npm]: https://www.npmjs.org/package/react-linkedin-oauth2

Demo: https://stupefied-goldberg-b44ee5.netlify.com/

This package is used to get authorization code for Linked In Log in feature using OAuth2 in a easy way, without redirecting your application to linked in authorization page. After have the authorization code, you can send it to server to continue to get information needed. For more, please see at [Authenticating with OAuth 2.0 - Linked In](https://developer.linkedin.com/docs/oauth2)  
See `demo/src/index.js` for examples.

## Table of contents

- [Installation](#installation)
- [Overview](#overview)
- [Usage](#usage)
- [Demo](#demo)
- [Props](#props)
- [Contribution](#contribution)
- [Issues](#issues)

## Installation

```
npm i react-linkedin-oauth2
```

## Overview

We will create a Linked In button (using `LinkedIn` component), after clicking on this button, a popup window will show up and ask for the permission. After we accepted, the pop up window will redirect to a specified URI which should be routed to `LinkedInPopUp` component. It has responsible to notice our openning app the authorization code Linked In provides us. You can consider using `react-router-dom` as a possible solution.

## Usage

First, we create a button and provide required props

```
import React, { Component } from 'react';

import { LinkedIn } from 'react-linkedin-oauth2';

class LinkedInPage extends Component {
  state = {
    code: '',
    errorMessage: '',
  };


  handleSuccess = (data) => {
    this.setState({
      code: data.code,
      errorMessage: '',
    });
  }

  handleFailure = (error) => {
    this.setState({
      code: '',
      errorMessage: error.errorMessage,
    });
  }

  render() {
    const { code, errorMessage } = this.state;
    return (
      <div>
        <LinkedIn
          clientId="81lx5we2omq9xh"
          onFailure={this.handleFailure}
          onSuccess={this.handleSuccess}
          redirectUri="http://localhost:3000/linkedin"
        >
          <img src={require('./assets/linkedin.png')} alt="Log in with Linked In" style={{ maxWidth: '180px' }} />
        </LinkedIn>
        {!code && <div>No code</div>}
        {code && <div>Code: {code}</div>}
        {errorMessage && <div>{errorMessage}</div>}
      </div>
    );
  }
}

export default LinkedInPage;
```

Usage with custom button

```
<LinkedIn
  clientId="81lx5we2omq9xh"
  onFailure={this.handleFailure}
  onSuccess={this.handleSuccess}
  redirectUri="http://localhost:3000/linkedin"
  renderElement={({ onClick, disabled }) => (
    <button onClick={onClick} disabled={disabled}>Custom linkedin element</button>
  )}
/>
```

Then we define a route to `redirect_url` and pass `LinkedInPopUp` to it as follow:

```
import React, { Component } from 'react';
import { LinkedInPopUp } from 'react-linkedin-oauth2';

import { render } from 'react-dom';
import { BrowserRouter, Route, Switch } from 'react-router-dom';
import LinkedInPage from './LinkedInPage';

class Demo extends Component {
  render() {
    return (
      <BrowserRouter>
        <Switch >
          <Route exact path="/linkedin" component={LinkedInPopUp} />
          <Route path="/" component={LinkedInPage} />
        </Switch>
      </BrowserRouter>
    );
  }
}
```

## Demo

You can see demo via this [https://github.com/YBeck/react-linkedin-login-oauth2/tree/master/demo/src](https://github.com/YBeck/react-linkedin-login-oauth2/tree/master/demo/src)  
Demo 1: [Use react-router-dom](https://github.com/YBeck/react-linkedin-login-oauth2/blob/master/demo/src/index.js)  
Demo 2: [Not use react-router-dom](https://github.com/YBeck/react-linkedin-login-oauth2/blob/master/demo/src/index1.js)  
Or run

```
git clone https://github.com/YBeck/react-linkedin-login-oauth2.git
cd react-linkedin-login-oauth2
npm start
```

## Props

`LinkedIn` component:

| Parameter     | value    | is required |                             default                             |
| ------------- | -------- | :---------: | :-------------------------------------------------------------: |
| clientId      | string   |     yes     |                                                                 |
| redirectUri   | string   |     yes     |                                                                 |
| scope         | string   |     no      | Default member permissions defined in application configuration |
| state         | string   |     no      |                          fdsf78fyds7fm                          |
| onSuccess     | function |     yes     |                                                                 |
| onFailure     | function |     yes     |                                                                 |
| className     | string   |     no      |                         'btn-linkedin'                          |
| disabled      | boolean  |     no      |                              false                              |
| onClick       | function |     no      |                                                                 |
| children      | function |     no      |                     Linked in Signin button                     |
| renderElement | function |     no      |     Render prop to use a custom element, use props.onClick      |

Read more about props here [https://developer.linkedin.com/docs/oauth2](https://developer.linkedin.com/docs/oauth2)

`LinkedinPopUp` component:  
No parameters needed

## Known issue

- `react-linkedin-oauth2` currently doesn't support IE 11.`

## Credit

Based on react-linkedin-login-oauth2 by Hung Viet Nguyen
