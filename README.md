import {observer} from "mobx-react";
import mobx from "mobx";
import hoistNonReactStatic from "hoist-non-react-statics";

function getDisplayName(WrappedComponent) {
  return WrappedComponent.displayName || WrappedComponent.name || 'Component';
}
// This function takes a component...
function InterceptComponent(WrappedComponent, interceptor) {
  // ...and returns another component...
  class Interceptor extends React.Component {
    constructor(props) {
      super(props);
    }

    render() {
      // ... and renders the wrapped component with the fresh data!
      // Notice that we pass through any additional props
      return <WrappedComponent {...this.props} />;
    }
  };
}

/* Component Life Cycle Stages:
    construction,
    default props,
    initial state,
    componentWillMount
    static getDerivedStateFromProps(props, state)
    render
    componentDidMount
*/
class BaseComponent extends React.Component {
  constructor(props) {
    super(props)
  }

  /* componentWillMount */
  /* is a chance for us to handle configuration, update
  our state, and in general prepare for the first render. At this point, props
  and initial state are defined. We can safely query this.props and this.state,
  knowing with certainty they are the current values. This means we can start
  performing calculations or processes based on the prop values. */
  componentWillMount() {}

  /****************************************************************************
  getDerivedStateFromProps:
    * Enables a component to update its internal state as the result of changes in props.
    * invoked right before calling the render method, both on the initial mount and on subsequent updates. It should return an object to update the state, or null to update nothing.
    * This method doesn’t have access to the component instance. If you’d like, you can reuse some code between getDerivedStateFromProps() and the other class methods by extracting pure functions of the component props and state outside the class definition.
    * NOTE that this method is fired on every render, regardless of the cause. This is in contrast to UNSAFE_componentWillReceiveProps, which only fires when the parent causes a re-render and not as a result of a local setState.
    * ANTI-PATTERNS: https://reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html
    *
  *****************************************************************************/
  static getDerivedStateFromProps(props, state)


  /*
  componentDidMount:
  Similar to componentWillMount(), componentDidMount() is only called one
  time. Unlike our other Birth/Mount methods, where we start at the top and
  work down, componentDidMount() works from the bottom up. Let's consider the
  following Component/Element Tree again:
  */
  componentDidMount() {}

  /* getSnapshotBeforeUpdate() is invoked right before the most recently rendered
  output is committed to e.g. the DOM. It enables your component to capture
  some information from the DOM (e.g. scroll position) before it is
  potentially changed. Any value returned by this lifecycle will be passed as
  a parameter to componentDidUpdate(). This use case is not common, but it may
  occur in UIs like a chat thread that need to handle scroll position in a special way.
  A snapshot value (or null) should be returned.*/
  getSnapshotBeforeUpdate(prevProps, prevState) {
    // Are we adding new items to the list?
    // Capture the scroll position so we can adjust scroll later.
    if (prevProps.list.length < this.props.list.length) {
      const list = this.listRef.current;
      return list.scrollHeight - list.scrollTop;
    }
    return null;
  }

  /* You may call setState() immediately in componentDidUpdate() but note that it must be wrapped in a condition or you’ll cause an infinite loo */
  /* componentDidUpdate() will not be invoked if shouldComponentUpdate() returns false. */
  /* If you need to perform a side effect (for example, data fetching or an animation) in response to a change in props, use componentDidUpdate lifecycle instead. */
  /* If you want to re-compute some data only when a prop changes, use a memoization helper instead. */
  /* If you want to “reset” some state when a prop changes, consider either making a component fully controlled or fully uncontrolled with a key instead. */
  /* if getSnapshotBeforeUpdate used in component, snapshot passed in as third param */
  componentDidUpdate(prevProps, prevState, snapshot) {
    if (this.props.userID !== prevProps.userID) {
      this.fetchData(this.props.userID);
    }
    // If we have a snapshot value, we've just added new items.
    // Adjust scroll so these new items don't push the old ones out of view.
    // (snapshot here is the value returned from getSnapshotBeforeUpdate)
    if (snapshot !== null) {
      const list = this.listRef.current;
      list.scrollTop = list.scrollHeight - snapshot;
    }
  }

  /* componentWillReceiveProps this lifecycle method often leads to bugs
  and inconsistencies, and for that reason it is going to be deprecated in the
  future.


  *****************************************************************************
  If you need to perform a side effect (for example, data fetching or
  an animation) in response to a change in props, use componentDidUpdate
  lifecycle instead
  *****************************************************************************

  For other use cases, follow the recommendations in this
  blog post about derived state. If you used componentWillReceiveProps for
  re-computing some data only when a prop changes, use a memoization helper
  instead. If you used componentWillReceiveProps to “reset” some state when a
  prop changes, consider either making a component fully controlled or fully
  uncontrolled with a key instead. In very rare cases, you might want to use
  the getDerivedStateFromProps lifecycle as a last resort. */
  componentWillReceiveProps(nextProps)

  shouldComponentUpdate(nextProps, nextState)
  componentDidCatch(error, info)
  componentWillUnmount()

  render(){}

}
