# WooCommerce E2E Tests Boilerplate

Are you looking to set up E2E tests for your WooCommerce extension? Then you can make use of the default WooCommerce E2E package.

This repo aims to provide a stripped down version of the default [WooCommerce E2E test suite](https://github.com/woocommerce/woocommerce/blob/trunk/plugins/woocommerce/tests/e2e/README.md) along with basic set up instructions to get started.

---

### Pre-requisites (Running tests locally)

* **Node.js** - https://nodejs.org/en/download/
* **NVM** - https://github.com/nvm-sh/nvm
* **Docker Desktop**
  * **Mac** - https://docs.docker.com/docker-for-mac/install/
  * **Windows** - https://docs.docker.com/docker-for-windows/install/

### Required Node Packages

* [**WooCommerce E2E Environment**](https://www.npmjs.com/package/@woocommerce/e2e-environment)
* [**WooCommerce E2E Utils**](https://www.npmjs.com/package/@woocommerce/e2e-utils)
* [**WooCommerce API**](https://www.npmjs.com/package/@woocommerce/api)
* [**Jest**](https://www.npmjs.com/package/jest)

### Optional Node Packages

* [**WooCommerce Core E2E Tests**](https://www.npmjs.com/package/@woocommerce/e2e-core-tests)
* [**WooCommerce Admin E2E Tests**](https://www.npmjs.com/package/@woocommerce/admin-e2e-tests)

---

## Setup

**Requirements**

* `npm@6` is required for setup. To install, run:
```
npm install -g npm@6
```

* The following steps are intended to be used with an existing WooCommerce extension that has a `package.json` file.
  * If your project does not contain a `package.json` file, create one:

```
npm init
```
More information on creating a `package.json` file can be found [here](https://docs.npmjs.com/creating-a-package-json-file#creating-a-new-packagejson-file).

**Steps**

* Download the **[latest release](https://github.com/woocommerce/woocommerce-e2e-boilerplate/releases)** of this package to the root of your WooCommerce extension.
* Modify **`tests/e2e/docker/initialize.sh`** as required.
* Modify **`package.json`** and add the following under **`scripts`**.

```
"docker:down": "npx wc-e2e docker:down",
"docker:ssh": "npx wc-e2e docker:ssh",
"docker:up": "npx wc-e2e docker:up",
"test:e2e": "npx wc-e2e test:e2e",
"test:e2e-debug": "npx wc-e2e test:e2e-debug",
"test:e2e-dev": "npx wc-e2e test:e2e-dev"
```

* Create the file **`.nvmrc`** on your project root and add the content as **`v12`**.
* Activate v12 of Node for your shell session by running **`nvm use`** from the project root.
* Install required packages as development dependencies.

```bash
npm i --save-dev @woocommerce/e2e-environment @woocommerce/e2e-utils @woocommerce/api jest
```

* _Optional: Install the optional package listed below for the full WooCommerce e2e test suite_

```bash
npm i --save-dev @woocommerce/e2e-core-tests
```

* Spin up Docker using **`npm run docker:up`**.
* To confirm that the Docker instance is running, open **`http://localhost:8084`** on your browser to see the newly set up WordPress site.
* Run the example test using **`npm run test:e2e`**.
* Once you've completed testing, spin down docker using **`npm run docker:down`**.

---

### Customizing docker environment

- Docker Container - https://github.com/woocommerce/woocommerce/blob/trunk/packages/js/e2e-environment/builtin.md
- WordPress - https://github.com/woocommerce/woocommerce/blob/trunk/plugins/woocommerce/tests/e2e/docker/init-wp-beta.sh

### Adding new E2E tests

To add new E2E tests, add new files to **`tests/e2e/specs/`**. **`tests/e2e/specs/example.test.js`** in this repository may be used as a starting point. Tests can also be organized in folders under the `specs` directory. For example: **`tests/e2e/specs/{folder-name}/example.test.js`**.

- Files with the extensions **`.test.js`** or **`.spec.js`** are run automatically by `npm run test:e2e`.
- Adhoc tests can be created with the **`.js`** and executed with `npm run test:e2e tests/e2e/specs/test-example.js`.

### Running core E2E tests

If you installed the optional `@woocommerce/e2e-core-tests` package and wish to run the tests within your project, note that all of the tests are wrapped in functions. In order to run the tests, create a local test spec and use the test function. For example, to run the activation tests, you would need to do the following:

```javascript
const { runActivationTest } = require( '@woocommerce/e2e-core-tests' );

runActivationTest();
```

For more information about the core tests, including what tests are available, how to re-run tests, and general setup, please see the [WooCommerce Core End to End Test Suite README](https://github.com/woocommerce/woocommerce/blob/trunk/packages/js/e2e-core-tests/README.md).

### Running Core API tests

The full suite of core API tests are found in the [@woocommerce/api-core-tests](https://www.npmjs.com/package/@woocommerce/api-core-tests) package. It uses Jest as the test runner, and [Supertest](https://www.npmjs.com/package/supertest) as the API testing library. However as of Jest v27, it is [not possible](https://github.com/facebook/jest/pull/11084) to run Jest tests when they're inside the `node_modules` folder. Jest v28 [does not have this limitation](https://github.com/facebook/jest/releases/tag/v28.0.0-alpha.5), but is still in alpha at the time of this writing. The following steps will guide the user in setting up the core API tests outside the `node_modules` folder in order to successfully run them. 

From the root directory of your project, use `npm pack` to download a tarball file of the Core API testing package.
```
npm pack @woocommerce/api-core-tests
```

Decompress the tarball into the `tests` folder. By default, contents will be extracted to the `package` folder. Rename the folder to anything appropriate like `api`.
```
tar -xf woocommerce-api-core-tests-0.1.0.tgz -C tests
rm woocommerce-api-core-tests-0.1.0.tgz # delete the tarball
mv tests/package tests/api
``` 

Navigate to the `api` folder to install dependencies
```
cd tests/api
npm install
```

Assuming that the e2e environment is already up, run the "hello" API test group to verify if the tests can make authenticated and non-authenticated calls to the REST API of the environment being tested.
```
npm run test:hello
```

By default, the API tests will run against the E2E docker environment, as indicated in the `BASE_URL` variable in `tests/api/.env` file. You may specify a different test environment for the tests to run against by changing this variable, along with `USER_KEY` and `USER_SECRET`.


### Adding new API tests

New API tests should be written in the `tests/api/tests` folder. The `tests/api/tests/hello/hello.test.js` file contains a simple test structure, and can be used as template for succeeding tests. Make sure to follow [these guidelines](https://github.com/woocommerce/woocommerce/blob/trunk/packages/js/api-core-tests/README.md#writing-tests) when writing new tests.


### Relevant files

* **Docker Initialization Script** - `tests/e2e/docker/initialize.sh`. This can be used to set up your testing environment such as installing additional plugins via WP-CLI / importing data / running additional scripts. Reference - https://github.com/woocommerce/woocommerce/blob/trunk/plugins/woocommerce/tests/e2e/docker/initialize.sh
* **Jest Setup Script** - `tests/e2e/config/jest.setup.js`. Can be used to define custom scripts for Jest. You can also make use of the example script **`tests/e2e/config/jest.setup.example.js`** which requires **`@woocommerce/e2e-utils`** package to be installed to run the defined functions. Please note that once the default template is enabled, all posts and products will be trashed, and local storage will be cleared between tests.
* **Test Variables** - To override the default test variables, create a new JSON file at **`tests/e2e/config/default.json`**. Reference - https://github.com/woocommerce/woocommerce/blob/trunk/packages/js/e2e-environment/config/default.json.
* **.env.example File** - `tests/api/.env.example`. This is the template for creating a `.env` file, which contains test variables essential for running Core API tests.

### Important References

* **WooCommerce E2E Tests** - https://github.com/woocommerce/woocommerce/blob/trunk/plugins/woocommerce/tests/e2e/README.md
* **WooCommerce E2E Environment** - https://www.npmjs.com/package/@woocommerce/e2e-environment
* **WooCommerce E2E Core Tests** - https://www.npmjs.com/package/@woocommerce/e2e-core-tests
* **WooCommerce Core API Tests** - https://www.npmjs.com/package/@woocommerce/api-core-tests

### Examples

This section provides examples of plugins and packages that are implementing WooCommerce related end-to-end tests.

**Woo Manage Fraud Orders**

[Woo Manage Fraud Orders](https://github.com/prasidhda/woo-manage-fraud-orders) is making use of the following packages in their end-to-end tests:

* [**WooCommerce E2E Environment**](https://www.npmjs.com/package/@woocommerce/e2e-environment)
* [**WooCommerce E2E Utils**](https://www.npmjs.com/package/@woocommerce/e2e-utils)
* [**WooCommerce API**](https://www.npmjs.com/package/@woocommerce/api)
* [**WooCommerce Core E2E Tests**](https://www.npmjs.com/package/@woocommerce/e2e-core-tests)

Examples of how they've integrated these packages and built out tests can be found [in the specs folder](https://github.com/prasidhda/woo-manage-fraud-orders/tree/master/tests/e2e/specs) within the project.

**WooCommerce Payments**

WooCommerce Payments is using the following packages as part of the [WooCommerce Payments end-to-end test suite](https://github.com/Automattic/woocommerce-payments/tree/develop/tests/e2e):

* [**WooCommerce E2E Utils**](https://www.npmjs.com/package/@woocommerce/e2e-utils)
* [**WooCommerce API**](https://www.npmjs.com/package/@woocommerce/api)

An example usage can be found in the [`Order > Refund Failure` test suite](https://github.com/Automattic/woocommerce-payments/blob/develop/tests/e2e/specs/merchant/merchant-orders-refund-failures.spec.js).

**WooCommerce Admin**

WooCommerce Admin has an [Admin E2E Tests NPM package](https://www.npmjs.com/package/@woocommerce/admin-e2e-tests) that makes use of the [**WooCommerce E2E Environment**](https://www.npmjs.com/package/@woocommerce/e2e-environment) and [**WooCommerce E2E Utils**](https://www.npmjs.com/package/@woocommerce/e2e-utils) packages. Note that these tests have been written in TypeScript.

### Changes made to the original WooCommerce E2E Test suite

* Removed the folders `tests/e2e/api`, `tests/e2e/core-tests`, `tests/e2e/utils`, `tests/e2e/env` since these are standalone packages.
* Removed the folder `tests/e2e/bin` (build scripts for E2E Components)
* Removed `tests/e2e/docker/init-wp-beta.sh`
* Renamed `tests/e2e/config/jest.setup.js` to `tests/e2e/config/jest.setup.example.js`.
* Modified `tests/e2e/docker/initialize.sh` to include additional WooCommerce store setup.
