# React.js
## Basics
Working with React means working with two separate libraries:
1) **React** - Has code which knows how to get components to work together (how to call a component function, take the JSX it returns and iterate over all the elements and decide to create an HTML element or call another component function). It's called a *reconciler*.
2) **ReactDOM** - Knows how to take JSX, turns it into HTML, and then puts the HTML into the DOM, which is then displayed in the browser. It's called a *renderer*.

When a link is entered into a web browser, then the browser sends a request to the server hosting the website for an `index.html` file. After `index.html` is received in the response, the browser parses the file, one of the `<script>` tags inside it tells the browser to make another request for a special JS file called `bundle.js`, which combines all the different JS files which make up the web application.

**Babel** is a library used to convert newer versions of JS (ES 2015, 2016 and onward) - which might not be supported by some browsers - into a version of JS which is supported by most browsers (ES5).

Using `import` instead of `require` to load in JS modules only specifies what kind of method is used to share the code between different JS files. ES2015 modules use `import`, whereas CommonJS modules use `require`.

___
## Components
A *React component* is a JS **function** or **class** that *returns some JSX* (which turns into HTML that is shown to the user) and *handles feedback from the user* (using event handlers). **JSX** is basically a set of instructions which tells React what to display on the screen. Before JSX is rendered in the browser, it first needs to be converted into regular JS code. Each tag in JSX is examined by checking if it's a *DOM* element (for example, a `<div>`). If so, the element is rendered. If it's a component, then the component function is called and all the JSX it returns is also inspected.

Three tenets of React:
1) **Component Nesting** - A component can be shown inside of another component
2) **Component Reusability** - Components should be easily reused throughout the application
3) **Component Configuration** - Components should be such so that we're able to configure them when they're created.

Creating a reusable, configurable component is done in a few steps:
1) Identify the JSX that is duplicated and that could be reused
2) Identify the purpose of that block of JSX and name the new component appropriately (eg. `Comment`)
3) Create a file with the same name as the chosen component name (`Comment.js`)
4) Place the selected JSX into the file and make it configurable by using the **props** system
5) The finished component should then have an `export default COMPONENTNAME`, in this case it would be `export default Comment`, after which it should be imported elsewhere with `import Comment from 'COMPONENT_PATH'` (with the appropriate component path)

### Props
When nesting components, the component which holds other components inside it is called the **parent component**, and the components inside it are its **children** (or **child components**). The **props** system (short for **properties**) is then used to pass data from a parent component to a child component, where the goal is to customize or configure the child component. It's all about having a parent customize how a child looks or behaves.

#
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
Information is passed from a parent to a child through props in a similar way to how HTML elements are given attributes. Default values for props can be set by using the OR operator after a prop, followed by the default value (like `<div>{props.text || 'Text placeholder'}</div>`).

#
    Comment.defaultProps = {
        author: 'Mike Mikerson',
        text: 'Text placeholder'
    }
A better way to set default values for props is to create a special object, which is created as an attribute of our component called `defaultProps` (for example `ExampleComponent.defaultProps`).

#
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
If a component is passed as a prop, it's referenced by using `{props.children}`.

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

The **state** system is used to manage data inside of the application, data which is prone to change over time. In general, the **state** system is used when we want React to update the content on the screen (for example when text is typed in or when a different option is selected). `state` is a JS object that contains data relevant to a component. Updating it causes the component to rerender. It **must** be initialized when a component is created and it should **only** be updated using the function `setState`. 

#   
    class Example extends React.Component {
        // One way of initializing state, through a constructor
        constructor {
            super(props);
            this.state = { exampleMsg: "This is an example!" };
        }
        
        // A simpler alternative that can be done instead of the constructor
        state = { exampleMsg: "This is an example!" }
    }
The `constructor` method is automatically called whenever a class component is first created, before anything else, and thus a good way to initialize **state** is by doing it inside of the `constructor`. Because writing the `constructor` overloads the one implemented in `React.Component`, a line with `super(props)` must be present in the constructor in order to call the original constructor function from `React.Component`, so that it can set the component up for us. If only **state** is initialized in the constructor, the entire constructor should be deleted and replaced with a `state = { exampleMsg: "This is an example!" }` (note that it says `state`, not `this.state` when initialized this way).

A **component lifecycle method** is a function that can optionally be defined inside of class-based components. If they're implemented, they'll be called automatically during certain points of a component's lifecycle. Some of these (in order of when they're triggered) are:
1) `constructor` - Always called first. Good place to do one-time setup. Shouldn't be used for data-loading according to the best practices, in order to have clean code.
2) `render` - Called after the constructor, unique in that it's not optional. Should avoid doing anything besides returning JSX here (such as making network requests).
3) `componentDidMount` - Called immediately after the component first gets rendered on the screen. Good place to do initial data-loading for the component, or start an outside process (like getting the user's geolocation).
4) `componentDidUpdate` - Called when a component updates itself, like through `setState`. `render` gets called before it every time. Good place to do more data-loading when there's a change to the **state** or **props**.
5) `componentWillUnmount` - Called when the component stops being displayed on the screen, useful for cleanup (especially for non-React stuff).

Three more lifecycle methods exist (`shouldComponentUpdate`, `getDerivedStateFromProps` and `getSnapshotBeforeUpdate`), but they're very rarely used.

#   
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
                    
                    //                Alternate way without callback method
                    <input type="text" onChange={(e) => this.setState({ 
                        msg: e.target.value  
                    })} />
                </div>
            </form>
        );
    }
When methods are passed to JSX element properties, parentheses aren't added at the end of the method, otherwise the method is called whenever the component is rendered. By leaving off the parentheses, the reference to the method is passed instead, so that it can be called at the appropriate time. These callback methods should follow a naming convention starting with `on` (or `handle`), followed by the name of the element it's assigned to (`input` in the above example), followed by when it's called (on the element being changed in the above example - `change`), for example `onInputChange` for something that triggers when a text input is changed. This is done so that the callback function names clearly convey their purpose. An alternative to single-line callback functions is to write them inline as an arrow function (like in the example above).

When the callback method is invoked, one argument will be passed to it automatically. This argument object is usually referred to as the `event` object, and it's a JS object which contains information about the event which just occurred. It can be accessed by using `event.target.value` to get the text from an input field, for example. Here are some special property names used to wire up callback methods and event handlers:
1) `onClick` - Triggers when the user clicks on something, supported by just about any JSX element
2) `onChange` - Triggers when the user changes text in an input
3) `onSubmit` - Triggers when the user submits a form

#
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

Another thing to note is that it's better to use **controlled components** than **uncontrolled components**. This is because controlled components always contain the values they're working with in React, and they don't have to look inside the HTML DOM to find them. All the data we're working with should be centralized inside React components. Another useful thing this allows us to do is setting the default values of elements (such as through **state**).

Callbacks can be used to transfer data from a child component to a parent component. This can be done by passing a method from a parent component to a child component, who then calls it on certain events.

### Tips
#   
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
        
        // Ugly approach with multiple ternary statements
        const text = season === 'winter' ? 'Frosty!' : 'Blazing hot!'
        const iconName = season === 'winter' ? 'snowflake' : 'sun'

        return (
            <div>
                <i className={`${iconName} icon`} />    // Variable interpolation
                <h1>{text}</h1>
            </div>
        );
    }
If a component has ternary statements which are repeated multiple times, it's good to replace them with a configuration structure. The example above shows how, by using the structure, it was possible to avoid multiple ternary statements which had the same condition but returned different things.

#
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

Conditional statements in the `render` method should be avoided because they make it harder to add or change JSX elements. A better way to go about that is by creating a helper method which houses the conditional statements, then calling it inside the `render` method, thus allowing the returned content from that helper method to be easily interacted with by new elements.

#   
    onFormSubmit(event) {
        // Prevent default form behavior, which refreshes the page
        event.preventDefault();
    }
    
    render() {
        <form onSubmit={this.onFormSubmit}>
            // Some code...
        </form>
    }
Forms refresh the page by default when they're submitted. This can be prevented by using `event.preventDefault()`.

#   
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
`this` in JS references a concrete instance of a class it's called from. When the callback in the above example is made, the `this` inside of the callback function becomes `undefined`, because the callback function implementation is taken from the instance and used on its own, thus losing the link needed to use `this`. One of the ways to fix this problem is by binding the callback function, then overwriting the current version of the function with the one that always has `this` bound to it. Another way to fix the problem is by turning the function into an arrow function, because arrow functions automatically bind the value of `this` for all the code inside the function.

Variables in `state` which we know are going to be arrays or objects should be initialized as empty arrays or objects, so that we don't get error messages when we call methods specific to those data types (such as `length`).

Arrow functions which are also `async` need to have the `async` keyword after the `=` sign, and before the parentheses which hold the parameters. <br>
For example `async myFun (params) {` becomes `myFun = async (params) => {`.
___

#
    ReactDOM.render(<App />, document.getElementById("root"));
The `ReactDOM.render()` function accepts two arguments:
1) The React component which is to be rendered (`<App />` in the example above)
2) The location inside the `index.html` file where the HTML from the component we selected will be placed into (`document.getElementById("root")` in the example above)

___
### JSX
JSX syntax differs from HTML in a few ways.
#
    <div style="border: 1px solid red;"></div>          // HTML
    <div style={{ border: '1px solid red' }}></div>     // JSX
	<button style="background-color: blue; color: white;">Submit</button>		// HTML
	<button style={{ backgroundColor: 'blue'; color: 'white' }}>Submit</button>	// JSX
Properties inside tags are done differently, where JS variables in curly brackets are used in place of multiple entries inside double quotation marks. The first set of curly brackets indicates that a JS variable will be referenced, while the second set (inside the first) denotes a JS *object*. Note that compound attribute names like `'background-color'` are changed to camelcase (`'backgroundColor'`).

#
	<label for="inputEmail">Email: </label>
The convention is to use double quotes for JSX properties and single quotes everywhere else.

___

#
	<label class="fancyLabel" for ='inputEmail'>Sample text</label>				// HTML
	<label className="fancyLabel" htmlFor ='inputEmail'>Sample text</label>		// JSX
When assigning a class to a tag for styling later on, `className` is used instead of `class`. The same applies to the `for` keyword used in the <label> tag, which is converted to `htmlFor`. Although they often result in code which generally works fine, JS keywords should be avoided and their substitutes should be used instead.

___

#
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
JSX can show a lot of things like variables and things returned from functions, but it can't show objects where text is expected. However, it **can** show object properties, so `txt.msg` would work just fine whereas `txt` on its own would cause an error.

### Asynchronous requests
#
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
Requests can be made using the `axios` library, with the `axios.get` function for **GET** requests. The function returns a `Promise` object, which has a function called `then` we can use to run some code after the response is received.

#
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
Another way to do this is by marking the request function as asynchronous, using the `async` keyword in front of the function name, then creating a variable which will hold the response, and assigning the `await axios.get()` statement to it.





