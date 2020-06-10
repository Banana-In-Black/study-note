# Tail Recursion

## Tail Call
The last action of function is return another function call's returning value.

```js
function doA(a) {
    return a;
}
function doB(b) {
    return doA( b + 1 ); // a tail call
}
function foo() {
    return 20 + doB(10); // not a tail call
}
foo(); // 31
```

## Tail Call Optimization

To avoid allocating a new stack frame for a function because the calling function will simply return the value that it gets from the called function. The most common use is **tail recursion** [[anchor]](#recursion), where a recursive function written to take advantage of tail call optimization can use constant stack space. [[stackoverlfow]](https://stackoverflow.com/a/310980/2334946)

In other words, tail call optimization means that it is possible to call a function from another function without growing the call stack. (Usually calling a new function requires an extra amount of reserved memory to manage the stack frame)

> A **stack frame** is a frame of data that gets pushed onto the stack. In the case of a **call stack**, a stack frame would represent a function call and its argument data. [[stackoverflow]](https://stackoverflow.com/a/10057535/2334946)

## Tail Recursion <a id="recursion"></a>
- **Traditional recursion**: the typical model is that you perform your recursive calls first, and then you take the return value of the recursive call and calculate the result. In this manner, you don't get the result of your calculation until you have returned from every recursive call.

    ```js
    function recsum(x) {
        if (x === 1) {
            return x;
        } else {
            // not a tail call
            return x + recsum(x - 1);
        }
    }

    recsum(5);
    // 5 + recsum(4)
    // 5 + (4 + recsum(3))
    // 5 + (4 + (3 + recsum(2)))
    // 5 + (4 + (3 + (2 + recsum(1))))
    // 5 + (4 + (3 + (2 + 1)))
    // 15
    ```

- **Tail recursion**: you perform your calculations first, and then you execute the recursive call, passing the results of your current step to the next recursive step. This results in the last statement being in the form of `return recursiveFunction(params);`. **Basically, the return value of any given recursive step is the same as the return value of the next recursive call.**  

    The consequence of this is that once you are ready to perform your next recursive step, you don't need the current stack frame any more. This allows for some optimization.

    ```js
    function tailrecsum(x, running_total = 0) {
        if (x === 0) {
            return running_total;
        } else {
            // a tail call
            return tailrecsum(x - 1, running_total + x);
        }
    }

    tailrecsum(5, 0);
    // tailrecsum(4, 5)
    // tailrecsum(3, 9)
    // tailrecsum(2, 12)
    // tailrecsum(1, 14)
    // tailrecsum(0, 15)
    // 15
    ```

Reference: [[stackoverflow]](https://stackoverflow.com/a/33930/2334946)