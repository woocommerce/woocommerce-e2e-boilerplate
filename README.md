# WooCommerce E2E Tests Boilerplate

Are you looking to set up E2E tests for your WooCommerce extension? Then you can make use of the default WooCommerce E2E package.

This repo aims to provide a stripped down version of the default WooCommerce E2E test suite(https://github.com/woocommerce/woocommerce/tree/master/tests/e2e) along with basic set up instructions to get started.

### Pre-requisites (Running tests locally)

* **Node.js** - https://nodejs.org/en/download/
* **NVM** - https://github.com/nvm-sh/nvm
* **Docker Desktop**
  * **Mac** - https://docs.docker.com/docker-for-mac/install/
  * **Windows** - https://docs.docker.com/docker-for-windows/install/

### Required Packages

* **WooCommerce E2E Environment** - `npm i @woocommerce/e2e-environment --save-dev`
* **WooCommerce E2E Utils** - `npm i @woocommerce/e2e-environment --save-dev`
* **Babel Preset Environment** - `npm i @babel/preset-env --save-dev`
* **Jest** - `npm i jest --save-dev`

### Optional Packages

* **WooCommerce Core E2E Tests** - `npm i @woocommerce/e2e-core-tests --save-dev`
* **WooCommerce API** - `npm i @woocommerce/api --save-dev`
* **WordPress E2E Utils** - `npm i @wordpress/e2e-test-utils --save-dev`

### Setup

* Download the **latest release**** of this package and copy the contents to the **`tests`** directory on the root of your WooCommerce extension.
* Create the file **`.nvmrc`** on your project root and add the content as **`v12`**.
* Modify **`package.json`** and add the following under **`scripts`**.

```
"docker:down": "npm explore @woocommerce/e2e-environment -- npm run docker:down",
"docker:ssh": "npm explore @woocommerce/e2e-environment -- npm run docker:ssh",
"docker:up": "npm explore @woocommerce/e2e-environment -- npm run docker:up",
"test:e2e": "npm explore @woocommerce/e2e-environment -- npm run test:e2e",
"test:e2e-debug": "npm explore @woocommerce/e2e-environment -- npm run test:e2e-debug",
"test:e2e-dev": "npm explore @woocommerce/e2e-environment -- npm run test:e2e-dev"
```

* Spin up Docker using **`npm run docker:up`**.
* To confirm that the Docker instance is running, open **`http://localhost:8084`** on your browser to see the newly set up WordPress site.
* Run the example test using **`npm run test:e2e`**.
* Once you've done testing, spin down docker using **`npm run docker:down`**.

### Adding new tests

To add new E2E tests, add new files to **`tests/e2e/specs/`** with the extension **`.test.js`**. The **`example.test.js`** may be used as a starting point.

### Relevant files

* **Docker Initialization Script** - `tests/e2e/docker/initialize.sh`. This can be used to set up your testing environment such as installing additional plugins via WP-CLI / importing data / running additional scripts. Reference - https://github.com/woocommerce/woocommerce/blob/master/tests/e2e/docker/initialize.sh
* **Jest Setup Script** - `tests/e2e/config/jest.setup.js`. Can be used to define custom scripts for Jest. You can also make use of the example script **`tests/e2e/config/jest.setup.example.js`** which requires **`@wordpress/e2e-test-utils`** package to be installed to run the defined functions. Please note that once the default template is enabled, all posts and products will be trashed, and local storage will be cleared between tests.
* **Test Varibles** - To override the default test variables, create a new JSON file at **`tests/e2e/config/default.json`**. Reference - https://github.com/woocommerce/woocommerce/blob/master/tests/e2e/env/config/default.json.

### Imortant References

* **WooCommerce E2E Tests** - https://github.com/woocommerce/woocommerce/blob/master/tests/e2e/README.md
* **WooCommerce E2E Environment** - https://www.npmjs.com/package/@woocommerce/e2e-environment
* **WooCommerce E2E Core Tests** - https://www.npmjs.com/package/@woocommerce/e2e-core-tests
* **WordPress E2E Test Utils** - https://www.npmjs.com/package/@wordpress/e2e-test-utils

### Changes made to the original WooCommerce E2E Test suite

* Removed the folders `tests/e2e/api`, `tests/e2e/core-tests`, `tests/e2e/utils`, `tests/e2e/env` since these are standalone packages.
* Removed the folder `tests/e2e/bin` (build scripts for E2E Components)
* Renamed `tests/e2e/config/jest.setup.js` to `tests/e2e/config/jest.setup.example.js`.
* Modified `tests/e2e/docker/initialize.sh` to include additional WooCommerce store setup.
