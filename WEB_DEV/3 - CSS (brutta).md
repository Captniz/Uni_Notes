---
Date created: 02-09-26 • 15:40
tags:
  - Web
Related PDF/DOC:
  - "[[03-CSS.pdf]]"
  - "[[03-CSS_2.pdf]]"
Related Pages:
---
```html
<link rel="stylesheet" type="text/css“ href="mystyle.css">
```  

There are two types of lengths used in CCS: absolute and relative  
Absolute units are considered to always be the same size  

Relative length units are relative to something else. The benefit in using them is that we can make it so the size of something scales relative to everything else on the page  

---
Combinators select elements exploiting the specific relationship between them  
There are five different combinators in CSS: 
- Simple + class (.) 
- descendant selector (space) 
- child selector (>)
-  adjacent sibling selector (+)
-  general sibling selector (~)  

---

Pseudo-class selectors are used to define a special state of an element: mouse over, link is visited or not and so on 

![[EMBED/03-CSS.png]]

[[03-CSS.pdf#page=20&rect=119,465,482,670|03-CSS, p.20]]

Pseudo-element selectors are used to style specified parts of an element: the first letter, the first line, and so on  

---
Cascading algorithm  
The steps that apply to the cascading order: 
- Selection of rules
-  Origin and importance
-  Specificity
-  Order of appearence  

Origin and importance:  
![[EMBED/03-CSS 2.png]]

[[03-CSS.pdf#page=22&rect=169,463,422,573|03-CSS, p.22]]

Specificity:
 In case of equality with an origin  The declaration with the highest specificity wins.  

Order of decreasing specificity:  
- Id selectors
- Class selectors / Attribute selectors / Pseudo-class selectors 
- Element selectors 
- Universal selector DOES NOT impact the specificity 
- Combinator selectors +, >, ~ DO NOT impact the specificity  

Inline styles (inline CSS) always overwrite any normal style in author stylesheets.

Order of appereance:   
if there are competing values  of equal specificity, the last declaration in the style order is applied.


----
Positioning  

The position property is used to set position for an element.  

There are several position values:  
- **Static**    : Default, according to page flow
- **Relative**  : Setting the top, right, bottom, and left properties of a relatively-positioned element will cause it to be adjusted away from its static position  
- **Absolute**  : [...] will cause it to be adjusted in an absolute position with respect to its *nearest positioned ancestor*  

