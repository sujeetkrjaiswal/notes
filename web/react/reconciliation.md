# Reconciliation

## Diff Algorithm

Generic Solution: Tree Edit Algorithm ([https://grfia.dlsi.ua.es/ml/algorithms/references/editsurvey\_bille.pdf](https://grfia.dlsi.ua.es/ml/algorithms/references/editsurvey\_bille.pdf))

**Complexity:** O(n^3)

React implements a **heuristic O(n) algorithm** based on two assumptions:

1. Two elements of different types will produce different trees.
2. The developer can hint at which child elements may be stable across different renders with a `key` prop.

React handles below different cases

#### Elements of Different Types

Tear down old tree and build a new one.

Components getting teared down will get `componentWillUnmount` and the components getting re-build will get `componentWillMount` and later `componentDidMount`

#### DOM Elements of same type

Keeps the underlying DOM and only update the required attributes.

```
<div style={{color: 'red', fontSize: 14}}>...</div>
<div style={{color: 'blue', fontSize: 14}}>...</div>
```

For the above, it will only update collor prop and not fontSize.

#### Components Element of same type

The instance remains the same and hence state is maintained across render. React updates the props of the underlying component and will call `componentWillReceiveProps` and `componentWillUpdate` on the instance. Next the `render` method is called.

#### Recursion on Children

By default, when recursing on the children of a DOM node, React just iterates over both lists of children at the same time and generates a mutation whenever there’s a difference.

If no keys are provided and the DOM nodes are inserted in the  begining or middle, it has bad performance.&#x20;

#### Keys

React supports a `key` attribute. When children have keys, React uses the key to match children in the original tree with children in the subsequent tree.
