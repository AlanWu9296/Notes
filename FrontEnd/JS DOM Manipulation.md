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

## Keycode

#### 字母和数字键的键码值(keyCode)

| 按键 | 键码 | 按键 | 键码 | 按键 | 键码 | 按键 | 键码 |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| A    | 65   | J    | 74   | S    | 83   | 1    | 49   |
| B    | 66   | K    | 75   | T    | 84   | 2    | 50   |
| C    | 67   | L    | 76   | U    | 85   | 3    | 51   |
| D    | 68   | M    | 77   | V    | 86   | 4    | 52   |
| E    | 69   | N    | 78   | W    | 87   | 5    | 53   |
| F    | 70   | O    | 79   | X    | 88   | 6    | 54   |
| G    | 71   | P    | 80   | Y    | 89   | 7    | 55   |
| H    | 72   | Q    | 81   | Z    | 90   | 8    | 56   |
| I    | 73   | R    | 82   | 0    | 48   | 9    | 57   |

#### 数字键盘上的键的键码值(keyCode)

| 按键 | 键码 | 按键  | 键码 |
| ---- | ---- | ----- | ---- |
| 0    | 96   | 8     | 104  |
| 1    | 97   | 9     | 105  |
| 2    | 98   | *     | 106  |
| 3    | 99   | +     | 107  |
| 4    | 100  | Enter | 108  |
| 5    | 101  | -     | 109  |
| 6    | 102  | .     | 110  |
| 7    | 103  | /     | 111  |

#### 功能键键码值(keyCode)

| 按键 | 键码 | 按键 | 键码 |
| ---- | ---- | ---- | ---- |
| F1   | 112  | F7   | 118  |
| F2   | 113  | F8   | 119  |
| F3   | 114  | F9   | 120  |
| F4   | 115  | F10  | 121  |
| F5   | 116  | F11  | 122  |
| F6   | 117  | F12  | 123  |

#### 控制键键码值(keyCode)

| 按键      | 键码 | 按键       | 键码 | 按键        | 键码 | 按键 | 键码 |
| --------- | ---- | ---------- | ---- | ----------- | ---- | ---- | ---- |
| BackSpace | 8    | Esc        | 27   | Right Arrow | 39   | -_   | 189  |
| Tab       | 9    | Spacebar   | 32   | Dw Arrow    | 40   | .>   | 190  |
| Clear     | 12   | Page Up    | 33   | Insert      | 45   | /?   | 191  |
| Enter     | 13   | Page Down  | 34   | Delete      | 46   | `~   | 192  |
| Shift     | 16   | End        | 35   | Num Lock    | 144  | [{   | 219  |
| Control   | 17   | Home       | 36   | ;:          | 186  | \|   | 220  |
| Alt       | 18   | Left Arrow | 37   | =+          | 187  | ]}   | 221  |
| Cape Lock | 20   | Up Arrow   | 38   | ,<          | 188  | '"   | 222  |

#### 多媒体键码值(keyCode)

| 按键   | 键码 |
| ------ | ---- |
| 音量加 | 175  |
| 音量减 | 174  |
| 停止   | 179  |
| 静音   | 173  |
| 浏览器 | 172  |
| 邮件   | 180  |
| 搜索   | 170  |
| 收藏   | 171  |

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

   ```
   0: 请求未初始化
   1: 服务器连接已建立
   2: 请求已接收
   3: 请求处理中
   4: 请求已完成，且响应已就绪
   ```

    ```js
   var xhr = new XMLHttpRequest()
   xhr.open('GET',url)
   xhr.onload = function(){}
   xhr.send()
    ```

   