# Objects in Powershell

Powershell is OOP

The pipeline '|'

- Send the output from one command into a second command.
  - output of the PowerShell command an object
    - Objects allow you to access output.

Get‑Member requires the pipeline to work

- Finds object properties.

**Formatting** - to manipulate the data object in the pipeline to your reporting needs
**Variable**- a key component for storing data objects in PowerShell.

Get‑Service - displays the Status as a Service, the service name, and also the display name.

*where‑object* is not pulling out just singular properties like *Select‑Object* is. *Where‑object* is taking a look at each of the objects, it's comparing it against the criteria, which was looking for a status equal to stop, and grabbing all of the data objects where that was true. So it's still leaving that data intact, so we can still work with all of these properties through the pipeline.

().count

- dot notate commands in parentheses
- Any property that's in that pipeline can be access through that parentheses. 

## Variables

- justlike in other OOP
- Great for efficiency
- Information container for reusability
  - any data type
- '$' + 'unique name'
- take a variable in the pipeline

'get-varible | more'

- shows all the varibles being used in the current session.

if you want the value to appear use double quotes
if you want the variable to appear use single quotes

## Formatting

Use 'Format-List' to have the output displayed as a list.
Use 'Format-Table' to have the output displayed in a table.

- has parameters to change the change the type of table display.