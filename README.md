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

# Expect
```bash
npm i expect --save-dev
```
```javascript
const expect = require('expect');

const add = (a, b) => a + b;
expect( add(2, 3) ).toBe(5);
```
