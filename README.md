# quill-table-better
A module that enhances the table functionality of [Quill](https://quilljs.com/).

## Demo
[quill-table-better Codepen Demo](https://codepen.io/attoae/pen/WNBGjZp)

## Dependencies
[quill.js](https://quilljs.com/) ">= v2.0.0"

## Quickstart
$\color{red}{notice}$
```html
const delta = quill.clipboard.convert({
  html,
  text: '\n'
})
quill.setContents(delta, Quill.sources.USER);

// The above method causes the table to not display properly
// Please use the following method instead
const [range] = quill.selection.getRange();
quill.updateContents(delta, Quill.sources.USER);
quill.setSelection(
  delta.length() - range.length,
  Quill.sources.SILENT,
);
quill.scrollIntoView();
```

npm
```html
import QuillTableBetter from 'quill-table-better';
import 'quill-table-better/dist/quill-table-better.css'

Quill.register({
  'modules/table-better': QuillTableBetter
}, true);

const options = {
  theme: 'snow',
  modules: {
    table: false,
    'table-better': {},
    keyboard: {
      bindings: QuillTableBetter.keyboardBindings
    }
  }
};
```
cdn
```html
<link href="https://cdn.jsdelivr.net/npm/quill@2/dist/quill.snow.css" rel="stylesheet" />
<link href="https://cdn.jsdelivr.net/npm/quill-table-better@1/dist/quill-table-better.css" rel="stylesheet" />
<script src="https://cdn.jsdelivr.net/npm/quill@2/dist/quill.js"></script>
<script src="https://cdn.jsdelivr.net/npm/quill-table-better@1/dist/quill-table-better.js"></script>

<div id="root"></div>
<script>
  Quill.register({
    'modules/table-better': QuillTableBetter
  }, true);

  const toolbarOptions = [
    ['bold', 'italic', 'underline', 'strike'],
    ['table-better']
  ];

  const options = {
    theme: 'snow',
    modules: {
      table: false,
      toolbar: toolbarOptions,
      'table-better': {
        toolbarTable: true
      },
      keyboard: {
        bindings: QuillTableBetter.keyboardBindings
      }
    }
  };
  const quill = new Quill('#root', options);
</script>
```

## Conifg

### language
The 'language' parameter has two types:
1. string, 'en_US' or 'zh_CN', default 'en_US'
2. Used to register a new language, such as:
  { name: 'en_UK', content: [en_US](https://github.com/attoae/quill-table-better/blob/develop/src/language/en_US.ts) } (For content, please refer to [en_US](https://github.com/attoae/quill-table-better/blob/develop/src/language/en_US.ts))

### menus
'menus' are used to configure the action bar, and those not in the array are not displayed.
Empty array or no configuration default all display.

The functions of the operation bar are as follows:
1. column
  - Insert column left
  - Insert column right
  - Delete column
2. row
  - Insert row above
  - Insert row below
  - Delete row
3. merge
  - Merge cells
  - Split cell
4. table
  - Table properties
5. cell
  - Cell properties
6. wrap (Insert paragraph outside the table)
  - Insert before
  - Insert after

### toolbarTable
'toolbarTable' is used to add a button to insert a table on the toolbar (true or false).
And 'table-better' needs to be added to toolbarOptions, for example: 
```html
const toolbarOptions = [
  ['bold', 'italic', 'underline', 'strike'],
  ['table-better']
];
```

```html
'table-better': {
  language: 'en_US',
  menus: ['column', 'row', 'merge', 'table', 'cell', 'wrap'],
  toolbarTable: true
}
```

## Formats supported by table
The table supports the following formats and supports simultaneous operations on multiple cells：
```html
const WHITE_LIST = [
  'bold',
  'italic',
  'underline',
  'strike',
  'size',
  'color',
  'background',
  'font',
  'list',
  'header',
  'align'
];
```

## Methods
```html
 const module = quill.getModule('table-better');
```
### deleteTable
```
module.deleteTable();
```
### deleteTableTemporary、hideTools
When you need to submit data(html or delta) to the server, you should use this function,for example：
```html
// Delta
module.hideTools();
const delta = quill.getContents();
axios.post(url, delta);
```

```html
// HTML
module.deleteTableTemporary();
const html = quill.getSemanticHTML();
axios.post(url, html);
```
### getTable(range = this.quill.getSelection())
Function return '[table, row, cell, offset]'
```html
module.getTable();
```
### insertTable(rows: number, columns: number)
```html
module.insertTable(3, 3);
```

## Download
```html
npm i quill-table-better
```

### CDN
```html
<link
  href="https://cdn.jsdelivr.net/npm/quill-table-better@1/dist/quill-table-better.css"
  rel="stylesheet"
/>
<script src="https://cdn.jsdelivr.net/npm/quill-table-better@1/dist/quill-table-better.js"></script>
```