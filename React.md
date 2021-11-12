# React.js
## Basics
Working with React means working with two separate libraries:
1) **React** - Has code which knows how to get components to work together (how to call a component function, take the JSX it returns and iterate over all the elements and decide to create an HTML element or call another component function). It's called a *reconciler*.
2) **ReactDOM** - Knows how to take JSX, turns it into HTML, and then puts the HTML into the DOM, which is then displayed in the browser. It's called a *renderer*.

When a link is entered into a web browser, then the browser sends a request to the server hosting the website for an `index.html` file. After `index.html` is received in the response, the browser parses the file, one of the `<script>` tags inside it tells the browser to make another request for a special JS file called `bundle.js`, which combines all the different JS files which make up the web application.

**Babel** is a library used to convert newer versions of JS (ES 2015, 2016 and onward) - which might not be supported by some browsers - into a version of JS which is supported by most browsers (ES5).

Using `import` instead of `require` to load in JS modules only specifies what kind of method is used to share the code between different JS files. ES2015 modules use `import`, whereas CommonJS modules use `require`.

The React way of returning multiple items from a function is to return them as an array, whereas the common JS way is to return them inside an object. Either way works.

A VM isn't needed for the deployment of a React application because it isn't executing any code on any server, but hosting plain static files (unlike the *Node JS API*, for example). This means it costs dramatically less to deploy a React application compared to applications which execute code, and thus need VMs.

To deploy with Vercel, first install the Vercel CLI using `npm install -g vercel`. After that, `cd` into the project directory, then do `vercel login`. Next, type `vercel` into the command line, and just press Enter to accept the default values since they work just fine for `create-react-app` projects. The app will then be deployed online. After making changes to the app, it can be redeployed with the `vercel --prod` command.

To deploy with Netlity, all work is done on their website. You must create a GitHub repo for the project, push the project onto the repo, then link your GitHub profile with Netlify. After that select **New site from Git**, select the repository you want to use, and you're done. The deployment URL is random initially, but can be changed in the settings afterwards. Netlify syncs with the GitHub repository, so the project is automatically deployed again after every push. Vercel can also be set up to track a GitHub repo.

___
## JSX
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

#
	<label class="fancyLabel" for ='inputEmail'>Sample text</label>				// HTML
	<label className="fancyLabel" htmlFor ='inputEmail'>Sample text</label>		// JSX
When assigning a class to a tag for styling later on, `className` is used instead of `class`. The same applies to the `for` keyword used in the <label> tag, which is converted to `htmlFor`. Although they often result in code which generally works fine, JS keywords should be avoided and their substitutes should be used instead.

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

#
    ReactDOM.render(<App />, document.getElementById("root"));
The `ReactDOM.render()` function accepts two arguments:
1) The React component which is to be rendered (`<App />` in the example above)
2) The location inside the `index.html` file where the HTML from the component we selected will be placed into (`document.getElementById("root")` in the example above)


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



### Class Components
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


___
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

## Hooks
In React, the **Hooks system** is all about giving functional elements a lot of additional functionality. Hooks are all about providing tools to write reusable code, instead of more classic techniques like *inheritance*. The **hooks system** provides some inbuilt functions, such as:
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
#   
    // State variable |                     Default value  |
    //               \/                                   \/
    const[exampleVariable, setExampleVariable] = useState(null);
    //                                    /\
    // Function to change this variable   |
    
    // This the class component equivalent
    state = { exampleVariable: null };
    setExampleVariable = (value) => { this.setState({ exampleVariable: value })
    
`state` inside functional elements can be set up by using the `useState` hook. As can be seen from the example, each `state` variable in functional components created using `useState` needs to be declared (using its own `useState`) or changed (using its own setter function) seperately, unlike class components where we can initialize or change multiple `state` variables in a single line (or with a single `this.setState` call).

#   
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
`useEffect` allows function components to use something similar to lifecycle methods. The first argument passed to it is the function which needs to be triggered. The second argument is the **dependency array**, which defines in which of the three scenarios will the function be called:
1) When the component is rendered for the first time
2) When it first renders and whenever it rerenders
3) When it first renders and (whenever it rerenders AND some piece of data changes)

React will show a warning whenever a prop or state variable that isn't referenced in the `useEffect` dependency array is referenced inside `useEffect`.

   
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
**Functions passed to `useEffect` can't be `async`.** There are three solutions to this:
1) Creating a helper function inside of `useEffect` which is `async`, then call it inside `useEffect`
2) Define a function without a name inside of `useEffect` and immediately invoke it
3) Using promises with `axios.get().then()`

#
    useEffect(() => {
        console.log("Main Call!");
        
        return () => console.log ("Cleanup Call!");
    });
    
    // The console logs will trigger as follows
    // First render -----  Main Call!
    // Second render ----- Cleanup Call!
    // Second render ----- Main Call!
When `useEffect` is created and a function is passed as the first argument, the only thing we're allowed to return from it is another function (called the **cleanup function**). The returned cleanup function is then called BEFORE the main function passed to `useEffect` every time after the first render. 

   
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
The cleanup function will also be invoked once the component stops being shown, which can be useful in cases where we want to do cleanup for the entire component.

#   
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
Events in react **bubble up**, which is to say that they will propagate from the component where they're created through its parent components, and eventually into the HTML body. Manual event listeners can thus be set up to watch for events which bubble up. However, all event listeners created using `addEventListener` get called first, and the React event listeners get called AFTER them. The order inside those two categories is still from the most child element towards the most parent element. It's possible to cancel event bubbling, but it's considered bad practice as it can break other parts of the code.
  
Manual event listeners added with `addEventListener` are usually only called one time, so an empty array is usually passed as the second argument to the `useEffect` function. The callback is also usually defined as a seperate helper function inside `useEffect`, so that it can easily be cleaned up with the `removeEventListener`.

Custom hooks are the best way to create reusable code in a React project (besides components). They're created by extracting hook-related code out of a function component, and they always make use of at least one primitive (inbuilt) hook internally. Each custom hook should have **one** purpose, and they're great for tasks like data-fetching. The process of creating reusable hooks is hard to define, but could go something like this:
1) Identify each line of code in a component related to a single purpose
2) Identify the inputs to that code
3) Identify the outputs of that code
4) Extract the code into a seperate function, receiving the inputs as arguments and returning the outputs

### Refs
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
React Refs are a system which gives us direct access to a single DOM element that is rendered by a component, and they're used in place of the standard JS `document.querySelector(some_tag)`. Since all JSX elements - even the ones which look like standard HTML elements such as `<img>` - aren't HTML, but are eventually converted to real HTML, we thus have no way to reference them aside from using refs. They're created in the constructor, assigned to instance variables, then passed to a particular JSX element as props. They can be assigned to the `state` of a component, but since refs don't change over time, it shouldn't be done.
    
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
However, refs on things which are loaded from elsewhere (such as the images in the previous example) won't work properly, since they'll be called on objects whose content arrives after a certain delay. A way to properly use these refs is by adding an event listener which waits for them to load properly.

#   
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
Refs can be created with hooks using the `useRef` hook function. The `.contains` method is shared for all DOM elements, and allows us to see if an element is contained within another.
___
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
`this` in JS references a concrete instance of a class it's called from. When the callback in the above example is made, the `this` inside of the callback function becomes `undefined`, because the callback function implementation is taken from the instance and used on its own, thus losing the link needed to use `this`. One of the ways to fix this problem is by binding the callback function, then overwriting the current version of the function with the one that always has `this` bound to it. Another way to fix the problem is by turning the function into an arrow function, because arrow functions automatically bind the value of `this` for all the code inside the function. This makes it a good idea to always write callback functions as arrow functions, just to be safe.

Variables in `state` which we know are going to be arrays or objects should be initialized as empty arrays or objects, so that we don't get error messages when we call methods specific to those data types (such as `length`).

Arrow functions which are also `async` need to have the `async` keyword after the `=` sign, and before the parentheses which hold the parameters. <br>
For example `async myFun (params) {` becomes `myFun = async (params) => {`.

#
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
The `.map` method works on arrays and is passed a function which is applied to each element of an array, then used to generate a new array without mutating the original. The first argument that `map` returns is the currently selected element from the collection, while the second (optional) argument is the index of the element.

#
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
If rendering items **from a list**, each element should have a `key` prop in order to allow React to see which elements are already in the DOM, so that they don't have to be rerendered if we're only putting in additional elements. Note that the `key` property should be given to the root tag that's being returned. A `key` should be a value which is consistent and unchanging between rerenders (such as the `id` property often coupled with data).

#   

Sometimes we want to render multiple things which are already enveloped by a `<div>` inside the parent component, and we might not want to have the extra `<div>` enveloping the elements rendered inside the child element as well (like when borders are applied to `<div>` elements, to avoid double borders being drawn).

    <React.Fragment key={someKey}>
        // Other JSX elements
    </React.Fragment>
`React.Fragment` can be used in place of a `<div>` in this scenario. Note that the `key` special prop can also be applied to these placeholders, in order to avoid warnings related to missing keys later on.

#
    // Print "Hello!" after 10 seconds
    setTimeout(() => console.log("Hello!"), 10000)
The `setTimeout` function accepts two arguments. The first argument is the function which is called once the timer expires, and the second argument is the duration of time. `setTimeout` also returns an identifier, which can be used by the `clearTimeout` function to prevent the timeout function from activating.



