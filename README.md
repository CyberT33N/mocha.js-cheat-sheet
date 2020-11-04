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
"scripts": {
  "test": "mocha yourfile.test.js --timeout 0",
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
