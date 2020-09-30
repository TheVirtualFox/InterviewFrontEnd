Implement methods that? during the execution of line
(2).plus(3).minus(1) would return 4.

```javascript

Number.prototype.plus = function(value) {
  return this + value;
}

Number.prototype.minus = function(value) {
  return this - value;
}

console.log((2).plus(3).minus(0));
```