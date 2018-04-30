# rouct

> Micro client-side routing component for React.

[![npm](https://img.shields.io/npm/v/rouct.svg?style=flat-square)](https://www.npmjs.com/package/rouct)
[![npm](https://img.shields.io/npm/dt/rouct.svg?style=flat-square)](https://www.npmjs.com/package/rouct)
[![npm](https://img.shields.io/npm/l/rouct.svg?style=flat-square)](https://github.com/jmjuanes/rouct)
[![pr](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)]()

**NOTE**: this package is on development.  

**Rouct** is a micro client-side routing component for building **single page applications** (SPA for short) with [React](https://www.reactjs.org). 

It uses the **hashbang** as the routing strategy. This strategy adds an exclamation mark after the hash to indicate that the hash is used for routing. A tipically url with a *hashbang* looks like: 

```
http://example.com/#!/about/contact
```
 

## Installation 

Use `npm` to install the package:

```bash
$ npm install --save rouct
```

## Example 

First of all, you should import `rouct` in your ES6 module:

```javascript
import * as Rouct from "rouct";
```

### Example using hashbang navigation method

Create

```jsx
class MyApp extends Rouct.App {
    
}
```

Implement the `render()` method in your new class. Use the `Rouct.Switch` component to find the route that matches the current location, and the `Rouct.Route` component to define which componet will be rendered when the current location matches the route's path. 

```jsx
class MyApp extends Rouct.App {
    render() {
        return (
            <Rouct.Switch>
                <Rouct.Route exact path="/" component={HomePage}/>
            </Rouct.Switch>
        );
    }
}
``` 

Now implement your `HomePage` component that will be rendered when the user navigates to the root of your website:

```jsx
class HomePage extends React.Component {
    render() {
        return <div>Hello world!</div>
    }
}
```


## API 

### Rouct.Router

The base component that is used by the router navigation strategies. Typically you should use `HashbangRouter` instead. Only use this component if you are going to implement your own navigation strategy.

This component accepts the following props:

- `location`: a `string` with the current location. The `Router` component will propague this location its `Route` and `Switch` children components.

```jsx
class App extends React.Component {
    constructor(super) {
        super(props);
        this.state = {location: "/"};
    }
    
    render() {
        return (
            <Rouct.Router location={this.state.location}>
                <Rouct.Route path="/" component={Home}/>
            </Rouct.Router>
    }
}
```

### Rouct.Switch

A component that renders the first child `Rouct.Route` that matches the current location. 

```jsx
<Rouct.Switch>
    /* Insert here your routes */
</Rouct.Switch>
```

### Rouct.Route

A React component that is used to assign a path to a component that should be rendered if the current path matches the route path.

```jsx
<Rouct.Switch>
    <Rouct.Route exact path="/" component={HomePage}/>
    <Rouct.Route exact path="/about" component={AboutPage}/>
    <Rouct.Route exact path="/portfolio" component={PortfolioPage}/>
</Rouct.Switch>
```

> **Important note**: The `Rouct.Route` component should always be used inside a `Rouct.Switch` component. Otherwise it will render nothing.

The `Rouct.Route` component expects the following props:

##### `path`

A `string` that describes the pathname that the route matches. When the current location matches the route's path, the component specified by the `component` prop is rendered.

```jsx
<Rouct.Route path="/about/contact" component={ContactPage}/>
```

The `path` can also be a dynamic path string, using named parameters prefixed by a colon to the parameter name. For example, the path `/user/:name` matches `/user/bob` and `user/susan`. The captured values are stored in the `request.params` object passed as a property to the rendered component.

```jsx
<Rouct.Route path="/user/:name" component={UserManagement}/>
```

You can also create a catch-all route setting the `path` value to `*`:

```jsx
<Rouct.Route path="*" component={NotFoundPage}/>
``` 

> **Important note**: query strings are not part of the route path.
 

##### `component`

A React component that should be rendered when the route matches. This component will be called with the props passed from the `props` property of the Route. Also, a `request` object will be passed as a prop with the following entries: 

- `path`: a `string` with the full matched path.
- `pathname`: a `string` with the part of the URL before the start of the `query`.
- `query`: an `object` that contains all query-string parameters in the matched path. Default is an empty object `{}`.
- `params`: an `object` that contains all parsed properties extracted from the dynamic parts of the path. Default is an empty object `{}`.

##### `props`

**Optionally** An `object` with the extra props that should be passed to the rendered `component` when the route matches.

```jsx
let homeProps = {
    title: "Hello world!"
};
<Rouct.Route path="/" component={HomePage} props={homeProps}/>
```

##### `exact`

**Optionally** A `boolean` used to ensure that the route's path is an exact match of the current location. Default is `false`.

| Router's path | Current path | Is Exact active? | Matches? |
|---------------|--------------|------------------|----------|
| `/`           | `/one/two`   | Yes              | No       |
| `/`           | `/one/two`   | No               | Yes      | 
| `/one`        | `/one/two`   | Yes              | No       |
| `/one/`       | `/one/two`   | No               | Yes      |
| `/one/two`    | `/one/two`   | Yes              | Yes      |
| `/one/two`    | `/one/two`   | No               | Yes      |


### Rouct.redirect(url)

Use this function to change the hash segment with the provided path. This function automatically adds the exclamation mark to the path after the hash.  

```jsx
class Menu extends React.Component {
    render() {
        return (
            <div className="menu">
                <a className="menu-link" onClick={() => Rouct.redirect("/");}>Home</a>
                <a className="menu-link" onClick={() => Rouct.redirect("/about");}>About</a>
                <a className="menu-link" onClick={() => Rouct.redirect("/portfolio");}>Portfolio</a>
            </div>
        );
    }
}
```


## License

Released under the [MIT LICENSE](./LICENSE).

