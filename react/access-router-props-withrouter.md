# Accessing router props using withRouter 

`withRouter` is a higher order component provided as part of the `react-router`-package. It can be used to wrap components down in a comopnent graph, making location, history and match available without having to do property passing.


```javascript
import React from 'react'
import PropTypes from 'prop-types'
import { withRouter } from 'react-router'

class ShowTheLocation extends Component {
  static propTypes = {
    match: PropTypes.object.isRequired,
    location: PropTypes.object.isRequired,
    history: PropTypes.object.isRequired
  }

  render() {
    const { match, location, history } = this.props

    return (
      <div>You are now at {location.pathname}</div>
    )
  }
}

export default withRouter(ShowTheLocation)

```

###NOTE
Remember to wrap `withRouter` outside `connect` if redux is used. The `connect`-function from react-redux implements the `componentShouldUpdate` lifecycle function in react, which stops re-rendering of components in some cases.

```javascript
// This gets around shouldComponentUpdate
withRouter(connect(...)(MyComponent))
// or
compose(
  withRouter,
  connect(...)
)(MyComponent)

// This does not
connect(...)(withRouter(MyComponent))
// nor
compose(
  connect(...),
  withRouter
)(MyComponent)
```

See [blocked-updates](https://github.com/ReactTraining/react-router/blob/master/packages/react-router/docs/guides/blocked-updates.md) for more details about this issue.