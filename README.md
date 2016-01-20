# Backgrid.js

The codebase for backgrid-dynamic-cell is forked from [wyuenho/backgrid](https://github.com/wyuenho/backgrid).

## Features

The goal of backgrid-dynamic-cell is to allow different types of cell to be rendered in same column; As current backgrid lacks this feature.

## Example

```javascript
var Variable = Backbone.Model.extend({});

var Variables = Backbone.Collection.extend({
  model: Variable
});

var variables = new Variables();

variables.add([
  { "varname": "First name: (Cell to right side is string cell)", "varvalue": "Harshal"},
  { "varname": "Last name: (Cell to right side is string cell)", "varvalue": "Dhumal"},
  { "varname": "Age: (Cell to right side is integer cell)", "varvalue": 26},
  { "varname": "Weight: (Cell to right side is number cell)", "varvalue": 64.5},
]);

// Column definitions
var columns = [{
  name: "varname",
  label: "Variable",
  // The cell type can be a reference of a Backgrid.Cell subclass, any Backgrid.Cell subclass instances like *id* above, or a string
  cell: "string" // This is converted to "StringCell" and a corresponding class in the Backgrid package namespace is looked up
}, {
  name: "varvalue",
  label: "Variable value",
  cell: "string" // This cell type will be used if cellFunction is not provided or cellFunction raises ReferenceError exception.
  cellFunction: function(model){
        /* This callback function will be called before every cell being rendered for this column.
         * This function must return valid cell class i.e. Backgrid.Cell or its subclass.
         * This function will be called with context (this) as column and model as argument.
         */
        var val = model.get(this.get('name'));
         if (isNaN(val)) {
             return "string";  // Backgrid.StringCell
         } else if (val % 1 === 0) {
             return Backgrid.IntegerCell;
         } else if (!isNaN(val)) {
            return Backgrid.NumberCell;
         } else {
            return Backgrid.Cell;
         }
     }
}];

// Initialize a new Grid instance
var grid = new Backgrid.Grid({
  columns: columns,
  collection: variables,
});

// Render the grid and attach the Grid's root to your HTML document
$("#example-1-result").append(grid.render().el);
```
