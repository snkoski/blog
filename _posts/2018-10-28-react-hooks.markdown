---
layout: post
title:  "React Hooks"
date:   2018-10-28 12:00:00 -0400
categories:
---
Now that the 2018 React Conference is over I want to take a little time to look at what I thought was one of the coolest new features that was shown. Hooks.

Hooks allow us to add state to our functional components and will help us reduce our need to use classes.

Yep, you heard that right, your stateless components are stateless no more. But that’s not all, hooks will also give us access to lifecycle methods in our functional components.

First let’s take a look at a functional component that we’re used to using in React.

{% highlight jsx %}
import React from 'react';

const MyComponent = (props) => {
  return (
    <div>
      <h1>Count: {props.count}</h1>
      <h1>What's your name? </h1>
      <input value={props.name} />
    </div>
  (
}
{% endhighlight %}


MyComponent is pretty simple it displays a number count and a name that it receives from props. But what happens when we want to change the count? or if we want to change the name that is in the input?

Before the introduction of hooks we would have to rely on a parent component passing new props to this component anytime we wanted to change the output of the component. Now thanks to hooks we have the ability to change these properties inside the component itself.

Let’s look at how we can use hooks to give our functional component it’s own state.

import React, { useState } from 'react';
The first step is to import ‘useState’ from React, this is the function that makes all this magic possible.

{% highlight jsx %}
import React, { useState } from 'react';

const MyComponent = () => {
  return (
    <div>
      <h1>Count: {count}</h1>
      <button type="button" 
        onClick={}>
        Increase Count
      </button>
    </div>
  (
}
{% endhighlight %}

Let’s start with the counter in our component. We can remove the props we are passing to the component because we will be using its own internal state (that’s the whole point of hooks after all.) Instead of props.count we will change it to just count. We will also add the button that we will use to increase the count, but what should we be calling in the buttons onClick?

{% highlight jsx %}
<button type="button" onClick={() => {
          updateCount( prevState => prevState + 1)
        }}>
        Increase Count
</button>
{% endhighlight %}

This looks pretty good. Our onClick will call an updateCount function which will return our previous state + 1. Now we just have to figure out where that previous state is coming from.

{% highlight jsx %}
import React, { useState } from 'react';

const MyComponent = () => {
  const [count, updateCount] = useState(0)
  return ...
}
{% endhighlight %}

Remember that useState function? And remember how I said that’s what makes the magic possible? Now we get to see it in action. So what’s going on here? First we’re destructuring an array, ‘count’ will be the variable that holds our state and ‘updateCount’ will be the function we use to change our state. useState(0) will set our initial count state to 0.

{% highlight jsx %}
import React, { useState } from 'react';

const MyComponent = () => {
  const [count, updateCount] = useState(0)
  return (
    <div>
      <h1>Count: {count}</h1>
      <button type="button" 
        onClick={() => {
          updateCount( prevState => prevState + 1)
        }}>
        Increase Count
      </button>
    </div>
  (
}
{% endhighlight %}

Now that we have our counter working let’s figure out how we can get our name input to work the way we want it to.

{% highlight jsx %}
import React from 'react';

const MyComponent = () => {
  return (
    <div>
      <h1>What's your name? </h1>
      <input value={name} 
        onChange={handleNameChange}/>
    </div>
  (
}
{% endhighlight %}

Once again we can change props.name to just name. We also know that we want to be able to change the name in the input so we will give it an onChange event with a handleNameChange function that we will write.

{% highlight jsx %}
const [name, setName] = useState('Billy')
{% endhighlight %}

Before we write our handleNameChange function lets get another useState set up, this time for our name state. Just like with count we will destructure an array with our state variable ‘name’ and the function that will be changing the state, ‘setName’ in this case. Once again we will also set ‘useState’ to our initial state.

{% highlight jsx %}
function handleNameChange(e) {
  setName(e.target.value)
}
{% endhighlight %}

Now we can make our handleNameChange function. It’s a very basic function that uses the setName we made above to read the value of the input and change our state.

{% highlight jsx %}
import React, { useState } from 'react';

const MyComponent = () => {
  const [count, updateCount] = useState(0)
  const [name, setName] = useState('Billy')
  function handleNameChange(e) {
    setName(e.target.value)
  }
return (
    <div>
      <h1>Count: {count}</h1>
      <button type="button" 
        onClick={() => {
          updateCount( prevState => prevState + 1)
        }}>
        Increase Count
      </button>
      <h1>What's your name? </h1>
      <input value={name} 
        onChange={handleNameChange}/>
    </div>
  (
}
{% endhighlight %}

Our finished functional component now looks like this. It lets us change the count and name state in our component without having to change it from a functional component to a class. Pretty neat.