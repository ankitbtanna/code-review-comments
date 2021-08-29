# JavaScript
Code review comments for JavaScript Code

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