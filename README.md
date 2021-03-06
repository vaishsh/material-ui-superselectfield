#material-ui-superSelectField

## Table of Contents
- [Preview](#preview)
- [Installation](#installation)
- [Properties](#properties)
- [Usage examples](#usage)
- [Demo](#demo)
- [Tests](#tests)
- [Contributing](#contributing)

##Preview
![](https://github.com/Sharlaan/material-ui-superSelectField/blob/master/dataSource.png)
![](https://github.com/Sharlaan/material-ui-superSelectField/blob/master/caseInsensitive.png)
![](https://github.com/Sharlaan/material-ui-superSelectField/blob/master/chips.png)


##Installation
`npm i material-ui-superselectfield`  

This component requires 4 dependencies :
- react
- react-dom
- react-tap-event-plugin 
- material-ui  

... so make sure they are installed in your project, or install them as well ;)

##Properties
| Name             | Type          | Default    | Description |
|:----             |:----          |:----       |:---- |
| style | object | {} | Insert your own inlined styles, applied to the root component. |
| menuProps | object | {} | Styles to be applied to the inner Menu component |
| children | any |  | Datasource is an array of any type of nodes, styled at your convenience.<br>/!\ IMPORTANT: each node must expose a `value` property (required). This value property will be used by default for both option's value and label.<br>A `label` property can be provided to specify a different value than value |
| value | string, string[], object, object[] | | Selected values |
| autocompleteFilter | function | see below | Provide your own filtering parser. Default: case insensitive. The search field will appear only if there are more than 10 children. |
| selectionsRenderer | function | see below | Provide your own renderer for selected options. Defaults to concatenating children's values text. Check CodeExample1 for a more advanced renderer example. |
| name | string | | Required to differentiate between multiple instances of superSelectField in same page. |
| hintText | string | 'Click me' | Placeholder text |
| multiple | bool | false | Include this property to turn superSelectField into a multi-selection dropdown. Checkboxes will appear.|
| onSelect | function | | Triggers when selecting/unselecting an option from the Menu.<br>signature: (selectedValues, name) with `selectedValues` array of selected values based on selected nodes' value property, and `name` the value of the superSelectField instance's name property |

####Default functions
| Name | Default function |
|:---- |:---- |
| autocompleteFilter | ```(searchText, text) => !text || text.toLowerCase().includes(searchText.toLowerCase())``` | `autocomplete` must be set to true. Provide your own filtering parser. Default: case insensitive. |
| selectionsRenderer | <span>(value, hintText) => value.length<br>? typeof value === 'string' ? value : value.join(', ')<br>: hintText</span> | Provide your own renderer for selected options. Defaults to concatenating children's values text. Check CodeExample1 for a more advanced renderer example. |


## Usage
#####Selection handler
```
handleSelection = (values, name) => this.setState({ [name]: values })
```
#####Basic example
```
<SuperSelectField
  name='basicExample'
  hintText='Single value'
  multiple // Add this property if need multiselection
  value={this.state.basicExample}
  onSelect={this.handleSelection}
>
  <div value='A'>Option A</div>
  <div value='B'>Option B</div>
  <div value='C'>Option C</div>
</SuperSelectField>
```
#####Complex example
```
handleCustomDisplaySelections = (name) => (values) => {
  return values.length
    ? <div style={{display: 'flex', flexWrap: 'wrap'}}>{values.map((obj, index) => (
        <Chip key={index} style={{margin: 5}} onRequestDelete={this.onRequestDelete(index, name)}>
          <Avatar icon={(
            <FontIcon className={`iconURLclass`} style={customStyle} />)}
          />
          {obj.text}
        </Chip>))}
      </div>
    : 'select some values'
}

onRequestDelete = (key, name) => (event) => {
  this.setState({ [name]: this.state[name].filter((v, i) => i !== key) })
}

render () {
  const dataSource = yourDataArray.map((obj, index) => (
    <div key={index} value={obj.value} label={obj.label} style={customStyle}>
      // use whatever html/css you want
    </div>
  ))

  return (
    <SuperSelectField
      name='complexExample'
      multiple
      hintText='Type some letters ...'
      onSelect={this.handleSelection}
      value={this.state.complexExample}
      selectionsRenderer={this.handleCustomDisplaySelections('complexExample')}
      menuProps={{maxHeight: 370}}
      style={{ width: 300 }}
    >
      {dataSource}
    </SuperSelectField>
  )
}
```

##Demo

Live demo [here](https://sharlaan.github.io/material-ui-superselectfield/#/example1). Or you can build it yourself :
```
git clone https://github.com/Sharlaan/material-ui-superselectfield.git

npm i

npm start
```
It should open a new page on your default browser @ localhost:3000
Then choose "Example 1" to check the superSelectField in action


## Tests


## Contributing
In lieu of a formal style guide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code.
