# React.js
## Basics
Working with React means working with two separate libraries:
1) **React** - Has code which knows how to get components to work together (how to call a component function, take the JSX it returns and iterate over all the elements and decide to create an HTML element or call another component function). It's called a *reconciler*.
2) **ReactDOM** - Knows how to take JSX, turns it into HTML, and then puts the HTML into the DOM, which is then displayed in the browser. It's called a *renderer*.

When a link is entered into a web browser, then the browser sends a request to the server hosting the website for an `index.html` file. After `index.html` is received in the response, the browser parses the file, one of the `<script>` tags inside it tells the browser to make another request for a special JS file called `bundle.js`, which combines all the different JS files which make up the web application.

**Babel** is a library used to convert newer versions of JS (ES 2015, 2016 and onward) - which might not be supported by some browsers - into a version of JS which is supported by most browsers (ES5).

Using `import` instead of `require` to load in JS modules only specifies what kind of method is used to share the code between different JS files. ES2015 modules use `import`, whereas CommonJS modules use `require`.

The **React** way of returning multiple items from a function is to return them as an array, whereas the common JS way is to return them inside an object. Either way works.

A VM isn't needed for the deployment of a **React** application because it isn't executing any code on any server, but hosting plain static files (unlike the *Node JS API*, for example). This means it costs dramatically less to deploy a **React** application compared to applications which execute code, and thus need VMs.

To deploy with Vercel, first install the Vercel CLI using `npm install -g vercel`. After that, `cd` into the project directory, then do `vercel login`. Next, type `vercel` into the command line, and just press Enter to accept the default values since they work just fine for `create-react-app` projects. The app will then be deployed online. After making changes to the app, it can be redeployed with the `vercel --prod` command.

To deploy with Netlify, all work is done on their website. You must create a GitHub repo for the project, push the project onto the repo, then link your GitHub profile with Netlify. After that select **New site from Git**, select the repository you want to use, and you're done. The deployment URL is random initially, but can be changed in the settings afterwards. Netlify syncs with the GitHub repository, so the project is automatically deployed again after every push. Vercel can also be set up to track a GitHub repo.

___
## JSX
JSX syntax differs from HTML in a few ways. Properties inside tags are done differently, where JS variables in curly brackets are used in place of multiple entries inside double quotation marks. The first set of curly brackets indicates that a JS variable will be referenced, while the second set (inside the first) denotes a JS *object*. Note that compound attribute names like `'background-color'` are changed to camelcase (`'backgroundColor'`).

```javascript
<div style="border: 1px solid red;"></div>          // HTML
<div style={{ border: '1px solid red' }}></div>     // JSX
<button style="background-color: blue; color: white;">Submit</button>		// HTML
<button style={{ backgroundColor: 'blue', color: 'white' }}>Submit</button>	// JSX
```


The convention is to use double quotes for JSX properties and single quotes everywhere else.

```javascript
<label for="inputEmail">Email: </label>
```


When assigning a class to a tag for styling later on, `className` is used instead of `class`. The same applies to the `for` keyword used in the <label> tag, which is converted to `htmlFor`. Although they often result in code which generally works fine, JS keywords should be avoided and their substitutes should be used instead.

```javascript
<label class="myLabel" for='inputEmail'>Sample text</label>	    // HTML
<label className="myLabel" htmlFor='inputEmail'>Sample text</label> // JSX
```


JSX can show a lot of things like variables and things returned from functions, but it can't show objects where text is expected. However, it **can** show object properties, so `txt.msg` would work just fine whereas `txt` on its own would cause an error.

```javascript
const App = () => {
    return (
    	<button style={{ backgroundColor: 'blue', color:'white' }}>
	    {txt}
    	</button>
    );
}

txt = 12345
txt = 'TestMessage'		// Works
txt = ['Test', 'Message']	// Works
txt = { msg: 'TestMessage' }	// Doesn't work
```


___

## Components
A *React component* is a JS **function** or **class** that *returns some JSX* (which turns into HTML that is shown to the user) and *handles feedback from the user* (using event handlers). **JSX** is basically a set of instructions which tells **React** what to display on the screen. Before JSX is rendered in the browser, it first needs to be converted into regular JS code. Each tag in JSX is examined by checking if it's a *DOM* element (for example, a `<div>`). If so, the element is rendered. If it's a component, then the component function is called and all the JSX it returns is also inspected.

Three tenets of **React**:
1) **Component Nesting** - A component can be shown inside of another component
2) **Component Reusability** - Components should be easily reused throughout the application
3) **Component Configuration** - Components should be such so that we're able to configure them when they're created.

Creating a reusable, configurable component is done in a few steps:
1) Identify the JSX that is duplicated and that could be reused
2) Identify the purpose of that block of JSX and name the new component appropriately (eg. `Comment`)
3) Create a file with the same name as the chosen component name (`Comment.js`)
4) Place the selected JSX into the file and make it configurable by using the **props** system
5) The finished component should then have an `export default COMPONENTNAME`, in this case it would be `export default Comment`, after which it should be imported elsewhere with `import Comment from 'COMPONENT_PATH'` (with the appropriate component path)

The `ReactDOM.render()` function accepts two arguments:
1) The **React** component which is to be rendered (`<App />` in the example above)
2) The location inside the `index.html` file where the HTML from the component we selected will be placed into (`document.getElementById("root")` in the example above)

```javascript
ReactDOM.render(<App />, document.getElementById("root"));
```

___

## Props
When nesting components, the component which holds other components inside it is called the **parent component**, and the components inside it are its **children** (or **child components**). The **props** system (short for **properties**) is then used to pass data from a parent component to a child component, where the goal is to customize or configure the child component. It's all about having a parent customize how a child looks or behaves.

Information is passed from a parent to a child through props in a similar way to how HTML elements are given attributes. Default values for props can be set by using the OR operator after a prop, followed by the default value (like `<div>{props.text || 'Text placeholder'}</div>`).

```javascript
// Inside parent component
<Comment
    author="Mike" 
    text="Thanks for the info!" 
    postedAt={commentInfo.date} 
/>

// Definition of child component inside child component file
const Comment = props => {
    // Some code...
    <a href="/">{props.author}</a>
    <div>{props.text || 'Text placeholder'}</div>   // Default prop example
    // Some code...
}
```


A better way to set default values for props is to create a special object, which is created as an attribute of our component called `defaultProps` (for example `ExampleComponent.defaultProps`).

```javascript
Comment.defaultProps = {
    author: 'Mike Mikerson',
    text: 'Text placeholder'
}
```


If a component is passed as a prop, it's referenced by using `{props.children}`.

```javascript
// Inside parent component
<CommentSection
    <Comment
        author="Mike" 
        text="Thanks for the info!" 
        postedAt={commentInfo.date} 
    />
/>

// Inside child component
const CommentSection = props => {
    // Some code...
    <div>{props.children}</div>   // This is where the child component will be inserted
    // Some code...
}
```


___

## Class Components
Props inside **class components** are used as part of `this`, so `this.props` instead of just `props`.

With the help of the **hooks system**, **function components** can have the same functionality as the **class components**, using hooks in place of the **Lifecycle Method system** to run code at specific points in time and using hooks to access the **state system** and update content on the screen. Functional components are good for simple content, while class components are good for just about everything else.

Some of the benefits of class components are:
1) Easier code organization
2) Usage of the **state** system, which makes it easier to handle user input
3) Usage of the **Lifecycle method** system, which enables rerendering when conditions are fulfilled

Class components follow three rules:
1) They must be a JS class
2) They must define a `render` method that returns some JSX
3) They must extend (subclass) `React.Component` (borrow useful methods other than `render`)

The **state** system is used to manage data inside of the application, data which is prone to change over time. In general, the **state** system is used when we want **React** to update the content on the screen (for example when text is typed in or when a different option is selected). `state` is a JS object that contains data relevant to a component. Updating it causes the component to rerender. It **must** be initialized when a component is created and it should **only** be updated using the function `setState`.

```javascript
this.state = { text: "Example text" }

// Don't do this, it's not how 'state' variables should be modified
this.state.text = "Something"

// Do this instead, it rerenders the page as intended by 'state'
this.setState({ text: "Something" })
```


The `constructor` method is automatically called whenever a class component is first created, before anything else, and thus a good way to initialize **state** is by doing it inside of the `constructor`. Because writing the `constructor` overloads the one implemented in `React.Component`, a line with `super(props)` must be present in the constructor in order to call the original constructor from `React.Component`, so that it can set the component up for us. If only **state** is initialized in the constructor, the entire constructor should be deleted and replaced with a `state = { exampleMsg: "This is an example!" }` (note that it says `state`, not `this.state` when initialized this way).

```javascript
class Example extends React.Component {
    // One way of initializing state, through a constructor
    constructor {
        super(props);
        this.state = { exampleMsg: "This is an example!" };
    }
    
    // A simpler alternative that can be done instead of the constructor
    state = { exampleMsg: "This is an example!" }
}
```


Variables in `state` which we know are going to be arrays or objects should be initialized as empty arrays or objects, so that we don't get error messages when we call methods specific to those data types (such as `length`).

A **component lifecycle method** is a function that can optionally be defined inside of class-based components. If they're implemented, they'll be called automatically during certain points of a component's lifecycle. Some of these (in order of when they're triggered) are:
1) `constructor` - Always called first. Good place to do one-time setup. Shouldn't be used for data-loading according to the best practices, in order to have clean code.
2) `render` - Called after the constructor, unique in that it's not optional. Should avoid doing anything besides returning JSX here (such as making network requests).
3) `componentDidMount` - Called immediately after the component first gets rendered on the screen. Good place to do initial data-loading for the component, or start an outside process (like getting the user's geolocation).
4) `componentDidUpdate` - Called when a component updates itself, like through `setState`. `render` gets called before it every time. Good place to do more data-loading when there's a change to the **state** or **props**.
5) `componentWillUnmount` - Called when the component stops being displayed on the screen, useful for cleanup (especially for non-React stuff).

Three more lifecycle methods exist (`shouldComponentUpdate`, `getDerivedStateFromProps` and `getSnapshotBeforeUpdate`), but they're very rarely used.

___

When methods are passed to JSX element properties, parentheses aren't added at the end of the method, otherwise the method is called whenever the component is rendered. By leaving off the parentheses, the reference to the method is passed instead, so that it can be called at the appropriate time. These callback methods should follow a naming convention starting with `on` (or `handle`), followed by the name of the element it's assigned to (`input` in the above example), followed by when it's called (on the element being changed in the above example - `change`), for example `onInputChange` for something that triggers when a text input is changed. This is done so that the callback function names clearly convey their purpose. An alternative to single-line callback functions is to write them inline as an arrow function (like in the example above).

When the callback method is invoked, one argument will be passed to it automatically. This argument object is usually referred to as the `event` object, and it's a JS object which contains information about the event which just occurred. It can be accessed by using `event.target.value` to get the text from an input field, for example. Here are some special property names used to wire up callback methods and event handlers:
1) `onClick` - Triggers when the user clicks on something, supported by just about any JSX element
2) `onChange` - Triggers when the user changes text in an input
3) `onSubmit` - Triggers when the user submits a form

```javascript
onInputChange(event) {
    this.setState({ msg: event.target.value });
}

render() {
    return (
        <form className="ui form">
            <div className="field">
                <label>Search</label>
                //                           No parentheses    v
                <input type="text" onChange={this.onInputChange} />
                
                //          Alternate inline way without callback method
                //                            \/   Event object
                <input type="text" onChange={(e) => this.setState({ 
                    msg: e.target.value
                })} />
            </div>
        </form>
    );
}
```


Another thing to note is that it's better to use **controlled components** than **uncontrolled components**. This is because controlled components always contain the values they're working with in **React**, and they don't have to look inside the HTML DOM to find them. All the data we're working with should be centralized inside **React** components. Another useful thing this allows us to do is setting the default values of elements (such as through **state**).

Callbacks can be used to transfer data from a child component to a parent component. This can be done by passing a method from a parent component to a child component, who then calls it on certain events.

```javascript
// Uncontrolled component
<input 
    type="text"
    onChange={
        (e) => this.setState({ msg: e.target.value }) 
    } 
/>

// Controlled component
<input 
    type="text"
    // The React controls and stores the data, not the DOM
    value={this.state.msg}
    onChange={
        (e) => this.setState({ msg: e.target.value }) 
    } 
/>
```


___
### Asynchronous requests
Requests can be made using the `axios` library, with the `axios.get` function for **GET** requests. The function returns a `Promise` object, which has a function called `then` we can use to run some code after the response is received.

```javascript
// The '.then' way
onSearchSubmit(searchTerm) {
    axios.get('https://api.unsplash.com/search/photos', {
        params: { query: searchTerm },
        headers: {
            Authorization: API_KEY
        }
    }).then((res) => {
        console.log(res.data.results)
    });
}
```


Another way to do this is by marking the request function as asynchronous, using the `async` keyword in front of the function name, then creating a variable which will hold the response, and assigning the `await axios.get()` statement to it.

```javascript
// The 'await' way
async onSearchSubmit(searchTerm) {
    const res = await axios.get('https://api.unsplash.com/search/photos', {
        params: { query: searchTerm },
        headers: {
            Authorization: API_KEY
        }
    });
    
    console.log(res.data.results)
    }
```


Arrow functions which are also `async` need to have the `async` keyword after the `=` sign, and before the parentheses which hold the parameters. <br>
For example `async myFun (params) {` becomes `const myFun = async (params) => {`.

___

## Hooks
In **React**, the **Hooks system** is all about giving functional elements a lot of additional functionality. Hooks are all about providing tools to write reusable code, instead of more classic techniques like *inheritance*. The **hooks system** provides some inbuilt functions, such as:
1) `useState` - Allows the use of `state` in a functional component
2) `useEffect` - Allows the use of something similar to lifecycle methods
3) `useRef` - Allows the creation of a `ref` in a function component
4) `useContext` - 
5) `useReducer` - 
6) `useCallback` - 
7) `useMemo` - 
8) `useImperativeHandle` - 
9) `useLayoutEffect` - 
10) `useDebugValue` - 
These inbuilt hooks can also be used to write custom hooks - pieces of code which do one very repeatable task.

`state` inside functional elements can be set up by using the `useState` hook. As can be seen from the example, each `state` variable in functional components created using `useState` needs to be declared (using its own `useState`) or changed (using its own setter function) seperately, unlike class components where we can initialize or change multiple `state` variables in a single line (or with a single `this.setState` call).

```javascript
// State variable |                     Default value  |
//               \/                                   \/
const[exampleVariable, setExampleVariable] = useState(null);
//                                    /\
// Function to change this variable   |

// This the class component equivalent
state = { exampleVariable: null };
setExampleVariable = (value) => { this.setState({ exampleVariable: value })
```


`useEffect` allows function components to use something similar to lifecycle methods. The first argument passed to it is the function which needs to be triggered. The second argument is the **dependency array**, which defines in which of the three scenarios will the function be called:
1) When the component is rendered for the first time
2) When it first renders and whenever it rerenders
3) When it first renders and (whenever it rerenders AND some piece of data changes)

```javascript
// Second argument is an empty array
// Trigger on first render
useEffect(() => {
    console.log('Event trigger!');
}, []);

// No second argument
// Trigger on first render and every rerender
useEffect(() => {
    console.log('Event trigger!');
});

// Second argument is an array with a variable
// Trigger on first render
// Trigger on every rerender IF data has changed since last render
useEffect(() => {
    console.log('Event trigger!');
}, [someVariable]);
```


**React** will show a warning whenever a prop or state variable that isn't referenced in the `useEffect` dependency array is referenced inside `useEffect`.

```javascript
// Doesn't work, gives an error
useEffect(async () => {
    await axios.get(SOMETHING);
});

// 1) Make a helper function, then call it
useEffect(() => {
    const search = async () => {
        await axios.get(SOMETHING);
    };
    
    search();
});

// Compact way of the example above
// 2) Define a function and immediately invoke it
useEffect(() => {
    (async () => {
        await axios.get(SOMETHING);
    })();
});

// 3) Make use of Promises
useEffect(async () => {
    axios.get(SOMETHING)
        .then((res) => {
            console.log(res.data);
        });
});
```


**Functions passed to `useEffect` can't be `async`.** There are three solutions to this:
1) Creating a helper function inside of `useEffect` which is `async`, then call it inside `useEffect`
2) Define a function without a name inside of `useEffect` and immediately invoke it
3) Using promises with `axios.get().then()`

```javascript
useEffect(() => {
    console.log("Main Call!");
    
    return () => console.log ("Cleanup Call!");
});

// The console logs will trigger as follows
// First render -----  Main Call!
// Second render ----- Cleanup Call!
// Second render ----- Main Call!
```


When `useEffect` is created and a function is passed as the first argument, the only thing we're allowed to return from it is another function (called the **cleanup function**). The returned cleanup function is then called BEFORE the main function passed to `useEffect` every time after the first render. The cleanup function will also be invoked once the component stops being shown, which can be useful in cases where we want to do cleanup for the entire component.

```javascript
useEffect(() => {
    // onBodyClick contains a ref 'myRef' to a component element
    // myRef will become undefined when the component isn't rendered
    const onBodyClick = (event) => {
        if (myRef.current.contains(event.target)) return;
        setOpen(false);
    }

    document.body.addEventListener("click", onBodyClick, { capture: true } );
    
    // Remove the 'onBodyClick' event listener from the 'click' event
    // This stops the undefined myRef from crashing the app when component is unloaded
    return () => {
        document.body.removeEventListener('click', onBodyClick);
    };
}, []);
```


___

Events in **React** **bubble up**, which is to say that they will propagate from the component where they're created through its parent components, and eventually into the HTML body. Manual event listeners can thus be set up to watch for events which bubble up. However, all event listeners created using `addEventListener` get called first, and the **React** event listeners get called AFTER them. The order inside those two categories is still from the most child element towards the most parent element. It's possible to cancel event bubbling, but it's considered bad practice as it can break other parts of the code.
  
Manual event listeners added with `addEventListener` are usually only called one time, so an empty array is usually passed as the second argument to the `useEffect` function. The callback is also usually defined as a separate helper function inside `useEffect`, so that it can easily be cleaned up with the `removeEventListener`.

```javascript
    // Example component
    const App = () => {
        return ( // 'App' sees it as the parent component
            <div> // I see it as the parent element
                <ExampleComponent> // I see it as the parent component
                    <ExampleElement /> // I create the event
                </ExampleDisplay>
            </div>
        );
    }
    
    // Event listener which will pick up everything
    // (every event will bubble up to the HTML body)
    document.body.addEventListener('click', () => console.log('Click!'));
```


Custom hooks are the best way to create reusable code in a **React** project (besides components). They're created by extracting hook-related code out of a function component, and they always make use of at least one primitive (inbuilt) hook internally. Each custom hook should have **one** purpose, and they're great for tasks like data-fetching. The process of creating reusable hooks is hard to define, but could go something like this:
1) Identify each line of code in a component related to a single purpose
2) Identify the inputs to that code
3) Identify the outputs of that code
4) Extract the code into a separate function, receiving the inputs as arguments and returning the outputs

### Refs
**React** Refs are a system which gives us direct access to a single DOM element that is rendered by a component, and they're used in place of the standard JS `document.querySelector(some_tag)`. Since all JSX elements - even the ones which look like standard HTML elements such as `<img>` - aren't HTML, but are eventually converted to real HTML, we thus have no way to reference them aside from using refs. They're created in the constructor, assigned to instance variables, then passed to a particular JSX element as props. They can be assigned to the `state` of a component, but since refs don't change over time, it shouldn't be done.

```javascript
constructor(props) {
    super(props);
    // Crete ref and assign it to a component instance variable
    this.imageRef = React.createRef();
}

componentDidMount() {
    // Use the ref
    console.log(this.imageRef);
}

render() {
    return(
        // Pass the ref to JSX element as a prop
        <img ref={this.imageRef} src={this.props.image.urls.regular} />
    );
}
```


However, refs on things which are loaded from elsewhere (such as the images in the previous example) won't work properly, since they'll be called on objects whose content arrives after a certain delay. A way to properly use these refs is by adding an event listener which waits for them to load properly.

```javascript
// Won't work, image isn't loaded from the remote source yet
componentDidMount() {
    console.log(this.imageRef.current.clientHeight);
}

// Works - It waits for the image to load before calling the function
componentDidMount() {
    this.imageRef.current.addEventListener('load', this.setSpans);
}

setSpans = () => {
    const spans = Math.ceil(this.imageRef.current.clientHeight / 10);
    this.setState({ spans });
}
```


Refs can be created with hooks using the `useRef` hook function. The `.contains` method is shared for all DOM elements, and allows us to see if an element is contained within another.

```javascript
const exampleRef = useRef();

useEffect(() => {
    document.body.addEventListener("click", (event) => {
        // Stop if event originated from this component
        // event.target returns the element from which the event originates
        if (exampleRef.current.contains(event.target)) return;
        // Some code if the element isn't contained in the ref element
    }, { capture: true } );
}, []);

return (
    <div ref={exampleRef} className="ui form">
        <div className="field">
            // More elements
        </div>
    </div>
);
```


___

## Tips
If a component has conditional statements (using the ternary operator) which are repeated multiple times, it's good to replace them with a configuration structure. The example above shows how, by using the structure, it was possible to avoid multiple ternary statements which had the same condition but returned different things.

```javascript 
const seasonConfig = {
    summer: {
        text: 'Blazing hot!',
        iconName: 'sun'
    },
    winter: {
        text: 'Frosty!',
        iconName: 'snowflake'
    }
};

const getSeason = (lat, month) => {
    if (month>2 && month<9){
        return lat > 0 ? 'summer' : 'winter';
    } else {
        return lat > 0 ? 'winter' : 'summer';
    }
}

const SeasonDisplay = props => {
    const season = getSeason(props.lat, new Date().getMonth());
    
    // Single-line solution made possible by the configuration structure
    const { text, iconName } = seasonConfig[season];
    
    // Bad approach with multiple ternary statements
    const text = season === 'winter' ? 'Frosty!' : 'Blazing hot!'
    const iconName = season === 'winter' ? 'snowflake' : 'sun'

    return (
        <div>
            <i className={`${iconName} icon`} />    // Variable interpolation
            <h1>{text}</h1>
        </div>
    );
}
```


Conditional statements in the `render` method should be avoided because they make it harder to add or change JSX elements. A better way to go about that is by creating a helper method which houses the conditional statements, then calling it inside the `render` method, thus allowing the returned content from that helper method to be easily interacted with by new elements.

```javascript
renderContent() {
    // Would need to add 3 <div> elements here to cover all the cases
    if (...) {  // Condition 1
        // Return 1
    }
    
    if (...) {  // Condition 2
        // Return 2
    }
    
    // Return 3
}

render() {
    return (
        // Only need to add one <div> here to cover all the cases
        <div className="border red">
            {this.renderContent()}
        </div>
    );
}
```


___

### Prevent refresh on form submit
Forms refresh the page by default when they're submitted. This can be prevented by using `event.preventDefault()`.

```javascript
onFormSubmit(event) {
    // Prevent default form behavior, which refreshes the page
    event.preventDefault();
}

render() {
    <form onSubmit={this.onFormSubmit}>
        // Some code...
    </form>
}
```


___

### The `this` keyword
`this` in JS references a concrete instance of a class it's called from. When the callback in the above example is made, the `this` inside of the callback function becomes `undefined`, because the callback function implementation is taken from the instance and used on its own, thus losing the link needed to use `this`. One of the ways to fix this problem is by binding the callback function, then overwriting the current version of the function with the one that always has `this` bound to it. Another way to fix the problem is by turning the function into an arrow function, because arrow functions automatically bind the value of `this` for all the code inside the function. This makes it a good idea to always write callback functions as arrow functions, just to be safe.

```javascript
exampleMethod(event) {
    // This causes an error if one of the fixes isn't applied!
    console.log(this.state);
}

// Fix #1: Bind 'this' to the function
constructor() {
    this.exampleMethod = this.exampleMethod.bind(this);
}

// Fix #2: Convert the function into an arrow function
exampleMethod = (event) => {
    // This causes an error if one of the fixes isn't applied!
    console.log(this.state);
}

render() {
    <form onSubmit={this.exampleMethod}>
        // Some code...
    </form>
    
    // Fix #3: Wrap the function call with an arrow function
    // The function is invoked, not referenced
    // Note the presence of the parentheses v
    <form onSubmit={() => this.exampleMethod()}>
        // Some code...
    </form>
}
```


___

### Map
The `.map` method works on arrays and is passed a function which is applied to each element of an array, then used to generate a new array without mutating the original. The first argument that `map` returns is the currently selected element from the collection, while the second (optional) argument is the index of the element.

```javascript
// Let's say we want to take the 'numbers' array, multiplied by 10
// We also want that to be a new array, without changing the original
const numbers = [0, 1, 2, 3, 4]

// numbers.map(function(num) {}) also works
numbers.map((num) => {
    return num * 10;
});

// Compact version, looks even nicer!
numbers.map(num => num * 10);

// Using index as the second argument
numbers.map((num, index) => {
    return num * 10 * index;
});
```


If rendering items **from a list** (such as when using `map`), each element should have a `key` prop in order to allow **React** to see which elements are already in the DOM, so that they don't have to be rerendered if we're only putting in additional elements. Note that the `key` property should be given to the root tag that's being returned. A `key` should be a value which is consistent and unchanging between rerenders (such as the `id` property often coupled with data).

```javascript
    // Works but gives off a warning that the 'key' prop is missing
    const images = props.images.map((image) => {
        return <img src={image.urls.regular} />
    });
    
    // No warning, has the 'key' prop
    const images = props.images.map((image) => {
        return <img key={image.id} src={image.urls.regular} />
    });
    
    // Destructured and easy on the eyes, doesn't repeat 'image.' several times
    const images = props.images.map(({ id, urls }) => {
        return <img key={id} src={urls.regular} />
    });
```


___

### `React.Fragment` as a placeholder enveloping element 
Sometimes we want to render multiple things which are already enveloped by a `<div>` inside the parent component, and we might not want to have the extra `<div>` enveloping the elements rendered inside the child element as well (like when borders are applied to `<div>` elements, to avoid double borders being drawn).

`React.Fragment` can be used in place of a `<div>` in this scenario. Note that the `key` special prop can also be applied to these placeholders, in order to avoid warnings related to missing keys later on.

```
<React.Fragment key={someKey}>
    // Other JSX elements
</React.Fragment>
```


___

### Timers with `setTimeout`
The `setTimeout` function accepts two arguments. The first argument is the function which is called once the timer expires, and the second argument is the duration of time. `setTimeout` also returns an identifier, which can be used by the `clearTimeout` function to prevent the timeout function from activating.

```javascript
// Print "Hello!" after 10 seconds
setTimeout(() => console.log("Hello!"), 10000)
```


___

### Array shortcuts [ES2015]
The ES2015 syntax for taking all elements from an array is to use `...` before the array name.
`[...oldArray, newElement]` would return a new array containing all the elements from `oldArray`, as well as the new element `newElement`.

Elements can be filtered out of an array by using the `array.filter(filterFunction)` method. It takes a function which returns `true` if an element should be kept in the resulting array, or `false` if it should be discarded. The original array isn't modified, but a new one is returned instead.

```javascript
// Remove a name from an array and return the new array
const removeName = (someArray, someName) => {
    return someArray.filter(name => name !== someName);
}
```


___

## Redux
**Redux** is a `state` management library. With **Redux**, rather than maintaining state inside a component, it's extracted into the Redux library. It makes creating complex applications easier, and it's not explicitly designed to work with **React**, and is thus used in other libraries, and there are ports of it for other languages as well. 

The **Redux cycle** consists of the following steps, listed in the order in which they are used: `Action Creator`->`Action`->`Dispatch`->`Reducers`->`State`.
* An `Action Creator` is a function that creates or returns a plain JS object - an `Action`.
* An `Action` has a **type** property and a **payload** property. The **type** property describes some change we might want to make to our data, while the **payload** describes the context around the change we want to make.
* The `dispatch` function takes in an `Action`, makes copies of that object and then passes it off to a bunch of different places inside the application.
* A `Reducer` is a function responsible for taking in an `Action` and some existing amount of data. The `Action` is then processed and, depending on the type of the `Action`, the data is changed. The data is then returned so it can be centralized somewhere - in the `State`.
* `State` is a central repository of all information created by the `Reducers`. This way, the app doesn't have to look for the individual `Reducers` to find the data it's looking for.

By convention, when creating an **action** type, all letters are uppercase and underscores are used instead of spaces. Payload properties are likewise passed as properties to the action creator in order to keep it reusable.
    
```javascript
// Action creator
const createPolicy = (name, amount) => {
    // Action that's being created
    return {
        type: 'CREATE_POLICY',
        payload: {
            name: name,
            amount: amount
            // Just 'name, amount' would have also worked with ES2015
        }
    };
};

const deletePolicy = (name) => {
    return {
        type: 'DELETE_POLICY',
        payload: {
            name: name
        }
    };
};

// Note how all the Action creators look nearly identical
// Just about any Action creator will look like this - a type and a payload
const createClaim = (name, amount) => {
    return {
        type: 'CREATE_CLAIM',
        payload: {
            name, amount
        }
    };
};
```


The `dispatch` function is implemented as part of **Redux**, so we don't have to concern ourselves with writing it.

When using `Reducers`, we should **ALWAYS** return a new array/object instead of modifying an old one. Modifying existing data structures should be avoided as much as possible. Edge cases such as the reducers called for the first time, where the existing values or data structures don't exist (and are thus `undefined`), should be handled by assigning the parameters default values.

```javascript
// If the reducer is called for the  |  1st time (claimsList = undefined)
//                                  \/  claimsList array doesn't exist yet
const claimsHistory = (claimsList = [], action) => {
    // Find out if the action concerns the reducer by looking at the type
    if (action.type === 'CREATE_CLAIM') {
        // Return a NEW array, don't touch the old one
        return [...claimsList, action.payload];
    }
    
    // If the action doesn't concern this reducer, return the existing value
    return claimsList;
};

const accounting = (money = 1000, action) => {
    if (action.type === 'CREATE_CLAIM') {
        return money - action.payload.amount;
    } else if (action.type === 'CREATE_POLICY') {
        return money + action.payload.amount;
    }
    
    return money;
}

// Check type, perform action and return a copy, or return existing data
const policies = (policiesList = [], action) => {
    if (action.type === 'CREATE_POLICY') {
        return [...policiesList, action.payload.name];
    } else if (action.type === 'DELETE_POLICY') {
        return policiesList.filter(name => name !== action.payload.name)
    }
    
    return policiesList;
}
```

  
Once the `Action Creators` and `Reducers` have been created, they can then be used to create a `Redux` app.

```javascript
const { createStore, combineReducers } = Redux;

// This holds all the references to the Reducers
// By convention, the keys should be similar to the Reducer function names
// Can be changed to change the names of the state properties
const departments = combineReducers({
    accounting: accounting,
    claimsHistory: claimsHistory,
    policies: policies
})

// Create a centralized storage for Reducer references and their data
const store = createStore(departments);

// Each dispatch runs through the entire Redux cycle
store.dispatch(createPolicy('Joe', 20));
store.dispatch(createPolicy('Jim', 30));
store.dispatch(createPolicy('Jay', 15));

store.dispatch(createClaim('Joe', 300));
store.dispatch(createClaim('Jay', 120));
store.dispatch(createClaim('Joe', 300));

store.dispatch(deletePolicy('Jim'));

console.log(store.getState());
```


The **Redux** `state` can't be modified without dispatching an action (things like `store.state.accounting` don't work).

### Working with React-Redux
The library which lets **React** and **Redux** work together is called `React-Redux`, and **Redux** can be set up for a `create-react-app` project with the following command:

```
npm install --save redux react-redux
```

`React-Redux` provides two components: `Provider` and `Connect`. Instances of them can be created and props can be passed to them to configure how the application will behave. The `Provider` is rendered at the top of the application hierarchy, even above the `<App>` component, and it provides information to other components in the app. After that, every component that needs to access the data inside the **store** should be made to communicate with the `Provider` using the `Connect` component. The communication between `Provider` and `Connect` is done not through the props system, but through the **context system**.

In short: create a `Provider`, pass it a reference to our **Redux store**, then wrap any component that needs to interact with the store with a `Connect`.

The following boilerplate code can be found in just about any **Redux** application.

```javascript
// /src/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import { createStore } from 'redux';

import App from './components/App';
import reducers from './reducers';

ReactDOM.render(
    //                                |   Creating a store with reducers
    // Putting App inside Provider   \/
    <Provider store={createStore(reducers)}>
        <App />
    </Provider>, 
    document.querySelector('#root')
);
```

Reducers are created as usual, then grouped up with the `combineReducers` function and exported.

```javascript
// /src/reducers/index.js
import { combineReducers } from "redux";

const songsReducer = () => {
    // This just returns a hardcoded list of songs, bad use of reducers
    return [
        { title: 'Resurrection Code', duration: '4:37' },
        { title: 'Starfire', duration: '5:01' },
        { title: 'Ready Steady Go', duration: '3:25' }
    ];
};

const selectedSongReducer = (selectedSong=null, action) => {
    if (action.type === 'SONG_SELECTED') {
        return action.payload;
    }

    return selectedSong;
}

export default combineReducers({
    songs: songsReducer,
    selectedSong: selectedSongReducer
});
```


When we want a component to make use of `Connect`, we need to modify its `export` statement. The `connect` function returns a function which takes our component as a parameter, the result of which is exported.

```javascript
import { connect } from 'react-redux';
// Component implementation...
export default connect()(ExampleComponent);
```


By convention, a function called `mapStateToProps` is defined for each component which accesses **Redux** state, and serves to get data from the **Redux** store into the component. As an argument, it takes in all the data inside the **Redux** store, then returns the data from it relevant to that component as an object property. This function is then passed to the `connect` function as an argument. Any action creators we want to use are likewise passed to the `connect` function, but inside an object.

```javascript
import React from 'react';
import { connect } from 'react-redux';
import { selectSong } from '../actions';

class SongList extends React.Component {
    // Component logic
}

const mapStateToProps = (state) => {
    // this.props now returns { songs: state.songs }
    return { songs: state.songs };
}

// Need to add the mapStateToProps function as an argument to 'connect'
//                           \/              \/ Add action creator as well
export default connect(mapStateToProps, { selectSong })(SongList);

```


This is how components are always connected with the **Redux** store - `connect` is imported, it is called and the component is passed as an argument to the function it returns. We always define the `mapStateToProps` function, it always gets an argument of `state`, and we always return an object that's going to show up as the `props` of our component.

When we pass the action creators in the object passed as an argument to the `connect` function, it does a special operation on all the functions inside that object and puts them in a new JS function, which we call with the current component as the argument (the second set of parentheses with the component name inside them). The `dispatch` function is automatically called on all the actions which are returned.

___

A **middleware** is a plain JS function which is called with every action that's dispatched. It has the ability to **STOP**, **MODIFY** or otherwise change actions. A lot of open source middleware exists, and their most common use is for dealing with `async` actions.

`redux-thunk` is middleware which helps us make requests in a **Redux** application. Actions must be plain objects, and can't use standard `async`-`await` keywords which, when converted to ES15 with Babel, turn them into a `Promise`. If `async`-`await` aren't used, then it takes too long for the data to get to the app, and we don't have any fetched data by the time our action gets to a reducer. Action creators which make requests to external sources are called **asynchronous action creators**. They return data after a certain amount of time and they require middleware (like `redux-thunk`) to work.

General data loading with **Redux** goes as follows:
* A component is rendered onto the screen
* Component's `componentDidMount` lifecycle method gets called
* An action creator is called from `componentDidMount`
* Action creator runs code to make an API request
* API responds with data
* Action creator returns an action with the fetched data in the `payload` property
* One of the reducers sees the action, returns the data from the `payload`
* Because a new *state* object is generated, **Redux** and **React-Redux** cause the **React** app to be rerendered

Components are generally responsible for fetching data they need by calling an action creator, as seen in the first three steps. Then, action creators take on the responsibility for making API requests, as seen in the following three steps (this is where **Redux-Thunk** comes into play). Lastly, the data is fetched into the component by generating a new state in the **Redux** store, then getting it in the component through `mapStateToProps`.

When adding `redux-thunk` to the application, the boilerplate in `/src/index.js` changes in the line where the `createStore` function is called:

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import { applyMiddleware, createStore } from 'redux';
import thunk from 'redux-thunk';

import App from './components/App';
import reducers from './reducers';

const store = createStore(reducers, applyMiddleware(thunk));

ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>,
    document.querySelector('#root')
);
```

Usually, action creators **must** return action objects, while with `redux-thunk` they can return action objects OR functions. That's the only change that `redux-thunk` makes to the application. If an action object is returned, it still must have a `type`, and it can optionally have a `payload`. If a function is returned, it gets called with the `dispatch` and `getState` functions as arguments, which lets the function change and read/access any data it wants. After the function has been invoked with `dispatch`, we wait for the request to finish and then **manually** dispatch the action.

When returning functions from an action creator, nothing should be returned from that inner function. Instead, `dispatch` should be called with the action we would otherwise return.

```javascript
export const fetchPosts = () => {
    return async (dispatch, getState) => {
        const res = await jsonPlaceholder.get('/posts');
        dispatch({ type: 'FETCH_POSTS', payload: res });
    }
}

// Shortened version
export const fetchPosts = () => async dispatch => {
    const res = await jsonPlaceholder.get('/posts');
    dispatch({ type: 'FETCH_POSTS', payload: res });
}
```

