To mount a react component in a browser
`ReactDOM.render(..., mountNode)`

```js
const Button = (props) => {
  return (
  	<button>{props.label}</button>
  );
};

ReactDOM.render(<Button label ="Do"/>, mountNode); 
```
> components properties are immutable 

Class Component  
- render is what returns the component's JSX

```js
class Button extends React.Component {
	constructor(props) { 
  	super(props); // honor inheritance of this component
    
    // initialize
    this.state = { counter: 0 };
  }

	render() {
		return (
    // this keyword refers to the component instance we're handing off to ReactDOM
    	<button>{this.state.counter}</button>
    );
	}
}

ReactDOM.render(<Button />, mountNode); 
``` 
Shorter syntax
```js
class Button extends React.Component {
		// use a class property without a constructor call
    state = { counter: 0 };
  }

	render() {
		return (
    // this keyword refers to the component instance we're handing off to ReactDOM
    	<button>{this.state.counter}</button>
    );
	}
}

ReactDOM.render(<Button />, mountNode); 
```

Click handler
```js
class Button extends React.Component {
    state = { counter: 0 };
  	
    // Click handler for counter button
    // Defined on component as an instance property
    // Arrow function is bound to the component instance
    // Will now act as a prototype function on this class
    handleClick = () => {
    	// "this" === component instance
      this.setState({
      	counter: this.state.counter + 1
      })
    
    };
    

	render() {
		return (
    	<button onClick={this.handleClick}>
      	{this.state.counter}
      </button>
    );
	}
}

ReactDOM.render(<Button />, mountNode); 
```

If state depends on current state to avoid async problems
```js
class Button extends React.Component {
    state = { counter: 0 };

    handleClick = () => {

      // setState is async which just schedules and update, so multiple setState calls may be 				batched for performance.
      // So use this contract.
      // Only need to use this syntax if your update depends on the current state
      this.setState((prevState) => {
      	return{
        	counter: prevState.counter + 1
        }
      })
	};    

	render() {
		return (
    	<button onClick={this.handleClick}>
      	{this.state.counter}
      </button>
    );
	}
}

ReactDOM.render(<Button />, mountNode); 
```

> React component can only return 1 element, so everything needs to be in a parent div

Multiple components in 1 app

```js
class Button extends React.Component {
    state = { counter: 0 };

    handleClick = () => {
      this.setState((prevState) => ({
      	counter: prevState.counter + 1  
      }));
	};    

	render() {
		return (
    	<button onClick={this.handleClick}>
      	{this.state.counter}
      </button>
    );
	}
}

const Result = (props) => {
	return (
  	<div>...</div>
  );
};

class App extends React.Component {
	render() {
  	return (
    	<div> 
      	<Button />
        <Result />
      </div>
    )
  }
}

ReactDOM.render(<App />, mountNode); 
```
> important to think through where you define your state

Passing props
```js
// Class Component
class Button extends React.Component {
	// handleClick = () => {
	// this.setState((prevState) => ({
	// counter: prevState.counter + 1  
	// }));
	// };    

	render() {
		return (
    // needs to be a prop now since it is being passed down
    	<button onClick={this.props.onClickFunction}>
      	+1
      </button>
    );
	}
}

// Functional Component

// from the point of view of this result component, 
// the counter is not a state, it is just a value that the app component is passing to it
const Result = (props) => {
	return (
	// don't need "this" in a functional component   
  	<div>{props.counter}</div>
  );
};

// Main Class component
// The state only changes on the app component level
class App extends React.Component {
	// Needed to move to the parent component so both the button and result can have access
	state = { counter: 0 };
  
  incrementCounter = () => {
     this.setState((prevState) => ({
    		counter: prevState.counter + 1  
    	}));
  }
  
	render() {
  	return (
    	<div> 
      {/* Need to pass a reference to the component */}
      	<Button onClickFunction={ this.incrementCounter }/>
        <Result counter={ this.state.counter }/>
      </div>
    )
  }
}

ReactDOM.render(<App />, mountNode); 
```

Multiple buttons

```js
class Button extends React.Component {
	
	handleClick = () => {
    this.props.onClickFunction(this.props.incrementValue);
	};    

	render() {
		return (
			// need to invoke the onclick function to access increment value
      // call the handleClick so not creating a new function everytime
    	<button 
      	onClick={this.handleClick}>
      	+{this.props.incrementValue}
      </button>
    );
	}
}

const Result = (props) => {
	return (
  	<div>{props.counter}</div>
  );
};

class App extends React.Component {
	state = { counter: 0 };
  
  incrementCounter = (incrementValue) => {
     this.setState((prevState) => ({
    		counter: prevState.counter + incrementValue  
    	}));
  }
  
	render() {
  	return (
    	<div> 
      	<Button incrementValue={1} onClickFunction={ this.incrementCounter }/>
        <Button incrementValue={5} onClickFunction={ this.incrementCounter }/>
        <Button incrementValue={10} onClickFunction={ this.incrementCounter }/>
        <Button incrementValue={100} onClickFunction={ this.incrementCounter }/>
        <Result counter={ this.state.counter }/>
      </div>
    )
  }
}

ReactDOM.render(<App />, mountNode); 
```

Two ways to return an object:
```js
const x = () => ({ some: 'thing'})

const x = () => {
	return { some: 'thing'}
}
```