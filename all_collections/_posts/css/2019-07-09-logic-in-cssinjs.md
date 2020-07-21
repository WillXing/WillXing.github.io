---
layout: post
title:  "Don't be afraid to put logic in CSSinJS"
author: "Will Xing"
categories:
   - css
   - blog
date: 2019-07-09T00:00:00Z
---

Recently encounter an issue, we are having this micro front end architecture. It's a good concept but the following things are more likely to happen under this architecture:

1. Multi loading dependencies.
2. Inconsistent user experience.
3. Poison from the global variable/assets.

The problem I gonna talk about is cause by the multiple loading of dependencies, which produce the poison from the global Variables and assets from [css in js](https://github.com/MicheleBertoli/css-in-js).

When you using [emotion](https://github.com/emotion-js/emotion) in your project, you are likely to see following picture situation in your `<head />`.

![](/assets/images/multiple-emotion-load.png)

when you using emotion in your local single project, you might not really need to worry much, emotion handle it quite well and every style object have it's own hash name, which means they won't influence each other.

But when we loading different module to our micro frontend app, it’s turns bad...

# Example Case

## Expected behaviour

There is a component, when it’s at default state, it should show green background with border radius `5px`, and show green background but make it looks round when it’s activated(meeting certain condition), peeps don't want to have logic in the css in js file, but still sounds like a easy work, so here’s what I implemented
```js
const baseStyle = css({
  backgroundColor: green,
  borderRadius: 5,
})

const activatedStyle = css({
  borderRadius: '50%',
})

const SpecialButton = (props) => (
	<button className={`${baseStyle} ${props.activated ? activatedStyle : ''}`}>
    Example
  </button>
)
```
It’s working for sure, we got exactly what we want on the page.

## Issue after integrated

This component is sharing with other modules in the same web app, which sounds normal in most of the case but this one is cause troubles.

In the first module is loading this component and pass the `activated` as true, and then the second one is pass `activated` as false
```js
<App>
	<ModuleOne>
		<SpecialButton activated/>
	</ModuleOne>
	...
	<ModuleTwo>
		<SpecialButton/>
	</ModuleTwo>
</App>
```
What we expect to see is two buttons, one with round border, second one with `5px` radius border. But what we actually will see is two button with the same non activated style.

## Reason

There are two factors causing this.

1. CSS in JS load multiple times
2. Weight of the css when it's multiple defined

### CSS in JS load multiple times

- Look at the screen shot I post in the beginning, when we load multiple modules in the app, the emotion will compile and load, when two module using the same components, the same style will load multiple times.
- Another thing is cause by our implementation, the `activated` style will not load because the `className` is dynamic in the component, and it's not activated in the second one.
- And plus when emotion compile style, the class name is not a random hash, if the style is the same, they will have the same class name.

 So the style probably will looks like follow after compiled:
```html
    <style type="text/css" data-emotion>
    	.css-9k2a3 {
        background-color: green;
        border-radius: 5px;
      }
      .css-2o9p0 {
        border-radius: 50%;
      }
    </style>
    <style type="text/css" data-emotion>
    	.css-9k2a3 {
        background-color: green;
        border-radius: 5px;
      }
    </style>
```
### Weight of the css when it's multiple defined

Firstly we can know the button will looks like following:
```html
  <button class="css-9k2a3 css-2o9p0">Example</button>
  <button class="css-9k2a3">Example</button>
```
So the class name `css-9k2a3` is defined two times, which make it's weight heavier than the class which been only defined once, which is `css-2o9p0`, so when have class name like `class="css-9k2a3 css-2o9p0"` for the first button, it actually will apply only the first one, because it's heavier.

## Solution

1. If you using css in Js, maybe emotion, find out if you can put label on the class, to make it more unique, or maybe apply the context there if it's available.
2. Put the logic into the css file, don't apply multiple classes on the same element. (**personal recommended**)
```js
        const buttonStyle = activated => css({
          backgroundColor: green,
          borderRadius: activated ? '50%' : 5,
        })
```
3. Don't use css in js but use sass or less or native css with [BEM](http://getbem.com/) naming

## Ending

When you build an module in a big app, css in js probably will be a very good choice for you because you don't need to worry much about the duplicated styling or the weight of the styling.

Personally I will say use css in js  but writing it like normal css (which like the example code above) is not a good practice, you might end up to meet the problem like this. Don't need to be afraid to put the logic inside the css, this is way to make it unique and reduce the unexpected behaviours. Because they are basically js file after all.

Don't worry much about the testing, if write them in the native css way, then combine the logic with the native css weight system together actually make it much much harder to test.