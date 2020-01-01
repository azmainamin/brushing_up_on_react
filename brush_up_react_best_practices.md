# React Best Practices <!-- omit in toc -->
- [Container and View components a.k.a. Smart and Dumb components](#container-and-view-components-aka-smart-and-dumb-components)
- [Higher Order Components](#higher-order-components)
- [Controlled Components](#controlled-components)
- [A component should be an self contained module](#a-component-should-be-an-self-contained-module)
- [Create a wrapper 3rd party components](#create-a-wrapper-3rd-party-components)
- [Use propTypes and defaultProps](#use-proptypes-and-defaultprops)
- [When updating state, always use prevState](#when-updating-state-always-use-prevstate)

## Container and View components a.k.a. Smart and Dumb components
This by far my favorite React pattern and enforces the divine coding rule of single responsibility. The idea is simple: encapsulate your logic i.e. business logic or fetching data into a component (container or smart components) and your presentation logic into another component (view or dumb component) who's only responsibility is to take data and  render html. The container component will do all the heacy lifting and render the view component passing in the required data.

Learn more from the man himself: [smart vs dumb components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0).

## Higher Order Components
The name higher order component comes from higer order functions, which takes other functions as arguments. HoC takes in a component as an arg, adds some logic and returns another component with the added logic. The idea is that the 'added logic' can be reused agnostically with any other component, thus keeping your code 'DRY'. 

[Learn More](https://reactjs.org/docs/higher-order-components.html).

## Controlled Components
Controlled components deal with HTML form elements such as `<input>`, `<textarea>`, and `<select>`. You wrap these elements with a custom component and dictate their behavior, like what happens when a user clicks on a radio button, using the `state` of the wrapper component. Why you ask? It has something to do getting data from DOM more efficiently. Probably. 

[Learn More](https://reactjs.org/docs/forms.html).

## A component should be an self contained module
The whole philosophy behind React is that you create resuable and modularized components. This ensures maintainability and consistency throughout the app. I like to keep my component source code, test code and css files in the same dir. It makes it easier to find everything you need for that component. You don't have to go on a wild hunt. Moreover, it reinforces the idea that your component is the source of reusability. If you want to use your component in a different project, you can just copy that dir and use the component.

## Create a wrapper 3rd party components
This helps with consistency throughout your app. Let's say you are using a third party carousel component. Wrap it in a custom `<MyCarousel/>` component and add your own modifications. Now when you need to use it again somewhere else, you can just use `<MyCarousel/>`. You don't need to add your modifications again. 

## Use propTypes and defaultProps
One of JS's curse (or blessing, depending who you ask) is that it's not type safe. Ofcourse you can use TypeScript, but that's not a part of this doc. One way you can add a little bit of type safety to your components and make them more robust is by using [propTypes](https://reactjs.org/docs/typechecking-with-proptypes.html).

Use `defaultProps` which are static class properties of React.Component that the component uses if no props are passed to it.  

## When updating state, always use prevState
Its very easy to forget that setState is async, and therefore the current state might not be what you think it is. So when you call setState, the safest bet is to use the prevState to update the newer state.
```
this.setState((prevState) => {
            return { numOfClicks: prevState.numOfClicks + 1 };
});
```
