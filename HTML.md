## HTML

HTML is a markup language and makes use of various **tags** to format the content. These tags are enclosed within angle braces `<Tag Name>`.

An **HTML element** is defined by a starting tag.

`Line Break Tag`

Whenever you use the `<br />` element, anything following it starts from the next line. This tag is an example of an **empty** element, where you do not need opening and closing tags, as there is nothing to go in between them.

`Division tag`

The div tag is known as Division tag. The div tag is used in HTML to make divisions of content in the web page like (text, images, header, footer, navigation bar, etc). Div tag has both open (<div>) and closing (</div>) tag and it is mandatory to close the tag.

HTML comments are placed in between **<!-- ... -->** tags.

`Preserve Formatting`

Sometimes, you want your text to follow the exact format of how it is written in the HTML document. In these cases, you can use the preformatted tag `<pre>`.

```html
<!DOCTYPE html>
<html>

   <head>
      <title>Preserve Formatting Example</title>
   </head>
	
   <body>
      <pre>
         function testFunction( strText ){
            alert (strText)
         }
      </pre>
   </body>
	
</html>
```

An **attribute** is used to define the characteristics of an HTML element and is placed inside the element's opening tag. All attributes are made up of two parts − a **name** and a **value**

For example, the paragraph **<p>** element in the example carries an attribute whose name is **align**, which you can use to indicate the alignment of paragraph on the page.

```html
<body> 
  <p align = "left">This is left aligned</p> 
  <p align = "center">This is center aligned</p> 
  <p align = "right">This is right aligned</p> 
</body>
```

`Core Attributes`

The four core attributes that can be used on the majority of HTML elements (although not all) are −

- Id
- Title
- Class
- Style

The **id** attribute of an HTML tag can be used to uniquely identify any element within an HTML page.

The **title** attribute specifies extra information about an element. The information is most often shown as a tooltip text when the mouse moves over the element.

The **class** attribute is used to associate an element with a style sheet, and specifies the class of element.

`Insert Image`

You can insert any image in your web page by using **<img>** tag.

The **alt** attribute is a mandatory attribute which specifies an alternate text for an image, if the image cannot be displayed.

```html
<img src = "/html/images/test.png" alt = "Test Image" width = "150" height = "100"/>
```

`Insert Tables`

The HTML tables are created using the **<table>** tag in which the **<tr>** tag is used to create table rows and **<td>** tag is used to create data cells. The elements under <td> are regular and left aligned by default

```html
<table border = "1">
  <tr>
    <td>Row 1, Column 1</td>
    <td>Row 1, Column 2</td>
  </tr>

  <tr>
    <td>Row 2, Column 1</td>
    <td>Row 2, Column 2</td>
  </tr>
</table>
```

Here, the **border** is an attribute of <table> tag and it is used to put a border across all the cells. If you do not need a border, then you can use border = "0".

`Linking Documents`

A link is specified using HTML tag <a>. This tag is called **anchor tag** and anything between the opening <a> tag and the closing </a> tag becomes part of the link and a user can click that part to reach to the linked document.

```html
<a href = "Document URL" ... attributes-list>Link Text</a> 
```