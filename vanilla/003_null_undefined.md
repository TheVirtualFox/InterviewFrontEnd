## null undefined
In JavaScript, undefined means a variable has been declared but has not yet been assigned a value, such as:

```javascript
var TestVar;
alert(TestVar); //shows undefined
alert(typeof TestVar); //shows undefined
```

null is an assignment value. It can be assigned to a variable as a representation of no value:

```javascript
var TestVar = null;
alert(TestVar); //shows null
alert(typeof TestVar); //shows object
```

From the preceding examples, it is clear that undefined and null are two distinct types: undefined is a type itself (undefined) while null is an object.

```javascript
null === undefined // false
null == undefined // true
null === null // true

console.log(!!null) // false
console.log(!!undefined) // false

console.log(Boolean(null)) // false
console.log(Boolean(undefined)) // false
```

```javascript
null = 'value' // ReferenceError
undefined = 'value' // 'value'
```

- null is an assigned value. It means nothing.
- undefined typically means a variable has been declared but not defined yet.
- null and undefined are falsy values.
- null and undefined are both primitives. However an error shows that typeof null = object.
- null !== undefined but null == undefined
- undefined return functions with out return