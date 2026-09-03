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

