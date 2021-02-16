# Mocha.js Cheat Sheet (https://mochajs.org/)
Mocha.js (Unit Testing) Cheat Sheet with the most needes stuff..


# Guides
Beginner Tutorial: https://www.youtube.com/watch?v=oJWOmT5UZYw






















<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

## Export results to HTML (https://www.npmjs.com/package/mochawesome)
```javascript
// ## mocha.js ##
// mocha **/*.test.js <-- Match any file that end with .test.js
// mocha ./utils --recursive <-- Match all files in folder recursive

/* 
--timeout 0 <-- disable timeout this is usefully for long async scripts. However it will be general for all unit tests.
--exit <-- exit the script. usefully for automated runs where you need to know when test is finished.
--parallel <-- runs tests parallel

## mochawesome
--reporter mochawesome <-- print the results to HTML
--reporter-options reportDir=./website/report,reportFilename=server <-- Set custom directory for export of the HTML files. default will be root directory. Also we can define custom name for the file by using reportFilename.
*/

"scripts": {
    "test": "mocha ./utils --recursive --timeout 0 --exit --parallel --reporter mochawesome --reporter-options reportDir=./website/report",
    "test-watch": "nodemon --exec \"npm test\""
  },
  "nodemonConfig": {
     "ignore": ["mochawesome-report/*"]
  }
```





























<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# Dependencies
```bash
npm i --save-dev expect mocha
npm i -g nodemon
```

## Use nodemon with test
```javascript
/*
--timeout 0 <-- means no timeout for any it/describe block. This is usefully when you got long async calls and have to wait.
--exit  <-- will exit the script after all tests are done
--ignore 'website/report/*' <-- will ignore any files/folder to prevent reload when files change there
-e js,html,yml <-- will monitor .js, .html & .yml files
*/

// nodemon is a live watcher package that will detect changes in our files and restart the test
"scripts": {
  "test": "mocha ./utils --recursive --timeout 0 --exit",
  "test-watch": "nodemon -e js,html,yml --ignore 'website/report' --exec \"npm test\""
}
```
```bash
npm run test-watch
```

































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# it
```javascript
// sync
it('Should return 1', () => {
  expect(number).toBe( 1 );
});

// await
it('Should return 1', async() => {
  expect(await storeMessages({"msg": 'test'})).toBe( 1 );
});

// multiple expect
it('Should return 1', async() => {
  expect(anyTest()).toBe(1337);
  expect(await storeMessages({"msg": 'test'})).toBe( 1 );
});
```

## skip it test
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







<br><br><br><br>

# describe (does not work with async)
```javascript
describe('storeMessages()', () => {
  const anyGlobalVariableForThisDescribeBlock = true;

  it('Should return 1', async() => {
    expect( await storeMessages({"msg": 'test'}) ).toBe( 1 );
  });
});
```
<br><br>


# skip describe block
```javascript
describe.skip('storeMessages()', () => {
  it('Should return 1', async() => {
    expect( await storeMessages({"msg": 'test'}) ).toBe( 1 );
  });
});
```
<br><br>

## Only run 1 describe block (.only)
```javascript
describe('feature 1', function() {});
describe.only('feature 2', function() {});
describe('feature 3', function() {});
// Only the feature 2 block would run in this case.
```











<br><br><br><br>

## timeout
- Default is 2 seconds
- **You can not use arrow functions with this.timeout**
```javascript
// Method #1
it('Some test', () => { /*..*/ }).timeout(5000)
describe('Some test', () => { /*..*/ }).timeout(5000)


// Method #2
it('accesses the network', function(done){
  this.timeout(500);
  /*..*/ 
});

describe('[PUPPETEER] Stress/stability tests', function () {
    this.timeout(60000 * 5)
    /*..*/ 
});
```


<br><br>

# before
```javascript
describe('some test here..', ()=>{
    // runs before all tests in this file regardless where this line is defined.
    before(()=>{ /*..*/ });


    // if you want to run async/use promise inside of before you need to call done at the end.
    // method #1
    before(done=>{
     (async()=>{
       // do something..
       done();
     })()
    });
    
    // method #2
    before(done=>{
     (async()=>{
       // do something..
     })()
    }).catch(done);
    
    // method #3
    before(done=>{
     (async()=>{
       // do something..
     })()
    }).then(done, done)
});
```

<br><br>

# beforeEach
```javascript
describe('some test here..', ()=>{
    beforeEach(()=>{ /*..*/ });
    
    // it test cases here..
});
```

<br><br>

# after
```javascript
describe('some test here..', ()=>{
    after(()=>{ /*..*/ });
});
```



<br><br>

# afterEach
```javascript
describe('some test here..', ()=>{
    afterEach(()=>{ /*..*/ });
});
```


















<br><br>

# global variables
```javascript
describe('Client Side Services', ()=> {
  before(done=>{(async ()=>{
    global.pptr = await new Init().getPPTR();
  })().catch(e=>{log('client.test.mjs - BEFORE() - Error: ' + e);});});


  it('Client Side test success - .finish-test should exist', async ()=>{
    expect(
        await pptr.page.waitForSelector('.finish-test', {
          visible: true, timeout: 0,
        }), // await pptr.page.waitForSelector('.finish-test', {
    ).toBeTruthy(); // expect(
  }); // it('Client Side test success - .finish-test should exist', async ()=>{
}); // describe('Client Side Services', ()=>{
```







































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


# Client Side

## ESM
- https://medium.com/dailyjs/running-mocha-tests-as-native-es6-modules-in-a-browser-882373f2ecb0
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

    <!-- test files -->
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
