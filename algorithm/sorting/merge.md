# Merge Sort

- Based on `merge` operation.
- The time complexity of best / worst / average case is the same: __O(nlogn)__;
- Two design:
  - Top-down (Recursive)
  - Bottom-up (iterative)

## Algorithm

Split array into length 1 subarray.

```js
    [8, 7, 1, 2, 4, 6, 5, 3]
                |
   [8, 7, 1, 2] | [4, 6, 5, 3]
                |
  [8, 7] [1, 2] | [4, 6] [5, 3]
                |
[8] [7] [1] [2] | [4] [6] [5] [3]
                V
              split
```

Recursively merge subarray respecting the order.

```js
              Merge
                |
[8] [7] [1] [2] | [4] [6] [5] [3]
                |
  [7, 8] [1, 2] | [4, 6] [3, 5]
                |
   [1, 2, 7, 8] | [3, 4, 5, 6]
                V
    [1, 2, 3, 4, 5, 6, 7, 8]
```

### Merge

```js
function merge(left, right) {
    const result = [];
    while(left.length > 0 && right.length > 0) {
        if (left[0] < right[0]) {
            result.push(left.shift());
        } else {
            result.push(right.shift());
        }
    }
    return result.concat(left, right);
}
```

### Top-down (Recursive)

```js
function sort(arr) {
    if (arr.length <= 1) {
        return arr;
    } else {
        const middle = Math.floor(arr.length / 2);
        const left = arr.slice(0, middle);
        const right = arr.slice(middle);
        return merge(sort(left), sort(right));
    }
}
```

### Bottom-up (iterative)

```js
function sort(arr) {
    const result = [...arr];
    const length = arr.length;
    for (let width = 1; width < length; width *= 2) {
        for (let i = 0; i < length; i += width * 2) {
            const leftIdx = i;
            const rightIdx = i + width;
            const left = result.slice(leftIdx, leftIdx + width);
            const right = result.slice(rightIdx, rightIdx + width);
            result.splice(i, width * 2, ...merge(left, right));
        }
    }
    return result;
}
```
