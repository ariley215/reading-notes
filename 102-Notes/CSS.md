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

- Each HTML page must include a reference to the external style sheet file inside the link element, inside the head section.
- An external style sheet can be written in any text editor, and must be saved with a .css extension.

Internal CSS

- Internal styles are defined within the style element, inside the head section of an HTML page:

Inline CSS

- An inline style may be used to apply a unique style for a single element.
- **Tip** An inline style loses many of the advantages of a style sheet (by mixing content with presentation). Use this method sparingly.

## Types of Selectors

1. *Element Selector:* This is the most basic selector and selects all instances of a particular HTML element. For example, p will select all p elements on the page.

2. *ID Selector*: An ID selector selects a single element based on its unique id attribute. To select an element by its ID, you use the # symbol followed by the ID value. For example, #myElement selects an element with id="myElement".

3. *Class Selector*: A class selector selects one or more elements based on the class attribute. To select elements by class, you use a . followed by the class name. For example, .highlight selects all elements with class="highlight".

3. *Descendant Selector*: This selector allows you to select an element that is a descendant of another element. For example, ul li selects all li elements that are descendants of a ul.

4. *Child Selector*: A child selector is similar to the descendant selector, but it selects only direct children of an element. It uses the > symbol. For example, ul > li selects all li elements that are immediate children of a ul.

5. *Adjacent Sibling Selector*: This selector selects an element that is an immediate sibling of another element. It uses the + symbol. For example, h2 + p selects the first p element that comes immediately after an h2.

6. *General Sibling Selector*: The general sibling selector selects all elements that are siblings of another element. It uses the ~ symbol. For example, h2 ~ p selects all p elements that are siblings of an h2.

7. *Attribute Selector*: You can select elements based on their attributes using attribute selectors. For example, type="button" selects all elements with type="button".

8. *Pseudo-class Selector*: Pseudo-classes are used to select elements based on their state or position. Common pseudo-classes include :hover, :active, :nth-child(), and :first-child.

9. *Pseudo-element Selector*: Pseudo-elements allow you to select and style a part of an element, such as the first line or the first letter. Common pseudo-elements include ::before and ::after.

10. *Grouping Selector*: You can group multiple selectors together using a comma. For example, h1, h2, h3 selects all h1, h2, and h3 elements.
