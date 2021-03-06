### `.fillIn(propValue, eventTargetValue[, options]) => Page`

Fill in a text input

**Default Checked Props:** `id`, `name`, and `placeholder`

#### Arguments
The `propValue` and `options` arguments are passed to
[`.findWrapperForFillIn`][find-wrapper-method] to find a
[`ReactWrapper`][react-wrapper]. The [`ReactWrapper`][react-wrapper] will be
used to simulate events.

1. `propValue` (`String`): Value is compared with the values of the checked
   props to assert a match.
2. `eventTargetValue` (`String`): Value which will equal
   `event.target.value` in `onChange` event handlers triggered by
   the simulated `change` event.
3. `options` (`Object`): Optional.
  * `propToCheck` (`String`): Name of prop to check against instead of the default checked props.

#### Returns

`Page` object which invoked the method. This allow the method to be chained
with another `Page` object method.

#### Simulated Events
If a [`ReactWrapper`][react-wrapper] is found by
[`.findWrapperForFillIn`][find-wrapper-method], then the following events will
be simulated on the [`ReactWrapper`][react-wrapper]'s React element:

1. `blur` event on the React element which is focused. This will occur if there
   is a focused React element and it is not the same as the
   [`ReactWrapper`][react-wrapper]'s React element.
2. `focus` event on the [`ReactWrapper`][react-wrapper]'s React element unless
   it is already in focus.
3. `change` event on the [`ReactWrapper`][react-wrapper]'s React
   element. For `onChange` event handlers triggered by this
   simulated `change` event, `event.target.value` will equal
   `eventTargetValue`.

If no [`ReactWrapper`][react-wrapper] is found, then an error is thrown.

#### Related Methods

- [`.findWrapperForFillIn(propValue, eventTargetValue[, options]) => ReactWrapper`][find-wrapper-method]

[react-wrapper]: https://github.com/airbnb/enzyme/blob/master/docs/api/mount.md#reactwrapper-api
[find-wrapper-method]: findWrapperForFillIn.md

#### Example in Jest

```js
import React, { Component } from 'react'
import Page from 'react-page-object'

class App extends Component {
  state = { text: '' }

  onChange = event => this.setState({ text: event.target.value })

  render() {
    return (
      <div>
        {this.state.text}
        <input id="input-id" onChange={this.onChange} />
        <input name="input-name" onChange={this.onChange} />
        <input placeholder="input-placeholder" onChange={this.onChange} />
        <input className="input-class" onChange={this.onChange} />
      </div>
    )
  }
}

describe('fillIn', () => {
  let page

  beforeEach(() => {
    page = new Page(<App />)
  })

  afterEach(() => {
    page.destroy()
  })

  it('fills in the input - targeting id', () => {
    page.fillIn('input-id', 'hello')
    expect(page.content()).toMatch(/hello/)
  })

  it('fills in the input - targeting name', () => {
    page.fillIn('input-name', 'hello')
    expect(page.content()).toMatch(/hello/)
  })

  it('fills in the input - targeting placeholder', () => {
    page.fillIn('input-placeholder', 'hello')
    expect(page.content()).toMatch(/hello/)
  })

  it('fills in the input - targeting non-default prop', () => {
    page.fillIn('input-class', 'hello', { propToCheck: 'className' })
    expect(page.content()).toMatch(/hello/)
  })
})
```
