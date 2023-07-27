#algorithms #data_structures 

**Difference** : [[MAP vs SET.loom]]

NOTE : Both only take *unique* values

## Map

```Javascript
// Create an empty map
let example = new Map()

// Assign an object to a map
const obj = { foo: 'bar', baz: 42 };
const map = new Map(Object.entries(obj));
console.log(map); // Map { foo: "bar", baz: 42 }

// Add key-value to a map
example.set(key,value)

// Check of a map has a key
example.has(key)

// Assign a value to a key
example['key1'] = 13;

```

## Set
``` javascript
// Create an empty map
let example = new Set()

// Assign an array to a map
const arr = [1,2,3,4];
const set = new Set();
arr.forEach((x)=>{set.add(x)})
console.log(set); // Set(1) { 1, 2, 3, 4 }

// Add key-value to a map
example.add(value)

// Check of a map has a key
example.has(value)
```


- In general use :
	- `Sets` when you need to _store unique values_ 
	- `Maps` when you need to _store key-value pairs_ .