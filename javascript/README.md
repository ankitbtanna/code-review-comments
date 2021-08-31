# JavaScript
Code review comments for JavaScript Code

## Use nullish coallescing operator for assigning default values to the null/undefined values
- The logical nullish assignment (x ??= y) operator only assigns if x is nullish (null or undefined).
- The nullish coalescing operator (??) is a logical operator that returns its right-hand side operand when its left-hand side operand is null or undefined, and otherwise returns its left-hand side operand.

```
const a = { duration: 50 };

a.duration ??= 10;
console.log(a.duration);
// expected output: 50

a.speed ??= 25;
console.log(a.speed);
// expected output: 25

const foo = undefined ?? 'default string';
console.log(foo);
// expected output: "default string"

const baz = 0 ?? 42;
console.log(baz);
// expected output: 0let foo = { someFooProp: "hi" };

console.log(foo.someFooProp?.toUpperCase() ?? "not available"); // "HI"
console.log(foo.someBarProp?.toUpperCase() ?? "not available"); // "not available"


let foo = { someFooProp: "hi" };
console.log(foo.someFooProp?.toUpperCase() ?? "not available"); // "HI"
console.log(foo.someBarProp?.toUpperCase() ?? "not available"); // "not available"
```

## Avoid using else keyword in if-else conditions
- Contributes to Readability and maintainability
- Else causes nesting 
- Use Guard clause methodology where we write negative scenarios first
- A function can have multiple-return statements. Multi-exit points
- Break the complexity into smaller functions

### Before
```
function canDrink(person) {
    if (person?.age !== null) {
        if (person.age < 18) {
            console.log('Nope!');
            return;
        } else if (person.age < 21) {
            console.log('Not in US');
            return;
        } else {
            console.log('Yes!');
            return;
        }
    } else {
        console.log('You are not a person.');
        return;
    }
}

canDrink({ age: 22 });
```

### After
```
function canDrink(person) {
    if (person?.age === null) {
        console.log("You are not a person.");
        return;
    }
  
    if (person.age < 18) {
        console.log("Nope!");
        return;
    }
    
    if (person.age < 21) {
        console.log("Not in US");
        return;
    }
    
    console.log("Yes!");
    return;
}

canDrink({ age: 22 });
```

## Function must be pure
A pure function does not interact with or modify the outside world. It solely depends on the input parameters for the computation.

### Example of impure function
```
var productPrice = 30;
function calculateGST() {
    productPrice = productPrice + 5;
    return productPrice * 0.05;
}
```
### Example of pure function
```
function calculateGST( productPrice ) {
    return productPrice * 0.05;
}
```

## Function name must begin with a verb/action
- print, load, delete, add, set, get

## Always provide Default values for parameters in the function
- If a function has parameters in the function declaration, make sure they have default values.
- This will reduce the usage of elvis operator (?) and make use of conditional execution
- Elvis operator is unwanted and creates a branch in the code. A branch which may need coverage.

### Before
```
function printPersonDetails(personDetails) {
    const name = personDetails?.name;
    console.log(name.toUpperCase()) // throws error
}
```

### After
```
function printPersonDetails(personDetails = undefined) {
    if (personDetails) {
        const name = personDetails.name;
        console.log(name.toUpperCase())
    }
}
```

```
function printPersonDetails(personDetails = { name: '' }) {
    const name = personDetails.name;
    console.log(name.toUpperCase())
}
```

## If function has more than 2 parameters, ask developers to use named parameter
- Named parameter helps developer not remember the order of parameters to be provided.
- They need to just pass the parameters in an object without need of remembering the order.
- They just need to remember the name of parameter which can be standardized by documentation
    - for e.g. what kind of parameter are you expecting
    - if it's id, parameter name should always be id
    - if it's name, parameter name should always be name

### Before
```
function printPersonDetails(name, age, gender) {
    console.log(name, age, gender);
}

printPersonDetails('john', 31, 'male'); // need to remember this sequence
printPersonDetails('john', 'male', 31); // Messing up the sequence causes the logic to be messed up
```

### After
```
function printPersonDetails(name, age, gender) {
    console.log(name, age, gender);
}

// No need to remember the sequence
printPersonDetails({
    age: 31,
    gender: 'male',
    name: 'john'
});

printPersonDetails({
    age: 31,
    name: 'john',
    gender: 'male'
});
```

## Use de-structuring and re-structuring for functions which take objects as in input and returning manipulated object
### Example
```
function ajaxOptions({
    url = "http://some.base.url/api",
    method = "post",
    data,
    callback,
    headers: [
        headers0 = "Content-Type": "text/plain",
        ...otherHeaders
    ] = []
} = {}) {
    return {
        url,
        method,
        data,
        callback,
        headers: [
            headers0,
            ...otherHeaders
        ]
    }
}

const ajaxWithDefault = ajaxOptions();
/* returns {
    url: 'http://some.base.url/api',
    method: 'post',
    headers: [
        "Content-Type": "text/plain"
    ]
} */

const ajaxWithOverrides = ajaxOptions({
    url: 'http://some.base.url/new',
    method: 'get',
    headers: [
        'Authorization': 'Bearer Token'
    ]
})
```

## Stop using arr.indexOf() functions for verifying if the item is present or now. Use arr.find() or arr.findIndex() or arr.includes()
- arr.indexOf() returns -1 value if item is not found
- JavaScript has better options like
    - arr.find()
    - arr.findIndex()
    - arr.includes()
- These methods help us determine if the element is present or not
- includes returns true or false value instead of -1
- includes is more ergonomic and semantic

