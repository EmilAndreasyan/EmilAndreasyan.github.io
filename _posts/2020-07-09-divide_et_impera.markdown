---
layout: post
title:      "DIVIDE ET IMPERA"
date:       2020-07-10 03:36:38 +0000
permalink:  divide_et_impera
---



As it sounds and feels, when you write a web page with React, it should be divided in order to be maintained and supervised easily, thus implementing separation of concepts. In this final project called 'Travista', I tried to bring to users the travel experience of their favorite countries and their cities (the country has many cities and cities belong to the country). It was written as a full-stack web page, with Rails and React running as back-end and front-end, respectively. Without thoroughly explaining the Rails part on the back-end, which was presented in previous projects and blog posts, we’ll try to understand all React components from the front-end side's perspective. However, some points of the Rails side should be underlined.
Two resources with tables were created for countries and cities, country_id foreign key for cities, models, controllers, serializers, CORS (cross-origin resource sharing), routes. It should be mentioned, that routes are nested, like in the example below:
 ```
namespace :api do
  namespace :v1 do
    resources :countries do 
      resources :cities do
        end
    end    
  end
end
```
This is important to bear in mind when we try to access nested attributes from the front-end side later on. By default, Rails runs on ‘http://localhost:3000’ server. That means, to have separate servers for front-end and back-end, we need to specify in out React application dependencies the port, which should differ from 3000. For instance, in package.json, we can write
`start": "PORT=3001 react-scripts start`
In React side, after running `npm i global create-react-app’ and ‘create-react-app <app-name>`, we should set our app further by installing Redux, 'redux-thunk', 'react-router-dom' and save them, and as you may guess, we could run it all at once by the following code in the command line:
`npm i redux && npm i react-redux && npm i redux-thunk && npm i react-router-dom --save`
In index.js, we should import 'Provider', 'thunk' (for making asynchronous web requests), 'createStore', 'applyMiddleware', 'compose', 'BrowserRouter', Redux development tools (quite handy one), and wrap our main App Component in Provider:
ReactDOM.render(
  <Provider store={store}>
    <Router>
    <App />
    </Router>
    </Provider>,
  document.getElementById('root')
);

**The Components.** Components are the heart of React, providing two major advantages, namely, 'state' and 'props'. State is what is adjacent for the component and what constantly changes in response to user's activity (filling out forms, clicking buttons, etc), whereas props (short for properties) are what child components can inherit from their ancestors. Also, components should be separated by their functionality, as a container (written as class components) and presentational components (written as 'consts' with arrow function). The main difference between container and presentational components is that the latter has no state and used only for displaying the data received from the parent, container component (accessed via props).

```
import React from 'react';
import { connect } from 'react-redux';
import {Route, Switch, Link} from 'react-router-dom';
import {fetchCountries} from '../actions/fetchCountries';
import CountryInput from '../components/countries/CountryInput';
import Countries from '../components/countries/Countries';

class CountriesContainer extends React.Component {
    
    componentDidMount() {
        this.props.fetchCountries();
    }
    
    render() {
        return (
            <div>
                <Link to='/countries/new'>Add New Country</Link><br/>
                <Switch>
                <Route path='/countries/new' component={CountryInput}/>
                <Route path="/countries" render={(routerProp) => <Countries {...routerProp} countries={this.props.countries}/>}/>
                </Switch>
            </div>
        );
    }
}

const mapStateToProps = (state) => {
    return {
        countries: state.countries
    };
};

export default connect(mapStateToProps, {fetchCountries})(CountriesContainer);
```


We see, that top level CountryContainer, passes data to its child, Countries, as props:
`<Countries countries={this.props.countries}/>`
In its turn, Countries receive props (inherited from CountriesContainer parent) which are accessible by ‘this.props’ syntax:

```
import React from 'react';
import {Link} from 'react-router-dom';
import CountryShow from './CountryShow'
import {connect} from 'react-redux';
import {deleteCountry} from '../../actions/deleteCountry'

class Countries extends React.Component {
    handleDelete = (country) =>{
       this.props.deleteCountry(country.id)
    }
    render(){
        const {countries} = this.props
    return (
        <div className="cards">    
            {countries && countries.map((country) => (
                <ul key={country.id}>
                    <Link to={`/countries/${country.id}`}><li>{country.name}</li></Link><CountryShow country={country} handleDelete={this.handleDelete}/>
                </ul>
            ))}
        </div>
    );
}
};

export default connect(null, {deleteCountry})(Countries);
```

In its turn, Countries component can pass its props (inherited from CountriesContainer parent) to its own child, CountryShow (and, actually, it does):

```
import React from 'react';
import {Link, Route, Switch} from 'react-router-dom'
import CountryEdit from './CountryEdit'
import CitiesContainer from '../../containers/CitiesContainer';

const CountryShow = (props) => {
    console.log(props);
    return (
        <div className="card">
<h3>
{props.country ? (<div>{<img src={props.country.flag_url} alt={props.country.name}/>}<p> Language: {props.country.language}</p> <p> Currency: {props.country.currency}</p> <p>Area: {props.country.area}</p></div>) : null}
</h3>
<button onClick={() => props.handleDelete(props.country)} className="btn btn-danger">Delete {props.country.name}</button><br/>
<Link to={`/countries/${props.country.id}/edit`}><button className="btn btn-secondary">Edit {props.country.name}</button></Link><br/>
<Link to={`/countries/${props.country.id}/cities`}>Explore cities of {props.country.name}</Link>
<Switch>
<Route path={`/countries/${props.country.id}/edit`} render={(routerProps) => <CountryEdit {...routerProps} country={props.country}/>}></Route>
<Route path={`/countries/${props.country.id}/cities`} render={(routerProps) => <CitiesContainer {...routerProps} country={props.country}/>}></Route>
</Switch>
</div>
    )
}

export default CountryShow;
```


The last one is a presentational component we indicated above (to differentiate, they are written in 'const', present data, and don't have state). Actually, the props can be inherited in two ways in presentational components, or by passing props and calling 'props.<attr_name>' in the function body, either restructuring 'props' by their names passed into an object as an argument ({<attr_name>}) and called directly by their names in function body (<attr_name> instead of props.<attr_name>).
**Redux.** Another major part of creating web applications is the Redux store. It can be avoided in smaller applications but is almost essential in larger ones. Despite taking some time and effort for the initial setup, it pays off a great deal later on. To put it simply, Redux provides a single source of truth and is accessible across all components. In other words, if a child component wants to connect with the Redux store, it can do it now directly, without climbing up the parent tree. To instantiate, we can bring the form that the user fills out, written as a component:

```
import React from 'react'
import {connect} from 'react-redux';
import {addCountry} from '../../actions/addCountry';

class CountryInput extends React.Component {
    state = { name: '', flag_url: '', capital: '', language: '', currency: '', area: '' }

    handleChange = (event) => {
        event.preventDefault();
        const {name, value} = event.target
        this.setState({[name]: value}) // without brackets will interpret name as key of object!
    }

    handleSubmit = (event) => {
        event.preventDefault()
        this.props.addCountry(this.state)
    this.setState({name: '', flag_url: '', capital: '', language: '', currency: '', area: '' })
    }

    render() { 
        const {name, flag_url, capital, language, currency, area} = this.state
        return ( 
        <>
    <form onSubmit={this.handleSubmit} className="cards">
        <label htmlFor="">Country Name: 
<input type="text" onChange={this.handleChange} value={name} name='name'/></label><br/>
<label htmlFor="">Flag: 
<input type="file" onChange={this.handleChange} value={flag_url} name='flag_url' accept=".doc,.docx,application/msword,application/vnd.openxmlformats-officedocument.wordprocessingml.document"></input></label><br/>
<label htmlFor="">Capital: 
<input type="text" onChange={this.handleChange} value={capital} name='capital'/></label><br/>
<label htmlFor="">Language: 
<input type="text" onChange={this.handleChange} value={language} name='language'/></label><br/>
<label htmlFor="">Currency: 
<input type="text" onChange={this.handleChange} value={currency} name='currency'/></label><br/>
<label htmlFor="">Area: 
<input type="text" onChange={this.handleChange} value={area} name='area'/></label><br/>
<input type="submit" className="btn btn-primary"/>
    </form>
        <p>{this.state.name}</p>
        <p>{this.state.capital}</p>
        <p>{this.state.language}</p>
        <p>{this.state.currency}</p>
        <p>{this.state.area}</p>
        </> );
    }
}

export default connect(null, {addCountry})(CountryInput);
```

You can notice two weird imports and one export added:
`import {connect} from 'react-redux`;
`import {addCountry} from '../../actions/addCountry'`;
`export default connect(null, {addCountry})(CountryInput)`;

‘Connect’ is what connects our component to Redux store, more specifically, we are connecting ‘CountryInput’ component with ‘addCountry’ function, imported from actions folder's addCountry.js file. But what this action is about? Let’s go inside and take a look:

```
export function addCountry (country) { // country == this.state from CountryInput
    const BASE_URL = 'http://localhost:3000/api/v1'
    return (dispatch) => {
    fetch(`${BASE_URL}/countries`, {
        headers: {
            'Content-Type': 'application/json',
            'Accept': 'application/json'
        },
        method: 'POST',
        body: JSON.stringify(country)
    })
    .then(resp => resp.json())
    .then(country => dispatch({type: 'ADD_COUNTRY', payload: country}))
}
}
```

So, when we trigger ‘handleSubmit’ function, we invoke ‘this.props.addCountry(this.state)’. In this case, our component doesn’t receive props from its parent, but from Redux store instead ('addCountry' is a function from Redux actions which takes the state of our component as an argument)!
And this is one of the moments of truth when we see how our React component fetches data from the back-end server! Fetch function is wrapped into dispatch (which is inherited from Redux Thunk), then we dispatch the returned data in an object with keys ('type', 'payload'), and values (‘ADD_COUNTRY’, 'country'), respectively.
**Reducer.** After fetching data from back-end via Redux action, we should go to reducer, which is plain old JavaScript function written as switch statement:

```
export default function countriesReducer(state = {countries: [], cities: []}, action) {
  switch (action.type) {
    case 'FETCH_COUNTRIES':
     return {...state, countries: action.payload}
    default:  
    return state
  }    
  }
```
Here, we check the action type from our addCountry.js file, which turns out to be ‘FETCH_COUNTRIES’, then we return the copy of our state’s countries array (via spread operator) and insert into it the payload's value we received from the action (‘action.payload’ from reducer is the same as ‘action: country’ from addCountry.js file!). This is how actions interact with reducers!
So, to recap, as mentioned earlier, when our website grows in components and complexity, it’s important to have a single source of truth (Redux reducer) and directly connect to the Redux store, thus simplifying data management and separating responsibilities.
It is arguable what Julius Caesar really meant when utilizing his famous **‘divide et impera’**. Was it about dividing and ruling the widespread Roman Empire, or it was a predecessor for crafting React library, cried out throughout centuries to eventually reach us? Unlike the React library, we were not as rapid to ‘react’ as he would have wanted us to be!

https://github.com/EmilAndreasyan/Travista-frontend
https://github.com/EmilAndreasyan/Travista-backend
