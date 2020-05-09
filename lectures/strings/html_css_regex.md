---
title: "html, css, regex"
author: 
date: 
urlcolor: blue
output: 
  html_document:
    toc: true
    toc_depth: 2
    toc_float: true # toc_float option to float the table of contents to the left of the main document content. floating table of contents will always be visible even when the document is scrolled
      #collapsed: false # collapsed (defaults to TRUE) controls whether the TOC appears with only the top-level (e.g., H2) headers. If collapsed initially, the TOC is automatically expanded inline when necessary
      #smooth_scroll: true # smooth_scroll (defaults to TRUE) controls whether page scrolls are animated when TOC items are navigated to via mouse clicks
    number_sections: true
    fig_caption: true # ? this option doesn't seem to be working for figure inserted below outside of r code chunk    
    highlight: tango # Supported styles include "default", "tango", "pygments", "kate", "monochrome", "espresso", "zenburn", and "haddock" (specify null to prevent syntax    
    theme: default # theme specifies the Bootstrap theme to use for the page. Valid themes include default, cerulean, journal, flatly, readable, spacelab, united, cosmo, lumen, paper, sandstone, simplex, and yeti.
    df_print: tibble #options: default, tibble, paged
    keep_md: true # may be helpful for storing on github
    
---


# appendix

appendix


## HTML and CSS

__Markup language__

> "A markup language is a computer language that uses tags to define elements within a document. It is human-readable, meaning markup files contain standard words, rather than typical programming syntax." [Markup language](https://techterms.com/definition/markup_language)

__Hypertext Markup Language (HTML)__

- HTML is a markup language for the creation of websites
- HTML puts the content on the webpage, but does not "style" the page (e.g., fonts, colors, background)

__Cascading style sheets (CSS)__

> "Cascading style sheets are used to format the layout of Web pages. They can be used to define text styles, table sizes, and other aspects of Web pages" [CSS](https://techterms.com/definition/css)

### Hypertext markup language (HTML)

Hypertext markup language (HTML) puts the content on the webpage, but does not "style" the page (e.g., fonts, colors, background)

<br>

1-minute overview of HTML [NOTES FROM YOUTUBE VIDEO CUT OR CLEAN UP]

```r
overview
HTML puts the content on the page, but does not style the page
CSS = cascading style sheets, which add style (e.g., fonts, colors, etc.) to the content
Javascript : to add Functionality 
Page where you can see what html code looks like when run
https://www.w3schools.com/html/tryit.asp?filename=tryhtml_default 
*all* of HTML consists of the following things:
Tags
Tagnames
attributes
Indentation and whitespace in your html script
Both are about making script readable for programmer
Html doesn’t care about white space
Html recognizes the first space in between words but not 2nd through 1 million-th space
Indenting is to make the script more readable; they don’t do anything


Tags 
Everything in html is considered a “tag”
Anything between opening tag and closing tag is considered inside of a tag
Can have tags inside of tags inside of tags inside of “body”
Tag can have one or more attributes
Attributes come after the tag name
Tag is what the thing is; attributes describe things about that tag
E.g., 
Example of tag called “frank”
<frank height=”300” intelligence=”high” /
Tag names
<html>
Must have <html> tag
<head> tag [goes inside html tag
<body tag>
Example of these three tag names in action
<!DOCTYPE html>
<html>
<head></head>
<body>


</body>
</html>
Note that <head> and <body> are inside the <html> tag
<title> tag would go in the “head”
```

Lots of wonderful resources on the web to learn HTML!

- Use this website to create/modify html code and view the result after it is compiled
    - [TryIt Editor](https://www.w3schools.com/html/tryit.asp?filename=tryhtml_default)
- Wath this __excellent__ 12-minute introductory HTML tutorial by LearnCode.academy
  - Link: [“HTML Tutorial for beginners”](https://www.youtube.com/watch?v=RjHflb-QgVc)
  - URL: https://www.youtube.com/watch?v=RjHflb-QgVc
- Can put hyperlinks within HTML using the `<a>` tag
  - Syntax: `<a href="url">text you want for link</a>`
  - Example: `<a href="https://www.w3schools.com/html/">Visit our HTML tutorial</a>`
  - [See](https://www.w3schools.com/html/html_links.asp)
- Html cheat sheets [CRYSTAL/PATRICIA - CHANGE IF YOU KNOW OF BETTER CHEAT SHEETS. I JUST GRABBED FIRST ONES]
  - [Link to HTML cheat sheet (PDF)](https://web.stanford.edu/group/csp/cs21/htmlcheatsheet.pdf)
  - [Link to another HTML cheat sheet (jpg)](https://i.pinimg.com/originals/90/d2/64/90d26403328832df18ebd8b5f47f08fa.jpg), which is also shown in image below

[![](https://i.pinimg.com/originals/90/d2/64/90d26403328832df18ebd8b5f47f08fa.jpg)](https://www.pinterest.com/pin/191332684147746741/)


Paste the below code into [TryIt Editor](https://www.w3schools.com/html/tryit.asp?filename=tryhtml_default) and click __Run__

```r
<!DOCTYPE html>
<html>
<head>
  <title>Page title (in head tag)</title>
</head>
<body>

  <h1>Title of level 1 heading</h1>
  
  <p>My first paragraph.</p>
  <p>My second paragraph.</p>
  <p>Add some bold text <strong>right here</strong></p>
  <p>Add some italics text <em>right here</em></p>
  

  <p>Include a hyperlink tag within a paragraph tag. this book looks interesting : <a href="https://bookdown.org/rdpeng/rprogdatascience/">R Programming for Data Science</a></p>  
  
  <p>Include another hyperlink tag within a paragraph tag. chapter on <a href="https://bookdown.org/rdpeng/rprogdatascience/regular-expressions.html">Regular Expressions</a></p>    
  <p> put a button inside this paragraph <button>I am a button!</button></p>
  
  <p>Here are some items in a list, but items not placed within an unordered list </p>
  
  <li> text you want in item</li>
  <li> text you want in another item</li>
  
  <p>Here are some items in an unordered list</p>
  
  <ul>
  <li> first item in unordered list </li>
  <li> second item in unordered list </li>
  </ul>

</body>
  
</html>
```

Student exercise

- Spend 5-10 minutes playing with a simple HTML document on the [TryIt Editor](https://www.w3schools.com/html/tryit.asp?filename=tryhtml_default)



### Cascading style sheets (CSS)

> "Cascading style sheets are used to format the layout of Web pages. They can be used to define text styles, table sizes, and other aspects of Web pages" [CSS](https://techterms.com/definition/css)

1-minute overview of CSS   [NOTES FROM YOUTUBE VIDEO CUT OR CLEAN UP]

- Two basic ways to add style to HTML tags
  - Can use a “style” tag
  - Add CSS style directly in script that contains html code
  - Can use “link” tag
  - Link tag allows you to move CSS out of html into its own file
  - Programmers usually  use this appraoch because allows you to create separate CSS scripts of styles and then apply them to 
- Style tag
  - <style> contents of style tag here </style>
  - Anything inside open and closing style tag is not html
- A __CSS rule__ consists of three things
  - A __selector__
    - A selector defines Which html tag(s) to add style to
    - Within a single selector can change multiple properties 
  - A __property__
    - Which style element to change (e.g., color, opacity, background)
  - A __value))
  - Value to apply to style element (e.g., color: red;)
- Looks like this:


```r
selector {


	property: value;
	property: value; 
}
Selector
Choose which tag you would like to apply style to
E.g., add style to <h1>
h1 {


	property: value;
	property: value; 
}
Property
E.g., “color” is a property and “red” is a value
```



Lots of resources to learn CSS on the web

- Wath this 7-minute introductory CSS tutorial by LearnCode.academy
  - Link: [“HTML CSS Tutorial for Beginners”](https://www.youtube.com/watch?v=J35jug1uHzE&list=PLoYCgNOIyGABDU532eesybur5HPBVfC1G&index=4&t=10s)
  - URL: https://www.youtube.com/watch?v=J35jug1uHzE&list=PLoYCgNOIyGABDU532eesybur5HPBVfC1G&index=4&t=10s
- CSS cheat sheets [CRYSTAL/PATRICIA - CHANGE IF YOU KNOW OF BETTER CHEAT SHEETS. I JUST GRABBED FIRST ONES]
  - [Link CSS cheat sheet (jpg)](https://i.pinimg.com/originals/90/d2/64/90d26403328832df18ebd8b5f47f08fa.jpg), which is also shown in image below

[![](https://i.pinimg.com/474x/9c/24/6e/9c246e9c9c4fd7224e93d2fdc2453722--custom-computers-design-web.jpg)](https://www.pinterest.com/pin/218495019393515946/)  
  
Paste the below code into [TryIt Editor](https://www.w3schools.com/html/tryit.asp?filename=tryhtml_default) and click __Run__

```r
<!DOCTYPE html>
<html>
<head>
  <title>Page title (in head tag)</title>
  
<style>

  h1 {
  	color: red;
  	background: rgb(58,58, 58)
  }
  p {
  	color: green;
  }
  body {
  	Background: rgb(60, 60, 60);
  	color: white;
  	font-family: Helvetica Neue, Helvetica, Arial, sans-serif
  }
  button {
  
  	background: gray;
  	border: none;
  	color: white;
  }
</style>
  
</head>
<body>

  <h1>Title of level 1 heading</h1>
  
  <p>My first paragraph.</p>
  <p>My second paragraph.</p>
  <p>Add some bold text <strong>right here</strong></p>
  <p>Add some italics text <em>right here</em></p>
  

  <p>Include a hyperlink tag within a paragraph tag. this book looks interesting : <a href="https://bookdown.org/rdpeng/rprogdatascience/">R Programming for Data Science</a></p>  
  
  <p>Include another hyperlink tag within a paragraph tag. chapter on <a href="https://bookdown.org/rdpeng/rprogdatascience/regular-expressions.html">Regular Expressions</a></p>    
  <p> put a button inside this paragraph <button>I am a button!</button></p>
  
  <p>Here are some items in a list, but items not placed within an unordered list </p>
  
  <li> text you want in item</li>
  <li> text you want in another item</li>
  
  <p>Here are some items in an unordered list</p>
  
  <ul>
  <li> first item in unordered list </li>
  <li> second item in unordered list </li>
  </ul>

</body>
  
</html>
```
