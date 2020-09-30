##var VS let VS const

var:
  - unction scoped
  - Undefined when accessing a variable before it's declared
let:
  - Block scoped
  - ReferenceError when accessing a variable before it's declared
const:
  - Block scoped
  - ReferenceError when accessing a variable before it's declared
  - Can't be reassigned

```javascript
function varTest() {
    var x = 1;
    if (true) {
      var x = 2;  // same variable!
      console.log(x);  // 2
    }
    console.log(x);  // 2
}
```

```javascript
function letTest() {
  let x = 1;
  if (true) {
    let x = 2;  // anather another
    console.log(x);  // 2
  }
  console.log(x);  // 1
}
```

```javascript
if (x) {
  let foo;
  let foo; // SyntaxError thrown.
}
```

```javascript
function do_something() {
  console.log(bar); // undefined
  console.log(foo); // ReferenceError: foo is not defined
  var bar = 1;
  let foo = 2;
}
```

```javascript
var a = 5;
var b = 10;

if (a === 5) {
  let a = 4; // The scope is inside the if-block
  var b = 1; // The scope is inside the function

  console.log(a);  // 4
  console.log(b);  // 1
} 

console.log(a); // 5
console.log(b); // 1
```
