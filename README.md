# Ionic Project with E2E testing

This is a simple tutorial to configure E2E testing in an ionic project by using only the minimal dependencies and configurations.
This tutorial is based on both <a href="https://leifwells.github.io/2017/08/27/testing-in-ionic-configure-existing-projects-for-testing/">Leif Wells</a> & <a href="http://lathonez.com/2017/ionic-2-e2e-testing/">Lathonez</a> posts on creating E2E tests in ionic.

Step 1: Install Required Node Modules and dependencies
------------------------------------------------------
```
npm install --save-dev jasmine-spec-reporter protractor ts-node @types/jasmine
```
There are quite a few modules here, but the important modules are jasmine and protractor. 
<i>jasmine</i> is the Jasmine module which is the test framework. 
<i>protractor</i> is the Protractor module which is our testing environment for our end-to-end tests. 
The rest of the modules are utilities that allow this configuration to work.

Step 2: Add Scripts to the package.json
---------------------------------------
There are a couple of scripts that need to be added to the `package.json` to make running tests from the command-line possible. Open the `package.json` file and add the following scripts to your `"scripts"` node:

```
"e2e": "npm run e2e-update && npm run e2e-test",
"e2e-test": "protractor ./test-config/protractor.conf.js",
"e2e-update": "webdriver-manager update --standalone false --gecko false"
```

With the addition of these items, you will be able to enter the `npm run e2e` to begin end-to-end testing.

Step 3: Add the Configuration Files
-----------------------------------
The control center for this testing configuration are a set of files which need to be added. Locate the `test-config` folder in this project and copy and paste that folder into the root of your project. Open the folder in your code editor and take a look. You should see a `protractor.conf.js` file which is the configuration file for Protractor.

We need to add one more folder to our project so we can implement the end-to-end testing solution. Locate the `e2e` folder in this project and copy and paste that folder into the root of your project.

The `tsconfig.json` file is an important configuration file which assists with compiling the project and tests for end-to-end testing. The file is placed here because it isolates it from the project’s `tsconfig.json` file.

For the lazy:

```
for file in protractor.conf.js
do
  wget https://raw.githubusercontent.com/marioshtika/ionic-project-with-e2e-testing/master/test-config/${file}
done

mkdir e2e
cd e2e

for file in tsconfig.json
do
  wget https://raw.githubusercontent.com/marioshtika/ionic-project-with-e2e-testing/master/e2e/${file}
done
```

The app.e2e-spec.ts file is an actual E2E test file. Note the name of the test as our configuration file is looking for files that have the .e2e-spec.ts as part of the name to identify it as an E2E test and not an unit test.

If you are following along with this tutorial, when you run the `npm run e2e` command in your terminal window, this test should work.

A simple e2e test on app.ts
---------------------------
Create a simple e2e test file `./e2e/app.e2e-spec.ts` to get us going:

```
import { browser, element, by } from 'protractor';

describe('MyApp', () => {

  beforeEach(() => {
    browser.get('/');
  });

  it('should have a title', () => {
    expect(browser.getTitle()).toEqual('Ionic App');
  });
})
```

<strong>Important: When you run E2E tests `npm run e2e`, make sure you are running `ionic serve` in another terminal window. The E2E configuration expects to have a connection to the server and if it is not it will fail with an error.</strong>
