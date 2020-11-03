# mocha.js-cheat-sheet (https://mochajs.org/)
Mocha.js (Unit Testing) Cheat Sheet with the most needes stuff..


# Guides
Beginner Tutorial: https://www.youtube.com/watch?v=oJWOmT5UZYw



<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />

# Expect (https://jestjs.io/docs/en/expect)
```bash
npm i expect --save-dev
```
```javascript
const expect = require('expect');
```

<br />
<br />

## toBe
```javascript
const add = (a, b) => a + b;
expect( add(2, 3) ).toBe(5);
expect( typeof add(2, 3) ).toBe('number');
```


## toEqual
```javascript
expect( {"test": 1} ).toEqual( {"test": 1} );
```
