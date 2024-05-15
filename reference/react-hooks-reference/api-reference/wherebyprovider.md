# WherebyProvider

```jsx
<WherebyProvider />
```

The `WherebyProvider` is necessary so that every component in the app, has access to Whereby's internal state.&#x20;

## Usage

```tsx
import * as React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import App from './App'
import { WherebyProvider } from '@whereby.com/browser-sdk/react'

ReactDOM.render(
  <WherebyProvider>
    <App />
  </WherebyProvider>,
  document.getElementById('root')
)
```
