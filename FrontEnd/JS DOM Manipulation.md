# What is the DOM

The **Document Object Model (DOM)** is a **programming interface** for HTML and  XML documents. It represents the page so that programs can change the  **document structure, style, and content**. The DOM represents the document  as nodes and objects.

> API (HTML or XML page) = DOM + JS (scripting language) 

![img](/home/alan/Documents/Notes/dom.png)

# Important Data Types

| `document`     | When a member returns an object of type `document` (e.g., the **ownerDocument** property of an element returns the `document` to which it belongs), this object is the root `document` object itself. The [DOM `document` Reference](https://developer.mozilla.org/en-US/docs/DOM/document) chapter describes the `document` object. |
| -------------- | ------------------------------------------------------------ |
| `element`      | `element` refers to an element or a node of type `element` returned by a member of the DOM API. Rather than saying, for example, that the [document.createElement()](https://developer.mozilla.org/en-US/docs/Web/API/Document/createElement) method returns an object reference to a `node`, we just say that this method returns the `element` that has just been created in the DOM. `element` objects implement the DOM `Element` interface and also the more basic `Node` interface, both of which are included together in this reference. |
| `nodeList`     | A `nodeList` is an array of elements, like the kind that is returned by the method [document.getElementsByTagName()](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementsByTagName). Items in a `nodeList` are accessed by index in either of two ways: 			 				list.item(1) 				list[1] 			 			These two are equivalent. In the first, **item()** is the single method on the `nodeList` object. The latter uses the typical array syntax to fetch the second item in the list. |
| `attribute`    | When an `attribute` is returned by a member (e.g., by the **createAttribute()**  method), it is an object reference that exposes a special (albeit  small) interface for attributes. Attributes are nodes in the DOM just  like elements are, though you may rarely use them as such. |
| `namedNodeMap` | A `namedNodeMap` is like an array, but the items are  accessed by name or index, though this latter case is merely a  convenience for enumeration, as they are in no particular order in the  list. A `namedNodeMap` has an item() method for this purpose, and you can also add and remove items from a `namedNodeMap`. |

# DOM Creation

DOM Node responds to a tag, a text or a HTML attribute. `nodeType` attribute represents the current node type of which 1-Element, 2-Attribute and 3-Text.

1. `document.createElement('div')`
2. `document.createTextNode('hello world')`
3. `document.appendChild(<node>)`

# DOM Query

1. use css property:
   1. `document.querySelector(".class")`
   2. `document.querySelectorAll("div.note, div.alert")`
2. use specific feature
   1. `document.getElementById('xxx')`
   2. `document.getElementsByClassName('xxx')`
   3. `document.getElementsByTagName('xxx')`
3. use reference of the parent/child DOM
   1. `element.parentElement` / `element.parentNode`
   2. `element.children`
   3. `element.querySelector(".class")` and all other query methods
   4. `ele.firstElementChild` / `ele.lastElementChild`
   5. `ele.nextElementSibling` / `ele.previousElementSibling`

# DOM Manipulation

## Structure

1. `ele.appendChild(el)`
2. `ele.removeChild(el)`
3. `ele.replaceChild(el1, el2)`
4. `parentElement.insertBefore(newElement, referenceElement)`

## Content

`ele.innerText=`

- `innerHTML`：内部HTML，`content<br/>`；
- `outerHTML`：外部HTML，`<div>content<br/></div>`；
- `innerText`：内部文本，`content `；
- `outerText`：内部文本，`content `；

![DOM content](/home/alan/Documents/Notes/dom-content.gif)

## Attribute

1. `el.attributes`
2. `el.getAttribute('class')` / `el.setAttribute('class','highlight')`
3. `el.hasAttribute('class')`
4. `el.removeAttribute('class')`

# DOM Event

capture phase -> target phase -> bubbling phase

https://developer.mozilla.org/en-US/docs/Web/Events

1. `el.onclick = function(){}` each element can only have one function
2. `el.addEventListener(<type>,<function>,<option>)` each element can have multiple callback functions
   1. <type>: event type like "click", https://developer.mozilla.org/zh-CN/docs/Web/Events
   2. <option> how this function deals with its child element (default false)
      1. true: this function will be triggered in capture phrase
      2. false: this function wiil be triggered in bubble phrase

# AJAX

AJAX stands for **A**synchronous **J**avaScript **A**nd **X**ML.

The two major features of AJAX allow you to do the following:

- Make requests to the server without reloading the page
- Receive and work with data from the server

1. Prepare the request

   1. `let httpRequest = new XMLHttpRequest();`
   2. `httpRequest.onreadystatechange = <callback function>` to register the call back function to process the callback data
   3. `httpRequest.open(<HTTP METHOD>, <url>)` http methods like GET, POST must in upper case
   4. `httpRequest.send()` to finally initiate the request

2. Deal with the request's callback data in the <callback function>

   1. `if (httpRequest.readyState === XMLHttpRequest.DONE)`check the request state, whether it has been successfully processed
   2. `if (httpRequest.status === 200)` check the request response whether it successfully connect to the server
   3. `httpRequest.responseText` to parse for response usually in JSON format thus it should be used with `JSON.parse()`

    