## Converting flat structure to tree

```javascript
const assert = require('assert');

const dataPoints = require('./data_points');
const testTree = require('./data_tree');

// Sort data points
dataPoints.sort((a, b) => (a.start + a.tier) > (b.start + b.tier) ? 1 : -1);

// Build tree
const dataTree = [];
dataPoints.forEach(p => addNode(p, dataTree));

function addNode(point, tree, previousTier = null) {

    point.children = [];

    if (!tree.length) return tree.push(point)

    for (const node of tree) {
        if (point.tier.startsWith(node.tier) && previousTier !== node.tier) {
            return addNode(point, node.children, node.tier);
        }
    }

    return tree.push(point);
}

// Test trees equality
assert.deepStrictEqual(dataTree, testTree, 'Tree objects are different');
assert.equal(JSON.stringify(dataTree), JSON.stringify(testTree), 'Serialized tree objects are different');

console.log('Tests are passed');

```

