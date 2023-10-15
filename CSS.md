# CSS

## Cascading Style Sheets

- Can't have without HTML

- Tells the browsers how the content should look.
  - can be used for very basic document text styling
  - can be used to create a layout
  - can be used for effects such as animation.

## CSS Syntax

- CSS is a rule-based language — you define the rules by specifying groups of styles that should be applied to particular elements or groups of elements on your web page.

  - changes heading text color to red 
    - h1 {
  color: red;
  font-size: 5em;
    }

![anatomy of css structure](https://developer.mozilla.org/en-US/docs/Learn/CSS/First_steps/How_CSS_is_structured/declaration.png) 

Differnt ways to insert CSS

External CSS

- Each HTML page must include a reference to the external style sheet file inside the <link> element, inside the head section.
- An external style sheet can be written in any text editor, and must be saved with a .css extension.

Internal CSS

- Internal styles are defined within the <style> element, inside the <head> section of an HTML page:

Inline CSS

- An inline style may be used to apply a unique style for a single element.
- **Tip** An inline style loses many of the advantages of a style sheet (by mixing content with presentation). Use this method sparingly.