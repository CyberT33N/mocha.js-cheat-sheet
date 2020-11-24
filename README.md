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

## Export results to HTML (https://www.npmjs.com/package/mochawesome)
```javascript
  "scripts": {
    "test": "mocha **/*.test.js --timeout 0 --exit --reporter mochawesome",
    "test-watch": "nodemon --exec \"npm test\""
  },
  "nodemonConfig": {
     "ignore": ["mochawesome-report/*"]
  }
```

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

# it (work with async)
```javascript
it('Should return 1', async() => {
    expect( await storeMessages({"msg": 'test'}) ).toBe( 1 );
});
```

# describe (does not work with async)
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


<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


# Client Side

## ESM
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Mocha Tests</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <link rel="stylesheet" href="css/lib/mocha.css"/>
  </head>
  <body>
    <div id="mocha"></div>

    <!-- mocha files -->
    <script src="js/lib/chai.js"></script>
    <script src="js/lib/mocha.js"></script>
    <script class="mocha-init">mocha.setup('bdd')</script>

    <!-- TDD -->
    <script src="js/lib/expect.min.js"></script>

    <!-- lib packages -->
    <script src="js/lib/axios.min.js"></script>

    <!-- custom files -->
    <script type="module" src="js/utils/test.main.mjs"></script>

    <!-- mocha INIT -->
    <script class="mocha-init" type="module">mocha.checkLeaks();</script>
    <script class="mocha-exec" type="module">mocha.run();</script>

  </body>
</html>
```

```javascript
// test.main.mjs
import * as req from '/js/req.mjs';

describe('getUserDetails() - POST', ()=>{

  it('Should return data object with key _id', async()=>{
    const r = await req.getUserDetails('mocha');
    console.log('Simulate correct token - result: ' + JSON.stringify(r, null, 4));

    expect( r?.data ).toEqual(expect.objectContaining({ _id: expect.anything() }));
  });

});
```

```javascript
// req.mjs
export const getUserDetails = async (token)=>{ console.log( 'getUserDetails() - token: ' + token + '\nHost: ' + window.location.origin );
  return await axios.post(  window.location.origin + '/api/getUserDetails', { usertoken: token  }, {
    headers: { authorization: 'sample_auth_token..' }
  });
};
```
