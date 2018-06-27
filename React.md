# JSX

Instead of artificially separating *technologies* by putting markup and logic in separate files, React [separates *concerns*](https://en.wikipedia.org/wiki/Separation_of_concerns) with loosely coupled units called “components” that contain both

```js
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;
```

## Specifying The React Element Type

### React Must Be in Scope

```js
// even there is no explicit React in the code, the JSX will need the React
import React from 'react';
import CustomButton from './CustomButton';

function WarningButton() {
  // return React.createElement(CustomButton, {color: 'red'}, null);
  return <CustomButton color="red" />;
}
```



### Using Dot Notation for JSX Type

```js
import React from 'react';

const MyComponents = {
  DatePicker: function DatePicker(props) {
    return <div>Imagine a {props.color} datepicker here.</div>;
  }
}

function BlueDatePicker() {
  return <MyComponents.DatePicker color="blue" />;
}
```

### User-Defined Components Must Be Capitalized

```js
import React from 'react';

// Correct! This is a component and should be capitalized:
function Hello(props) {
  // Correct! This use of <div> is legitimate because div is a valid HTML tag:
  return <div>Hello {props.toWhat}</div>;
}

function HelloWorld() {
  // Correct! React knows <Hello /> is a component because it's capitalized.
  return <Hello toWhat="World" />;
}
```

### Choosing the Type at Runtime

```js
import React from 'react';
import { PhotoStory, VideoStory } from './stories';

const components = {
  photo: PhotoStory,
  video: VideoStory
};

function Story(props) {
  // Correct! JSX type can be a capitalized variable.
  const SpecificStory = components[props.storyType];
  return <SpecificStory story={props.story} />;
}
```

### Spread Attributes

```js
function App1() {
  return <Greeting firstName="Ben" lastName="Hector" />;
}

function App2() {
  const props = {firstName: 'Ben', lastName: 'Hector'};
  return <Greeting {...props} />;
}
```

## Children in JSX

In JSX expressions that contain both an opening tag and a closing  tag, the content between those tags is passed as a special prop: `props.children`. 

# Rendering Elements

1. Elements are what components are “made of”
2. React elements are [immutable](https://en.wikipedia.org/wiki/Immutable_object). Once you create an element, you can’t change its children or  attributes. An element is like a single frame in a movie: it represents  the UI at a certain point in time.
3. In our experience, thinking about how the UI should look at any given  moment rather than how to change it over time eliminates a whole class  of bugs.

# Components and Props

1. Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.

2. Conceptually, components are like JavaScript functions. They accept  arbitrary inputs (called “props”) and return React elements describing  what should appear on the screen

3. Always start component names with a capital letter

4. We recommend naming props from the component’s own point of view rather than the context in which it is being used.

5. Whether you declare a component [as a function or a class](https://reactjs.org/docs/components-and-props.html#functional-and-class-components), it must never modify its own props

   

   ```js
   function Welcome(props) {
     return <h1>Hello, {props.name}</h1>;
   }
   ```

   This function is a valid React component because it accepts a single  “props” (which stands for properties) object argument with data and  returns a React element. We call such components “functional” because  they are literally JavaScript functions.

```js
// a full process of defining, creating and rendering a functional component
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

# State and Lifecycle

1. State is similar to props, but it is private and fully controlled by the component.

```js
// a class component (which can fulfil a state)
class ClassComponent extends React.component{
    constructor(props){
        super(props) // use React.component methods to use props
        this.state = {
            stateName: <property>
        // This binding is necessary to make `this` work in the callback
    	this.<function_name> = this.<function_name>.bind(this);
        }
    }
        render(){
            return (
                <JSX Syntax>
            )
        }
}
    
// in later time update the state by the component.setState({<state name>:<property>})
// .setState() will update the UI automatically
    this.setState({
        stateName:<property>
    })
```

## Using State Correctly

### Do Not Modify State Directly

```js
// Wrong, it won't re-render the content
this.state.comment = 'Hello';
// Correct
this.setState({comment:"Hello"})
```

### State Updates May Be Asynchronous

Because `this.props` and `this.state` may be updated asynchronously, you should not rely on their values for calculating the next state.

```js
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

To fix it, use a second form of `setState()` that accepts a  function rather than an object. That function will receive the previous  state as the first argument, and the props at the time the update is  applied as the second argument:

```js
// Correct
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```

### State Updates are Merged

When you call `setState()`, React merges the object you provide into the current state.

## The Data Flows Down

1. Neither parent nor child components can know if a certain component is  stateful or stateless, and they shouldn’t care whether it is defined as a function or a class.
2. This is commonly called a “top-down” or “unidirectional” data flow. Any  state is always owned by some specific component, and any data or UI  derived from that state can only affect components “below” them in the  tree.

# Handling Events

- React events are named using camelCase, rather than lowercase.
- With JSX you pass a function as the event handler, rather than a string.
- you cannot return `false` to prevent default behavior in React. You must call `preventDefault` explicitly

```js
// html
<button onclick="activateLasers()">
  Activate Lasers
</button>

//react
<button onClick={activateLasers}>
  Activate Lasers
</button>

// html prevent default
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>

// react prevent default

function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```

If calling `bind` annoys you, there are two ways you can get around this. If you are using the experimental [public class fields syntax](https://babeljs.io/docs/plugins/transform-class-properties/), you can use class fields to correctly bind callbacks:

```js
// recommended
class LoggingButton extends React.Component {
  // This syntax ensures `this` is bound within handleClick.
  // Warning: this is *experimental* syntax.
  handleClick = () => {
    console.log('this is:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}

// if the callback is not passed to a lower components
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // This syntax ensures `this` is bound within handleClick
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
}
```

## Passing Arguments to Event Handlers

Inside a loop it is common to want to pass an extra parameter to an event handler. For example, if `id` is the row ID, either of the following would work:

```js
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

# Conditional Rendering

In React, you can create distinct components that encapsulate behavior  you need. Then, you can render only some of them, depending on the state of your application.

### Inline If with Logical && Operator

```js
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}
```

### Inline If-Else with Conditional Operator

```js
render() {
  // use local variable to store elements for later use
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
    </div>
  );
}
```

### Preventing Component from Rendering

In rare cases you might want a component to hide itself even though it was rendered by another component. To do this return `null` instead of its render output.

# Lists and Keys

### Basic List Component

```js
const todoItems = todos.map((todo, index) =>
  // Only do this if items have no stable IDs
  <li key={index}>
    {todo.text}
  </li>
);
// A good rule of thumb is that elements inside the map() call need keys.
```

### Embedding map() in JSX

```js
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) =>
        <ListItem key={number.toString()}
                  value={number} />
      )}
    </ul>
  );
}
```

Sometimes this results in clearer code, but this style can also be  abused. Like in JavaScript, it is up to you to decide whether it is  worth extracting a variable for readability

# Forms

In most cases, it’s convenient to have a JavaScript function that  handles the submission of the form and has access to the data that the  user entered into the form. The standard way to achieve this is with a  technique called “controlled components”.

## Controlled Components

In HTML, form elements such as `<input>`, `<textarea>`, and `<select>` typically maintain their own state and update it based on user input.  In React, mutable state is typically kept in the state property of  components, and only updated with [`setState()`](https://reactjs.org/docs/react-component.html#setstate).

We can combine the two by making the React state be the “single source  of truth”. Then the React component that renders a form also controls  what happens in that form on subsequent user input. An input form  element whose value is controlled by React in this way is called a  “controlled component”.

```js
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

## The textarea Tag

In React, a `<textarea>` uses a `value` attribute instead. This way, a form using a 

```js
class EssayForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: 'Please write an essay about your favorite DOM element.'
    };

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('An essay was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Essay:
          <textarea value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

## The select Tag

React, instead of using this `selected` attribute, uses a `value` attribute on the root `select` tag. This is more convenient in a controlled component because you only need to update it in one place

```js
class FlavorForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: 'coconut'};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('Your favorite flavor is: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Pick your favorite La Croix flavor:
          <select value={this.state.value} onChange={this.handleChange}>
            <option value="grapefruit">Grapefruit</option>
            <option value="lime">Lime</option>
            <option value="coconut">Coconut</option>
            <option value="mango">Mango</option>
          </select>
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
// You can pass an array into the value attribute, allowing you to select multiple options in a select tag:
<select multiple={true} value={['B', 'C']}>
```

## Handling Multiple Inputs

When you need to handle multiple controlled `input` elements, you can add a `name` attribute to each element and let the handler function choose what to do based on the value of `event.target.name`.

```js
class Reservation extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isGoing: true,
      numberOfGuests: 2
    };

    this.handleInputChange = this.handleInputChange.bind(this);
  }

  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          Is going:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          Number of guests:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```

## Uncontrolled Components

In most cases, we recommend using [controlled components](https://reactjs.org/docs/forms.html)  to implement forms. In a controlled component, form data is handled by a  React component. The alternative is uncontrolled components, where form  data is handled by the DOM itself.

To write an uncontrolled component, instead of writing an event handler for every state update, you can [use a ref](https://reactjs.org/docs/refs-and-the-dom.html) to get form values from the DOM.

```js
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.handleSubmit = this.handleSubmit.bind(this);
    this.input = React.createRef();
  }

  handleSubmit(event) {
      // this.input is a DOM node; node.current.value is a DOM Api
    alert('A name was submitted: ' + this.input.current.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" ref={this.input} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

# Lifting State Up

Often, several components need to reflect the same changing data. We  recommend lifting the shared state up to their closest common ancestor

In React, sharing state is accomplished by moving it up to the closest  common ancestor of the components that need it. This is called “lifting  state up”

There should be a single “source of truth” for any data that changes in a React application. Usually, the state is first added to the component  that needs it for rendering. Then, if other components also need it, you can lift it up to their closest common ancestor. Instead of trying to  sync the state between different components, you should rely on the [top-down data flow](https://reactjs.org/docs/state-and-lifecycle.html#the-data-flows-down).

# Thinking in React

## Props or States?

Let’s go through each one and figure out which one is state. Simply ask three questions about each piece of data:

1. Is it passed in from a parent via props? If so, it probably isn’t state.
2. Does it remain unchanged over time? If so, it probably isn’t state.
3. Can you compute it based on any other state or props in your component? If so, it isn’t state.

## Where states should live

For each piece of state in your application:

- Identify every component that renders something based on that state.
- Find a common owner component (a single component above all the components that need the state in the hierarchy).
- Either the common owner or another component higher up in the hierarchy should own the state.
- If you can’t find a component where it makes sense to own the state,  create a new component simply for holding the state and add it  somewhere in the hierarchy above the common owner component.

# Context

Context provides a way to pass data through the component tree without having to pass props down manually at every level.

```js
// Context lets us pass a value deep into the component tree
// without explicitly threading it through every component.
// Create a context for the current theme (with "light" as the default).
const ThemeContext = React.createContext('light');

class App extends React.Component {
  render() {
    // Use a Provider to pass the current theme to the tree below.
    // Any component can read it, no matter how deep it is.
    // In this example, we're passing "dark" as the current value.
    return (
      <ThemeContext.Provider value="dark">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}

// A component in the middle doesn't have to
// pass the theme down explicitly anymore.
function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

function ThemedButton(props) {
  // Use a Consumer to read the current theme context.
  // React will find the closest theme Provider above and use its value.
  // In this example, the current theme is "dark".
  return (
    <ThemeContext.Consumer>
      {theme => <Button {...props} theme={theme} />}
    </ThemeContext.Consumer>
  );
}
```

Don’t use context just to avoid passing props a few levels down. Stick  to cases where the same data needs to be accessed in many components at  multiple levels.

# Refs and the DOM

Refs provide a way to access DOM nodes or React elements created in the render method.

In the typical React dataflow, [props](https://reactjs.org/docs/components-and-props.html)  are the only way that parent components interact with their children.  To modify a child, you re-render it with new props. However, there are a  few cases where you need to imperatively modify a child outside of the  typical dataflow. The child to be modified could be an instance of a  React component, or it could be a DOM element. For both of these cases,  React provides an escape hatch.

### When to Use Refs

There are a few good use cases for refs:

- Managing focus, text selection, or media playback.
- Triggering imperative animations.
- Integrating with third-party DOM libraries.

### Creating Refs

Refs are created using `React.createRef()` and attached to React elements via the `ref`  attribute. Refs are commonly assigned to an instance property when a  component is constructed so they can be referenced throughout the  component.

```js
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  render() {
    return <div ref={this.myRef} />;
  }
}Avoid using refs for anything that can be done declaratively.
```

### Accessing Refs

When a ref is passed to an element in `render`, a reference to the node becomes accessible at the `current` attribute of the ref.

```js
const node = this.myRef.current;
```

The value of the ref differs depending on the type of the node:

- When the `ref` attribute is used on an HTML element, the `ref` created in the constructor with `React.createRef()` receives the underlying DOM element as its `current` property.
- When the `ref` attribute is used on a custom class component, the `ref` object receives the mounted instance of the component as its `current`.
- **You may not use the ref attribute on functional components** because they don’t have instances.

```js
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    // create a ref to store the textInput DOM element
    this.textInput = React.createRef();
    this.focusTextInput = this.focusTextInput.bind(this);
  }

  focusTextInput() {
    // Explicitly focus the text input using the raw DOM API
    // Note: we're accessing "current" to get the DOM node
    this.textInput.current.focus();
  }

  render() {
    // tell React that we want to associate the <input> ref
    // with the `textInput` that we created in the constructor
    return (
      <div>
        <input
          type="text"
          ref={this.textInput} />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focusTextInput}
        />
      </div>
    );
  }
}
```

## Forwarding Refs

Ref forwarding is a technique for automatically passing a [ref](https://reactjs.org/docs/refs-and-the-dom.html) through a component to one of its children. This is typically not  necessary for most components in the application. However, it can be  useful for some kinds of components, especially in reusable component  libraries.

```js
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));

// You can now get a ref directly to the DOM button:
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;
```

# Fragments

A common pattern in React is for a component to return multiple  elements. Fragments let you group a list of children without adding  extra nodes to the DOM.

```js
render() {
  return (
    <React.Fragment> // <> this is experimentally supported
      <ChildA />
      <ChildB />
      <ChildC />
    </React.Fragment> // </> this is experimentally supported
  );
}
```

# Higher-Order Components

A higher-order component (HOC) is an advanced technique in React for  reusing component logic. HOCs are not part of the React API, per se.  They are a pattern that emerges from React’s compositional nature.

Concretely, **a higher-order component is a function that takes a component and returns a new component**(simply because all react components are just functions, higher order components are just like high order functions)

```js
const EnhancedComponent = higherOrderComponent(WrappedComponent);

//example 
const CommentListWithSubscription = withSubscription(
  CommentList,
  (DataSource) => DataSource.getComments()
);

const BlogPostWithSubscription = withSubscription(
  BlogPost,
  (DataSource, props) => DataSource.getBlogPost(props.id)
);


// This function takes a component...
function withSubscription(WrappedComponent, selectData) {
  // ...and returns another component...
  return class extends React.Component {
    constructor(props) {
      super(props);
      this.handleChange = this.handleChange.bind(this);
      this.state = {
        data: selectData(DataSource, props)
      };
    }

    componentDidMount() {
      // ... that takes care of the subscription...
      DataSource.addChangeListener(this.handleChange);
    }

    componentWillUnmount() {
      DataSource.removeChangeListener(this.handleChange);
    }

    handleChange() {
      this.setState({
        data: selectData(DataSource, this.props)
      });
    }

    render() {
      // ... and renders the wrapped component with the fresh data!
      // Notice that we pass through any additional props
      return <WrappedComponent data={this.state.data} {...this.props} />;
    }
  };
}
```

Note that a HOC doesn’t modify the input component, nor does it use inheritance to copy its behavior. Rather, a HOC *composes* the original component by *wrapping* it in a container component. A HOC is a pure function with zero side-effects !!!

HOCs add features to a component. They shouldn’t drastically alter its  contract. It’s expected that the component returned from a HOC has a  similar interface to the wrapped component.

## Caveats

1. Don’t Use HOCs Inside the render Method

2. ### Static Methods Must Be Copied Over

3. ### Refs Aren’t Passed Through

# Optimizing Performance

## shouldComponentUpdate

![should component update](https://reactjs.org/static/should-component-update-5ee1bdf4779af06072a17b7a0654f6db-9a3ff.png)

Since `shouldComponentUpdate` returned `false` for the subtree rooted at C2, React did not attempt to render C2, and thus didn’t even have to invoke `shouldComponentUpdate` on C4 and C5.

For C1 and C3, `shouldComponentUpdate` returned `true`, so React had to go down to the leaves and check them. For C6 `shouldComponentUpdate` returned `true`, and since the rendered elements weren’t equivalent React had to update the DOM.

The last interesting case is C8. React had to render this component,  but since the React elements it returned were equal to the previously  rendered ones, it didn’t have to update the DOM.

Note that React only had to do DOM mutations for C6, which was  inevitable. For C8, it bailed out by comparing the rendered React  elements, and for C2’s subtree and C7, it didn’t even have to compare  the elements as we bailed out on `shouldComponentUpdate`, and `render` was not called.

```js
class CounterButton extends React.Component {
  constructor(props) {
    super(props);
    this.state = {count: 1};
  }

  shouldComponentUpdate(nextProps, nextState) {
    // this is a shallow comparision
    if (this.props.color !== nextProps.color) {
      return true;
    }
    if (this.state.count !== nextState.count) {
      return true;
    }
    return false;
  }

  render() {
    return (
      <button
        color={this.props.color}
        onClick={() => this.setState(state => ({count: state.count + 1}))}>
        Count: {this.state.count}
      </button>
    );
  }
}

// this is equal to the example above with the PureComponent
class CounterButton extends React.PureComponent {
  constructor(props) {
    super(props);
    this.state = {count: 1};
  }

  render() {
    return (
      <button
        color={this.props.color}
        onClick={() => this.setState(state => ({count: state.count + 1}))}>
        Count: {this.state.count}
      </button>
    );
  }
}
```

##  Not Mutating Data

```js
handleClick() {
  this.setState(prevState => ({
    words: [...prevState.words, 'marklar'],
  }));
};

function updateColorMap(colormap) {
  return {...colormap, right: 'blue'};
}
```

## Using Immutable Data Structures

[Immutable.js](https://github.com/facebook/immutable-js) is another way to solve this problem. It provides immutable, persistent collections that work via structural sharing:

- *Immutable*: once created, a collection cannot be altered at another point in time.
- *Persistent*: new collections can be created from a previous  collection and a mutation such as set. The original collection is still  valid after the new collection is created.
- *Structural Sharing*: new collections are created using as  much of the same structure as the original collection as possible,  reducing copying to a minimum to improve performance.

# Portals

Portals provide a first-class way to render children into a DOM node  that exists outside the DOM hierarchy of the parent component.

```js
ReactDOM.createPortal(child, container)
```

The first argument (`child`) is any [renderable React child](https://reactjs.org/docs/react-component.html#render), such as an element, string, or fragment. The second argument (`container`) is a DOM element.

```js
render() {
  // React does *not* create a new div. It renders the children into `domNode`.
  // `domNode` is any valid DOM node, regardless of its location in the DOM.
  return ReactDOM.createPortal(
    this.props.children,
    domNode
  );
}
```

# Render Props

The term [“render prop”](https://cdb.reacttraining.com/use-a-render-prop-50de598f11ce) refers to a simple technique for sharing code between React components using a prop whose value is a function.

A component with a render prop takes a function that returns a React  element and calls it instead of implementing its own render logic.

More concretely, **a render prop is a function prop that a component uses to know what to render.**

```js
<DataProvider render={data => (
  <h1>Hello {data.target}</h1>
)}/>
```

```js
class Cat extends React.Component {
  render() {
    const mouse = this.props.mouse;
    return (
      <img src="/cat.jpg" style={{ position: 'absolute', left: mouse.x, top: mouse.y }} />
    );
  }
}

class Mouse extends React.Component {
  constructor(props) {
    super(props);
    this.handleMouseMove = this.handleMouseMove.bind(this);
    this.state = { x: 0, y: 0 };
  }

  handleMouseMove(event) {
    this.setState({
      x: event.clientX,
      y: event.clientY
    });
  }

  render() {
    return (
      <div style={{ height: '100%' }} onMouseMove={this.handleMouseMove}>

        {/*
          Instead of providing a static representation of what <Mouse> renders,
          use the `render` prop to dynamically determine what to render.
        */}
        {this.props.render(this.state)}
      </div>
    );
  }
}

class MouseTracker extends React.Component {
  render() {
    return (
      <div>
        <h1>Move the mouse around!</h1>
        <Mouse render={mouse => (
          <Cat mouse={mouse} />
        )}/>
      </div>
    );
  }
}
```

### Be careful when using Render Props with React.PureComponent

Using a render prop can negate the advantage that comes from using [`React.PureComponent`](https://reactjs.org/docs/react-api.html#reactpurecomponent) if you create the function inside a `render` method. This is because the shallow prop comparison will always return `false` for new props, and each `render` in this case will generate a new value for the render prop.

To get around this problem, you can sometimes define the prop as an instance method, like so:

```js
class MouseTracker extends React.Component {
  // Defined as an instance method, `this.renderTheCat` always
  // refers to *same* function when we use it in render
  renderTheCat(mouse) {
    return <Cat mouse={mouse} />;
  }

  render() {
    return (
      <div>
        <h1>Move the mouse around!</h1>
        <Mouse render={this.renderTheCat} />
      </div>
    );
  }
}
```

In cases where you cannot define the prop statically (e.g. because you need to close over the component’s props and/or state) `<Mouse>` should extend `React.Component` instead.

# Strict Mode

`StrictMode` is a tool for highlighting potential problems in an application. Like `Fragment`, `StrictMode` does not render any visible UI. It activates additional checks and warnings for its descendants.

> Note:
>
> Strict mode checks are run in development mode only; *they do not impact the production build*.

You can enable strict mode for any part of your application. For example: 

```js
import React from 'react';

function ExampleApplication() {
  return (
    <div>
      <Header />
      <React.StrictMode>
        <div>
          <ComponentOne />
          <ComponentTwo />
        </div>
      </React.StrictMode>
      <Footer />
    </div>
  );
}
```

In the above example, strict mode checks will *not* be run against the `Header` and `Footer` components. However, `ComponentOne` and `ComponentTwo`, as well as all of their descendants, will have the checks.

`StrictMode` currently helps with:

- [Identifying components with unsafe lifecycles](https://reactjs.org/docs/strict-mode.html#identifying-unsafe-lifecycles)
- [Warning about legacy string ref API usage](https://reactjs.org/docs/strict-mode.html#warning-about-legacy-string-ref-api-usage)
- [Detecting unexpected side effects](https://reactjs.org/docs/strict-mode.html#detecting-unexpected-side-effects)
- [Detecting legacy context API](https://reactjs.org/docs/strict-mode.html#detecting-legacy-context-api)

### Identifying unsafe lifecycles

As explained [in this blog post](https://reactjs.org/blog/2018/03/27/update-on-async-rendering.html),  certain legacy lifecycle methods are unsafe for use in async React  applications. However, if your application uses third party libraries,  it can be difficult to ensure that these lifecycles aren’t being used.  Fortunately, strict mode can help with this!

### Detecting unexpected side effects

Conceptually, React does work in two phases:

- The **render** phase determines what changes need to be made to e.g. the DOM. During this phase, React calls `render` and then compares the result to the previous render.
- The **commit** phase is when React applies any changes.  (In the case of React DOM, this is when React inserts, updates, and  removes DOM nodes.) React also calls lifecycles like `componentDidMount` and `componentDidUpdate` during this phase.

The commit phase is usually very fast, but rendering can be slow. For  this reason, the upcoming async mode (which is not enabled by default  yet) breaks the rendering work into pieces, pausing and resuming the  work to avoid blocking the browser. This means that React may invoke  render phase lifecycles more than once before committing, or it may  invoke them without committing at all (because of an error or a higher  priority interruption).

Render phase lifecycles include the following class component methods:

- `constructor`
- `componentWillMount`
- `componentWillReceiveProps`
- `componentWillUpdate`
- `getDerivedStateFromProps`
- `shouldComponentUpdate`
- `render`
- `setState` updater functions (the first argument)

Because the above methods might be called more than once, it’s  important that they do not contain side-effects. Ignoring this rule can  lead to a variety of problems, including memory leaks and invalid  application state. Unfortunately, it can be difficult to detect these  problems as they can often be [non-deterministic](https://en.wikipedia.org/wiki/Deterministic_algorithm).

Strict mode can’t automatically detect side effects for you, but it  can help you spot them by making them a little more deterministic. This  is done by intentionally double-invoking the following methods:

- Class component `constructor` method
- The `render` method
- `setState` updater functions (the first argument)
- The static `getDerivedStateFromProps` lifecycle