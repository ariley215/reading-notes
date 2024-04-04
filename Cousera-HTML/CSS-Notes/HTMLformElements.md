# Glossary: HTML Form Elements

```hmtl
<input> 
```

The key attribute of this element is type. Some common values for the type include: button, checkbox, date, email, number, password, submit, text, and url. These values dictate the appearance of the element.

```html
<label>
```

It has an attribute "for", the value of which should be equal to the id attribute of the element it is associated with. Note how in the example above, the <label is associated with the <input using its id value.

**textarea**

*cols *defines the width of the text area, the default value is 20

*form* the form element the text area is associated with

*maxlength* when specified, limits the maximum number of characters that can be entered in the text area

*minlength* the minimum number of characters that the user should enter

*readonly* once set, the user cannot modify the contents

*rows* defines the number of visible text lines for the text area

**fieldset** 

Used to group related input elements in a form. For instance, elements related to the user’s personal information and educational qualification can be grouped separately in two field sets.

**legend** 

Defines a caption for the *fieldset* element.

```html
<fieldset> 
  <legend>Personal Info</legend> 
  <label for="fname">First name:</label><br> 
  <input type="text" id="fname" name="fname" value="John"><br> 
  <label for="lname">Last name:</label><br> 
  <input type="text" id="lname" name="lname" value="Doe"><br> 
</fieldset> 

<fieldset> 
  <legend>Qualificaiton</legend> 
  <label for="pdegree">Primary degree:</label><br> 
  <input type="text" id="pdegree" name="degree" value="Masters"><br> 
  <label for="fos">Last name:</label><br> 
  <input type="text" id="fos" name="field" value="Psychology"><br> 
</fieldset> 
```

**datalist**

Specifies a list of pre-defined options for an input element.

It differs from <select since the user can still provide textual or numeric input other than the listed options.

**option**

Defines an option for the drop-down list. The following code example demonstrates how a simple list can be defined, with the rendered view below the code block.

## Source

[Glossary](https://www.coursera.org/learn/html-and-css-in-depth/supplement/UxG2i/glossary-html-form-elements)