# DOM

![[EMBED/12-BOM-DOM-JS-1.png]]

[[12-BOM-DOM-JS-1.pdf#page=2&rect=117,467,484,731|12-BOM-DOM-JS-1, p.2]]

The DOM Document Object Model (DOM) is a  standard object model (*is an object-oriented representation of the structure and content of a web page*) and programming interface for HTML.  

>“The W3C Document Object Model (DOM) is a platform and language-neutral interface that allows programs and scripts to dynamically access and update the content, structure, and style of a document” 

It defines: 
- The HTML elements as objects 
-  The properties of all HTML elements 
-  The methods to access all HTML elements 
-  The events for all HTML elements  

Every element in a document is part of its DOM, so they can all be accessed and manipulated using language like JavaScript  

JavaScript can: 
- change all the HTML (elements, attributes, styles) in the page 
- add or remove existing HTML elements and attributes 
- react to HTML events in the page 
-  create new HTML events in the page  

Implementations of the DOM can be built for any language But Javascript is the only one that can work client-side  

## Elementi e struttura
Node (Abstract base class)
Every object located within a document is a node. In an HTML document, an object can be a `document`, an `element node` but also a `text node` or `attribute node`

Document (a Node) 
The root document object itself. Document contains a main child node , which is the `<html>` element (*element node*). From there, the entire hierarchy develops.

Element (a Node)
The most general base class from which all element objects (i.e. objects that represent elements) in a Document inherit  

Attr (a Node) 
Attr object represents an HTML attribute

NodeList (has Node(s)) 
A nodeList is an array of nodes. Items in a nodeList are accessed by index:
Node_list_name.item(1) or Node_list_name[1]

NamedNodeMap 
A namedNodeMap is an associative array, where the items are accessed by name. They can also be accessed by index using the item() method (but nodes are in no particular order in the list). You can also add and remove items from a namedNodeMap.  

Text (a Node): 
a text object represents the text inside a HTML element. A text node contains only a string. It may not have childrent and is always a leaf of the tree  

![[EMBED/12-BOM-DOM-JS-1 1.png]]

[[12-BOM-DOM-JS-1.pdf#page=10&rect=118,109,478,381|12-BOM-DOM-JS-1, p.10]]

![[EMBED/12-BOM-DOM-JS-1 2.png]]

[[12-BOM-DOM-JS-1.pdf#page=11&rect=118,110,478,384|12-BOM-DOM-JS-1, p.11]]

Node properties:
- Node.nodeType: Read only 
- Node.nodeName: Read only (An HTMLElement will contain the name of the corresponding tag, like 'audio' for an HTMLAudioElement, a Text node will have the '#text' string, or a Document node will have the '#document' string).
- Node.baseURI : Read only
-  Node.textContent : Read/Write  

![[EMBED/12-BOM-DOM-JS-1 3.png]]

[[12-BOM-DOM-JS-1.pdf#page=13&rect=133,516,469,715|12-BOM-DOM-JS-1, p.13]]

![[EMBED/11-BOM-DOM-JS-2.png]]

[[11-BOM-DOM-JS-2.pdf#page=1&rect=120,223,503,718|11-BOM-DOM-JS-2, p.1]]

 There are several ways traditionally used to find elements:
 -  By id 
 - By tag name 
 - By class name 
 - By name  

Today, to perform element selection is preferable to use:
- `document.querySelector(cssSelector)` It returns (if found) the first element that matches the specified CSS selector. Otherwise null
- `document.querySelectorAll(cssSelector)` It returns (if found) a NodeList of all elements that match the specified CSS selector. Otherwise an empty NodeList  

<hr style="width: 70%; margin-left: auto;margin-right: auto;">
## Eventi
Events are "things" that happen to HTML elements

An HTML event can be something the browser does, or something a user does
Examples: 
- The user selects, clicks, or hovers the cursor over a certain element 
- The user chooses a key on the keyboard 
- ...

When an event happens a signal of some kind is generated. A mechanism by which an action can be automatically taken is provided  

The Event interface represents an event that takes place in the DOM.  There are many types of events, some of which use other interfaces based on the main Event interface. Event itself contains the properties and methods which are common to all events.  

To react to an event, you attach an **event handler** to an element. 
An event handler is a piece JavaScript (*a function*) that runs when a specific event occurs 

We say we are registering an event handler when we define this code. Two ways to play with DOM events : 
1. **Event handler attributes**: you define the handler directly in HTML using attributes (e.g. onclick). The attribute value is the JavaScript code you want to run when the event occurs 
2. **Event listener**: you attach an event listener to an element. The listener waits for the event to happen, and then calls the  handler function  

