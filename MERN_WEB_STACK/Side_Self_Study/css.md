## Cascading Style Sheets (CSS)
### Introduction:
is a stylesheet language used to describe the presentation of a document written in HTML or XML (including XML dialects such as SVG, MathML, or XHTML). CSS describes how elements should be rendered on screen, on paper, in speech, or on other media.

## Inline CSS, internal or embedded CSS, and external CSS.
You might be wondering how this CSS code is applied to HTML content, though. Much like HTML, Cascading Style Sheets are written in simple, plain text through a text editor or word processor on your computer, and there are three main ways to add that CSS code to your HTML-styled pages. CSS code (or a style sheet) can be external, internal, or inline.

**External** style sheets are saved as a CSS file (.css) and can be used to determine the appearance of an entire website through one file (rather than adding individual instances of CSS code to every HTML element you want to adjust). To use this type of style sheet, your .html files need to include a header section that links to the external style sheet and looks something like this:
```
<head>
<link rel=”stylesheet”  type=”text/css”  href=mysitestyle.css”>
</head>
```
This will link the .html file to your style sheet (in this case, mysitestyle.css), and all of the CSS instructions in that file will then apply to your linked .html pages.

**Internal** style sheets are CSS instructions written directly into the header of a specific .html page. (This is especially useful if you have a single page on a site that has a unique look.) An internal style sheet looks something like this. . .
```
<head>
<style>
body {  background-color: thistle;  }
p {  font-size:20px;  color: mediumblue;  }
</style>
</head>
```

Finally, **inline** styles are snippets of CSS written directly into HTML code, and applicable only to a single coding instance, without an extra CSS file. For example:

```<h1  style=”font-size:40px;color:violet;”>Check out this headline!</h1>```

### CSS Rule

A CSS rule consists of a selector and a declaration block.

The selector points to the HTML element you want to style.

The declaration block contains one or more declarations separated by semicolons.

Each declaration includes a CSS property name and a value, separated by a colon.

Multiple CSS declarations are separated with semicolons, and declaration blocks are surrounded by curly braces.

Example of CSS:
```
p {
  color: red;
  text-align: center;
}
```
Example Explained
- ```p``` is a selector in CSS (it points to the HTML element you want to style: <p>).
- ```color``` is a property, and ```red``` is the property value
- ```text-align``` is a property, and ```center``` is the property value

## Useful CSS Properties

Again, we certainly won't be covering every interesting CSS property here, but can introduce a few (with links to the CSS reference for each).

```text-align```

As we can seen, text-align can be used to change the text justification. Possible values are left, right, center, and justify (for full justification with to ragged left or right margin).

```font-style```

Used to control italic text: possible values include italic and normal (for non-italics).

```font-weight```

Used to control bold text: possible values include bold and normal (for non-bold).

```color```

Used to set the colour (usually of the text) for the element. For example, “color: green;”. We will discuss colour values more in Colours in CSS.

```background-color```

Used to set the background colour (behind the text) for the element. For example, **“background-color: black;”**.

```border-width```

```border-style```

```border-color```

These are used to control the border around an element (width of the line, type of line, and color of line, respectively). You can either use these properties separately or use a shorthand property border to combine these into one line (giving the three values in any order). These two things are equivalent:
```
figure {
  border-style: solid;
  border-width: 3px;
  border-color: red;
}
figure {
  border: solid red 3px;
}
```
```line-height```

Controls the amount of vertical space each line of text takes up (which you might know as “leading”). For example, **“line-height: 1.5;”** sets the spacing between single- and double-spaced (and is probably a good default value.

```font-family```

Sets the font for the text. You should give a list of fonts that are tried in order until the browser finds one available on the user's system. There are five generic font families and your list must end with one of them since it's guaranteed to work. For example, **“font-family: "Garamond", "Times", serif;”**.
