# Github Cards

## Styling
- `className` instead of class
```css
.info {
  color: red;
}
``` 
OR 
just write the CSS in the JS too (kind of like using inline styles)
```js
const Card = (props) => ( 
	<div style={{margin: '1em'}}>
	  <img src="http://placehold.it/75" /> 
    <div style={{display: 'inline-block', marginLeft: 10}}>
      <div style={{fontSize:'1.25em', fontWeight:'bold '}}>
      	Name
      </div>
      <div>Company</div>
    </div>
	</div>
)

ReactDOM.render(<Card /> , mountNode)
``` 

Spread operator
```js
const CardList = (props) => {
	return (
  	<div>
 		{/* Spread operator instead of <Card name={props.name} company={props.company}	etc	*/}
      	{props.cards.map(card => <Card {...card} />)}
  	</div>
  )
}
```

Form prevent default refresh
```js
class Form extends React.Component {
  // In a class component, do not use const for the function def
	const handleSubmit = () => {
  	// stops it from refreshing the page on submit
    event.preventDefault();
  }
  render() {
  	return (
    	<form onSubmit={this.handleSubmit}>
    	  <input type="text" placeholder="Github Username" required/>
        <button type="submit">Add Card</button>
    	</form>
    )
  }
}
```
controlled element
read input values
```js
class Form extends React.Component {
	// this ...
  state = { userName: '' }
  handleSubmit = (event) => {
    event.preventDefault();
    
  };
  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <input type="text"
        	// ... combined with this
          // creates a controlled element
          value={this.state.userName}
          onChange={(event) => this.setState({ userName: event.target.value })}
          placeholder="Github username" required />
        <button type="submit">Add card</button>
      </form>
    );
  }
}
```
> React components have a 1 way flow of data. Components can NOT change the state of their parents. But the App component can pass properties to other components. 
Parent can pass properties to the child. Both primitive values or function references that the child can invoke up the chain on the parent component itself.``
```js
class Form extends React.Component {
  state = { userName: '' }
  handleSubmit = (event) => {
    event.preventDefault();
    axios.get(`https://api.github.com/users/${this.state.userName}`)
			.then(response => {
      	// Calling parent component function passed by props
      	this.props.onSubmit(response.data);
      })   
  };
  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <input type="text"
          value={this.state.userName}
          onChange={(event) => this.setState({ userName: event.target.value })}
          placeholder="Github username" required />
        <button type="submit">Add card</button>
      </form>
    );
  }
}

class App extends React.Component {
  state = {
    cards: []
  };
  
  addNewCard = (cardInfo) => {
  	console.log(cardInfo)
  }

  render() {
    return (
      <div>
      	{/* Parent is passing a reference to a function that will be invoked in the form component*/}
        <Form onSubmit={this.addNewCard} />
        <CardList cards={this.state.cards} />
      </div>
    );
  }
}
```

empty input
```js
class Form extends React.Component {
  state = { userName: '' }
  handleSubmit = (event) => {
    event.preventDefault();
    axios.get(`https://api.github.com/users/${this.state.userName}`)
      .then(resp => {
        this.props.onSubmit(resp.data);
        // Set back to an empty input
        this.setState({ userName: '' });
      });
  };
  ```

Entire project
https://gist.github.com/samerbuna/7791e68fd93ccc47b8bf48f8a7f35b0f#file-script-js


```js
const Card = (props) => {
  return (
    <div style={{margin: '1em'}}>
      <img width="75" src={props.avatar_url} />
      <div style={{display: 'inline-block', marginLeft: 10}}>
        <div style={{fontSize: '1.25em', fontWeight: 'bold'}}>
          {props.name}
        </div>
        <div>{props.company}</div>
      </div>
    </div>
  );
};

const CardList = (props) => {
  return (
    <div>
      {props.cards.map(card => <Card key={card.id} {...card} />)}
    </div>
  );
};

class Form extends React.Component {
  state = { userName: '' }
  handleSubmit = (event) => {
    event.preventDefault();
    axios.get(`https://api.github.com/users/${this.state.userName}`)
      .then(resp => {
        this.props.onSubmit(resp.data);
        this.setState({ userName: '' });
      });
  };
  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <input type="text"
          value={this.state.userName}
          onChange={(event) => this.setState({ userName: event.target.value })}
          placeholder="Github username" required />
        <button type="submit">Add card</button>
      </form>
    );
  }
}

class App extends React.Component {
  state = {
    cards: []
  };

  addNewCard = (cardInfo) => {
    this.setState(prevState => ({
      cards: prevState.cards.concat(cardInfo)
    }));
  };

  render() {
    return (
      <div>
        <Form onSubmit={this.addNewCard} />
        <CardList cards={this.state.cards} />
      </div>
    );
  }
}

ReactDOM.render(<App />, mountNode);
```
