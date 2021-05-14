# web components engineering
WebComponent Creation and Maintainance or How to Optimize web components.

## What is a web component?
It's a Collection of CSS, HTML, JS Strings that need to get delivered to a viewer via different Loading Algorithms. The Component will get requested via
a server and then the client will use its loading Algorithms to Interpret the HTML Stream which can then lead to additional requests for different
parts of the Component.

## WebComponent Injection
Gets done via the HTML String Representation of the Component. So when a Component can return its HTML String Representation it is also able
To Handle its Life Cycle

Some Components do get shipped as ECMAScript/JS String inside the HTML String Representation and will then use the DOM API to render the view.
While the General Way is to insert a string into the Document and let the Browser create the DOM Representation from that String.

as the rule of thumb when you want serverside rendering in an Efficient Way. You need to get your Components to return String Representations. While there are ways To Static Render DOM only Components they are all Inefficient by Design as that has always a Higher Complexity than String Concatenation.

## WebComponent Types
- Static String Only
  - Also known as Templating and Template Components most time get rendered server-side or client-side with Static Content from an external data source
- Dynamic String Only
  - like Static String Only but they get rendered more often Also known as Template Partials most time.
- DOM Interfaces with View Updates created from Static or Dynamic String Only Components
  - They Do at last contain Script tags to handle Complex stuff like bindings and updates.
- DOM Only Components. That may or not Maybe able to be Serialized to a String Representation that is hydrate and dehydrate able

## What does String Only Mean?
String Only means that there will be no ECMAScript/JS Code. so no Script Tags the exact Oposits are Dynamic Interfaces and DOM Only Components.

## What does Serialization and Hydration as also DeHydration mean?
The process of turning a DOM Object into a String is called Serialization. This Serialized Representation can be Hydrated or Dehydrated.
The Terms Hydrated and Dehydrated can have 2 different meanings. 

hydrated for example can reference that the Component got Serialized with its Final Content and State while it can also mean that it has its optional
bindings and update handlers Applied so it depends on the type of web component that you talk about what hydrated and dehydrated do mean.

a dehydrated component for example can be as small as a single HTML TAG in its String representation that gets Hydrated (filled) with its content and handlers once it gets rendered via an additional SCRIPT Tag.
It can also be a collection (the full final view) of the Component missing only optional bindings and update handlers for user interaction.

a fully hydrated Component always Consists of its Final Useable State. Often called Initialised. 

and a String Only Component (Template) is always Hydrated often called Initialized.

## Code execution in HTML Viewers
There are different states when code gets executed you can define at what state your code should get loaded or executed via HTML Strings and the DOM API.
There are many Ways to Optimize Existing Code Execution and Load times so that everything only Happens when needed.

The most often used way of bundling all logic into a single script that you ship with your HTML String representation is most times when you do server-side rendering a bad idea at last when it loads all assets before they are even needed. While it can be the right thing to do when you Deliver Web Or Mobile Applications that should work offline.

## That Leads us to the follwing Framwork Conclusions
Todo.... I Should now prepare a demonstration of all the existing Frameworks and Show why they fail The goal of Fast Rendering Webpages Most of the Time.
I should also demo why the even fail when you create Large Complex Applications.

I should show Stealify Components and why they are designed the way they are designed and should clean up the loader Documentation and modules.

I should rant about WebPack and all the stuff that we as a community created to make our life more complex then it should be.

And i Should maybe Talk about Testing and Performance Budgeds that do give the user a better Expirence.


# About CSS
When you code custom Elements or Components you should make the Style editable
The right method to do that depends on the Component size on a element level its most time enough to be able to set the element.style attribute.
- https://stackoverflow.com/a/56706888/2950649
- https://developer.mozilla.org/de/docs/Web/CSS/@import


Also not that it is often clever to not depend on link rel stylesheet inclusion to get better performance often 
```
<style>@import url(":/mycss") </style>
```
is preferable and can also be easy inserted via js it also supports 3 parameter to import only on matching media querys read the @import docs.

also see: https://web.dev/defer-non-critical-css/

A Really good pattern is to inject your style programatical and make it identify able via a extra attribute on the style or link tag
this way you can check if the style exists already at the given place and only render it when it is needed.

The best way to Integrate styles is to use a style component a String only Component that renders the style tag for a given section. this allows good templating
and keeps the components crisp.

## If you want to deal with customElements that get inserted dynamical
First of all this Both are DOM Only patterns and should be only used to create Frameworks and Editors this is not the best way to render components in general.
For example backbone, react, vue, donejs, svelt and many others do not use customElement api by default. While Angular for example does use that API by default.
- use CustomElements Api
- use MutationObserver for on insert events use subtree true to do it on Application level.


A Litle Example to demonstrate access to a shared registrie based on ECMAScript Modules you can call any module with this as parameter.
```html
<link rel="preload" href="style.css" as="style" onload="import('./style.js').then(s=>{console.log(s);this.onload=null;this.rel='stylesheet';})">
```
