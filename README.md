# Mocha.js Cheat Sheet (https://mochajs.org/)
Mocha.js (Unit Testing) Cheat Sheet with the most needes stuff..


# Guides
Beginner Tutorial: https://www.youtube.com/watch?v=oJWOmT5UZYw



<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />

# Dependencies
```bash
npm i --save-dev expect mocha
npm i -g nodemon
```

## Use nodemon with test
```javascript
// mocha **/*.test.js will detect any file that ends with .test.js! This means multiple files can be tested this way
// --timeout 0 means no timeout for any it/describe block. This is usefully when you got long async calls and have to wait.
// --exit will exit the script after all tests are done
// nodemon is a live watcher package that will detect changes in our files and restart the test
"scripts": {
  "test": "mocha **/*.test.js --timeout 0 --exit",
  "test-watch": "nodemon --exec \"npm test\""
}
```
```bash
npm run test-watch
```



<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />

# it
```javascript
it('Should return 1', async() => {
    expect( await storeMessages({"msg": 'test'}) ).toBe( 1 );
});
```

# describe
```javascript
describe('storeMessages()', () => {

  it('Should return 1', async() => {
    expect( await storeMessages({"msg": 'test'}) ).toBe( 1 );
  });

});
```

# before, beforeEach, after, afterEach
```javascript
describe('hooks', function() {

    before(function() {
        // runs before all tests in this file regardless where this line is defined.
    });

    after(function() {
        // runs after all tests in this file
    });

    beforeEach(function() {
        // runs before each test in this block
    });

    afterEach(function() {
        // runs after each test in this block
    });

    // it test cases here..
});
```

# skip test (works with id and describe)
```javascript
// method #1 (xit)
xit('Should return 1', async() => {
   expect( await storeMessages({"msg": 'test'}) ).toBe( 1 );
});

// method #2 (skip)
it.skip('Should return 1', async() => {
   expect( await storeMessages({"msg": 'test'}) ).toBe( 1 );
});
```

# run only 1 specific test and ignore other 
```javascript
describe('feature 1', function() {});
describe.only('feature 2', function() {});
describe('feature 3', function() {});
// Only the feature 2 block would run in this case.
```
