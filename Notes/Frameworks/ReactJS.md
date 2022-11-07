### What is React?
- A JavaScript library maintained by facebook

### Why use React?
- Component based architecture
- Reusable code
- Declarative : Each step is not needed to be informed
- e.g.
```
  let li = document.createElement('li');
  li.className = "c1";
  li.textContent = "Imperative way of programming";
  root.appendChild(li);
  
  // VS
  
  ReactDOM.render(<li className="c1">Declarative way of programming</li>, root);
```
- Efficient updating and rendering of components
- Seamless integration into your application

### Creating a React application
- to install the package we use `npm`. While, to install and execute package at the same time use `npx`
- to use npm, we have to install 'create-react-app' package first `npm i -g create-react-app`
- `npm create-react-app project-name`
- `npx create-react-app project-name`

### Folder structure
- _package.json_ : metadata about the project and its dependancies
- _package-lock.json_ : metadata about core dependancies (dependancies for dependancies)
- _node_modules_ : files and package install directory
- _public_ : contains index.html and main files
- _src_ : directory for components and related files.

### Components in React
- **root** component is used to wrap all the components of an app
- We can have multiple custom components inside **root** div
- Components can be written in _.js_ or _.jsx_ extensions
- They describe a part of UI
- Components are reusable and can be nested inside other components
- 2 types of components
	- _Stateless Functional component_ : a plain javascript function returning html
	- _Stateful Class component_ : a class extending component class (`React.Component`) containing a render method returning html

### Functional components
- to create a custom functional component, create a new folder named **components**
- Inside **components** folder, create a .js file with name of component. Use PascalCase naming convention
- File should contain a function that returns result html.
- Use `export default` to export a default method (or we can use general export but we need to take care of names while importing)
- **default export vs named export**:
	- use of default export allows us to import the component/module by any name
	- using named export, we can only import by using the method/property names mentioned in the export module. Use `{}` for importing specific property or `*` for importing all properties using `as` clause.
- Functional components can optionally recieve **props** as input and append them into data as required
- No `this` keyword
- No states receivable hence aka stateless components. This is solved by using **hooks** (more on hooks ahead)

### Class components
- Similar to functional components but are ES6 classes which return a function that returns html/data
- Apart from **props**, class components can also recieve private/restricted **states** to describe UI
- Maintain their own private data - states hence aka stateful
- Allow complex UI logic
- Provide lifecycle hooks

### Hooks update
- Introudced in _React 16.7.0-alpha_
- Allow using states with functional components similar to lifecycle hooks in Class components
- This was a backwards-compatible change


### JSX
- JSX (javascript XML)  is a js extension which allows us to write HTML directly into .js file
- Main reason of import `React` to all files is `React` converts JSX to JavaScript
- JSX have tag name, attributes and children
- JSX transpiles to pure JavaScript for browser
- JSX must be returned a single parent element only. To do this without adding a seperate tag, we wrap elements in a empty tag `<></>`
- JSX has a unique way to enclose JavaScript code inside curly braces `{console.log("JSX")}`
- Different ways to create an element in React
1. return html inside functions
	```
	class DefaultCE extends React.Component{
		render(){return <div>Default Class Export</div>}
	}
	export default DefaultCE
	```
2. using `React.createElement(element, attributes, content/children)`
	```
	const CustomDiv()=>{
		return(
			React.createElement(
			'div',
			{id:'jsx1', className:'jsx'},
			React.createElement('div',null,'JSX1-React.createElement()')
			)
		)
	}
	export default CustomDiv
	```
- Differences between **JSX** and **JS**
	1. class -> className
	2. for -> htmlFor
	3. camelCase property naming convention
		- onclick -> onClick
		- tabindex -> tabIndex
		
### props
- **props** short for **properties**
- props are custom properties that we need to pass to the element as a variable
- props are passed to a JSX tag as an attribute. e.g. `<Greet name="my_name" />`
- Multiple props can also be passed to the tag. e.g. `<Greet name="my_name" surname="my_surname" />`
- props are accessed using curly braces inside the component AND passing the props object as a parameter if function export. e.g. 
	```
	const Greet = (props)=>{
		return <div>values : {props.name}, {props.surname}</div>
	}
	```

- Children props are the content that is unknown and passed to JSX tag as a chidren e.g.
	```
	<PropsDemo>
        <p>At vero ia omnis voluptas assumenda est, omnis dolor repellendus.</p>
	</PropsDemo>
	```
	`return <div> values : {props.children}</div>`
- In class components, props are accessed using `this` reserved keyword. There is no need to pass props as argument to the class component. e.g. `return <div>values : {this.props.name}, {this.props.surname}</div>`
- **PROPS are Immutable**. We cannot change props in any case hence we use **component states** to pass mutable arguments.

### states
- **states** are used to modify the provided properties unlike props.
- **states** are properties that we can modify as per requirements.
- We can achieve this using a constructor in a class component. We create an object this.state in the constructor.
	```
	class StatesDemo extends React.Component{
		constructor(){
			// super keyword since we are extending an existing class
			super()
			// now create state object
			this.state = {
			  message : 'Subscribe now.'
			}
		}
		// next, render the element using this.state.parameter method
		render(){
			return{
				<div>{this.state.message}</div>
			}
		}
	}
	```
- We can add a function to modify this state using onClick event on the button and providing a function of the same class.   
	While adding a method in the class, make sure to use this.setState method of React.Component class
	```
	changeMessage = ()=>{
		this.setState({
		  message : "Thank you!"
		})
	  }
	```
	and the event on button is added as follows    
	`<button type="button" onClick={()=>this.changeMessage()}>subscribe</button>`
	
### setState
- To modify a state of an object, we must use `setState` method. If not used,the values are modified but they are not rendered to the dom.
- e.g. below is a function for the click event on a button used without setState method
	```
	increment(){
		this.state.count++;
		console.log(this.state.count);
	}
	// this will print correct modified values in the console but will not be rendered to the DOM, hence useless
	// this is the main reason to use setState method
	```
- `setState` recieves an object as argument which contains properties that are needed to be modified
- We observe that after using `setState`, the values in console may be different from actual rendered value. This is because setState is **Asynchronous**
- To execute the rest code the values are modified in `setState`, we can use **callbacks**
	```
	minus = ()=>{
		this.setState({
		  count : this.state.count-1
		},()=>console.log(this.state.count))
	}
	```
	we can use arrow function, normal functions or function names as callback
- React is highly efficient and thus uses asynchronous state updates i.e. React may update multiple setState() updates in a ***single go***.
	
- **DOs and DON'Ts** for setState
	- use `setState` instead of directly updating the values, this will update the values in console  but will not be rendered into DOM
	- use a callback to execute the underlying code. As `setState` is asynchronous, rest code will execute before the state of component is changed
	- to change a state depending upon the previous state, pass the previous state as an argument to the setState method.
	e.g.
	```
	// we proide prev_state object as previous state to change state according to its previous value
	plus = ()=>{
		this.setState(prev_state=>({
			count: prev_state.count+1
		})
	}
	// we can also add another parameter as props to add props dependent changes
	plus = ()=>{
		this.setState((prev_state, props)=>({
			count: prev_state.count	+ props.value
		})
	}
	```

### props vs states 
|props|state|
|:--|:--|
|props get passed to the component|state is managed within component|
|are function parameters|are variables declared in function body|
|immutable|changeable|
|props- functional component|useState hook- functional component|
|this.props- class component|this.state- class component|

### Destructuring props and state
- Destructuring is used to consider only useful properties instead of having all the properties. e.g. name,surname,age are passed to a component. But I only need name and age then
	```
	const Greet = (props) => return(<>{props.name},{props.age}</>)
	// instead of passing an object named props like above, we destructure the object as belows
	const Greet = ({name,age}) => return(<>{name},{age}</>)
	// or we can do it in other way like
	cosnt Greet = (props){
		const {name,age} = props
		//const {state1,state2} = this.state  // for states. we need to study hooks to use states inside function components
		return(<>{name},{age}</>)
	}
	```

### Event handling
- Add events to an element using JSX.
- Instead of using `onclick` use `onClick` and pass a function as value instead of a string.   
	`<button onClick={clickHandler}>click</button>`
- **_Note:_** if we add paranthesis at the end of function, it is a function call which will be executed even when event is not triggered and it will also will do nothing on event trigger. For class components, it keeps rerendering the state and causes an infinite loop. Therefore value for event handler is a function and not a function call
- For class exports, methods are accessed using `this` keyword.   
	`<button onClick={this.clickHandler}>click</button>`
- Events are similar to **vanilla JS**

### Event binding
- **Why event binding is important?**
	- If we `console.log(this)`, `undefined` return value is printed in console. Hence we cannot use `this.setState` method in class exports. To avoid this, we use event binding.
	- There are multiple ways to bind handlers.
- **Binding methods:**
	```
	// main function
	clickHandler(){
		console.log(this)
	}
	```
	1. Using `bind` keyword.   
	`<button onClick={this.clickHandler.bind(this)}>click</button>`   
	   Will return an **EventBinding** object which contains properties like method,props,state etc
	2. Using an arrow function inside event handler   
	`<button type="button" id="bind1" onClick={()=>this.clickHandler()}>{message}</button>`
	3. Binding inside the constructor   
	`this.clickHandler = this.clickHandler.bind(this)`
	4. using an arrow function instead of a regular function
	```
	// main function
	clickHandler = ()=>{
		console.log(this)
	}
	```
	
### Methods as props
- To use methods of a parent component for child component, we have to pass methods as props.    
	```
	// this is parent class constructor and method 
	constructor(props){
		super(props)
		this.state={
		  name : "_Parent"
		}
		this.greet = this.greet.bind(this)
	}
	greet(){
      console.log(`Hello ${this.state.name}`)
	}
	```
	```
	// passing parent method as prop to child
	<ChildComp greetHandler={this.greet} />
	```
- To pass arguments for methods, we use arrow function
	```
	// parent method
	greet(param){
      console.log(`Hello ${this.state.name}`)
	  console.log(param)
	}
	```
	`<ChildComp greetHandler={()=>this.greet('parameter')} />`
	
### Conditional rendering
- Methods:
	1. if/else
	2. Element variables
	3. Ternary operator
	4. Short circuit operator
- if...else   
	We can use if else statement like javascript to return a specific element for `render()` method. But we cannot use conditionals inside **JSX** (i.e. inside tags)
	```
	render(){
		if(condition===true){ return <div>Hello, {this.state.name}</div> }
		else{ return <div>Hello, guest</div> }
	 }
	```
- Element variables   
	We can store tag as a variable and render it accordingly
	```
	render(){
		let message
		if(condition===true){ message = `Hello, ${this.state.name}` }
		else{ message = "Hello, guest"}

		return( <div>{message}</div> )
	 }
	```
- Ternary operator   
	Ternary operator makes it easier to add conditional inside **JSX** 
	```
	render(){
		return(
		  (condition===true) ?
		  (<div>Hello, {this.state.name}</div>) :
		  (<div>Hello, guest</div>)
		)
	  }
	```
	`Syntax: <condition>?<do_if_true>:<do_if_false>`
- Short Circuit operator   
	The method evaluates the first condition, if it is true then evaluates second condition otherwise doesn't return any value.
	This is used when we have to render an element if the condition is true and render nothing when condition is false
	Doesn't work for 0 and 1 values, works only for boolean values
	```
	render(){
		return(
		  this.state.isLogged && <div>Hello, {this.state.name}</div>
		)
	  }
	```
	
### List render
- JS array methods:
	1. **array.map** : returns a new array by performing function provided on each element
	```
	let arr = [1,3,4,5,6]
	let result1 = arr.map(x =>{return x*2})
	```
	2. **array.filter** : returns a new array by filtering values according to given condition inside funtion
	```
	const gthan2 = (val)=>{
		return val>2
	}
	let result2 = arr.filter(gthan2)
	```