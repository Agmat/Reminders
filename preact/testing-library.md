# Testing a preact application with `@testing-library` and Typescript

I had a hard time setting up my tests in my first `preact` project with a custom `webpack.config`.

So I decided to sum up important parts here.

## Setup

Install dependencies

``` bash
npm install --save-dev @testing-library/jest-dom @testing-library/preact
npm install --save-dev ts-jest jest @types/jest
npm install --save-dev identity-obj-proxy
```

Add a jest config:

``` js
module.exports = {
    preset: 'ts-jest',
    moduleNameMapper: {
        '^react$': 'preact/compat',
        '^react-dom$': 'preact/compat',
        '^.+\\.(css|scss)$': 'identity-obj-proxy',
    },
};
```

Create `setupTests.ts` in your `/src` with: 

``` ts
// jest-dom adds custom jest matchers for asserting on DOM nodes.
// allows you to do things like:
// expect(element).toHaveTextContent(/react/i)
// learn more: https://github.com/testing-library/jest-dom
import '@testing-library/jest-dom/extend-expect';
```

## Enjoy

Now you can start testing your components as the exemple bellow (from [@testing-library](https://testing-library.com/docs/preact-testing-library/example))

### Component

``` tsx
function HiddenMessage({ children }) {
  const [showMessage, setShowMessage] = useState(false)

  return (
    <div>
      <label htmlFor="toggle">Show Message</label>
      <input
        id="toggle"
        type="checkbox"
        onChange={e => setShowMessage(e.target.checked)}
        checked={showMessage}
      />
      {showMessage ? children : null}
    </div>
  )
}
```

### Test 


``` tsx 
// NOTE: jest-dom adds handy assertions to Jest and it is recommended, but not required.
import '@testing-library/jest-dom/extend-expect'

import { h } from 'preact'
import { render, fireEvent } from '@testing-library/preact'

import HiddenMessage from '../hidden-message'

test('shows the children when the checkbox is checked', () => {
  const testMessage = 'Test Message'

  const { queryByText, getByLabelText, getByText } = render(
    <HiddenMessage>{testMessage}</HiddenMessage>
  )

  // query* functions will return the element or null if it cannot be found.
  // get* functions will return the element or throw an error if it cannot be found.
  expect(queryByText(testMessage)).toBeNull()

  // The queries can accept a regex to make your selectors more resilient to content tweaks and changes.
  fireEvent.click(getByLabelText(/show/i))

  // .toBeInTheDocument() is an assertion that comes from jest-dom.
  // Otherwise you could use .toBeDefined().
  expect(getByText(testMessage)).toBeInTheDocument()
})
```