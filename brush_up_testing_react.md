# Testing React <!-- omit in toc -->
- [Intro](#intro)
- [Basic test (test state and props)](#basic-test-test-state-and-props)
- [Testing user interaction](#testing-user-interaction)
- [Testing whether a class method is called or not](#testing-whether-a-class-method-is-called-or-not)
- [Things to remember](#things-to-remember)

## Intro
  
I am assuming you already have a testing environment set up with all the tools installed: karma + webpack and enzyme. 

If not, these are good resources:
-  [How to test React with Jest & Enzyme](https://www.robinwieruch.de/react-testing-jest-enzyme)
-  [Using Enzyme with Karma](https://airbnb.io/enzyme/docs/guides/karma.html) 

## Basic test (test state and props)
We want to test the Button component defined in our react component [guide](brush_up_react_core).

```
import React from 'react';
import { shallow } from 'enzyme';
import Button from './Button';


function render(props) {
	return shallow(<Button {...props} />);
}

describe("Button component tests", () => {
    it("renders the prop correctly", () => {
        props = {
            buttonName: 'mockButton'
        };

        const renderedComp = render(props);

        expect(renderedComp.props().buttonName).toEqual(props.buttonName);
    });

    it("has correct initial state", () => {
        const renderedComp = render({});
        expect(renderedComp.state().numOfClicks).toEqual(0);
    });
});
```

## Testing user interaction

```
it("increase numOfClick by 1 when button is clicked", () => {
    const renderedComp = render({});
    expect(renderedComp.state().numOfClicks).toEqual(0);

    const button = renderedComp.find('button');
    button.simulate('click');
    
    expect(renderedComp.state().numOfClicks).toEqual(1);    
});
```

Always try to mock user interaction as much as possible i.e. simulating a click. 

## Testing whether a class method is called or not

```
it("increase numOfClick by 1 when button is clicked", () => {
    const functionToBeCalled = spyOn(Button.prototype, 'handleClick')

    const renderedComp = render({});
    const button = renderedComp.find('button');
    button.simulate('click');
    
    expect(functionToBeCalled).toHaveBeenCalled();    
});
```
## Things to remember

- You can have nested `describe` blocks. Your describe block should act like a test suite.
- Each `it` block should test one piece of functionality.
- Use `beforeAll()` and `beforeEach()`.
- `shallow` rendering will get the job done 90% of the time. But, there are times when you need to use `mount` to do a full mounting of your react component in your tests. To learn more about their differences, visit [here](https://gist.github.com/fokusferit/e4558d384e4e9cab95d04e5f35d4f913).

<div style="display: flex; justify-content:space-between">
  <a href="./brush_up_running_react.md"><p style="text-align: left;">Previous: Running React</p>

  <a href="./brush_up_react_best_practices"><p style="text-align: right;">Next: Best Practices</p></a>
</div>