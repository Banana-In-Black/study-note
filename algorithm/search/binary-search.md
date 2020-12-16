# Binary Search

## Implementation

### Recursive

```js
/**
 * @return The index of value. If not found, return -([index to insert] + 1) instead.
 */
function binarySearch(ary, value, low = 0, high = ary.length - 1) {
    if (low > high) {
        return -(low + 1);
    }

    // equals to Math.floor((low + high) / 2)
    // only if low and high are non-negative.
    const mid = low + high >>> 1; 

    if (value > ary[mid]) {
        return binarySearch(ary, value, mid + 1, high);
    } else if (value < ary[mid]) {
        return binarySearch(ary, value, low, mid - 1);
    } else {
        return mid;
    }
}
```

### Iterative

```js
/**
 * @return The index of value. If not found, return -([index to insert] + 1) instead.
 */
function binarySearch(ary, value) {
    let low = 0;
    let high = ary.length - 1;

    while (low <= high) {
        // equals to Math.floor((low + high) / 2)
        // only if low and high are non-negative.
        const mid = low + high >>> 1;

        if (value > ary[mid]) {
            low = mid + 1;
        } else if (value < ary[mid]) {
            high = mid - 1;
        } else {
            return mid;
        }
    }

    return -(low + 1);
}
```
