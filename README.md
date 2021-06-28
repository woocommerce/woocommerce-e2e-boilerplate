# WooCommerce E2E Tests Boilerplate

Are you looking to set up E2E tests for your WooCommerce extension? Then you can make use of the default WooCommerce E2E package.

This repo aims to provide a stripped down version of the default [WooCommerce E2E test suite](https://github.com/woocommerce/woocommerce/tree/master/tests/e2e) along with basic set up instructions to get started.

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
* [**Jest**](https://www.npmjs.com/package/jest)

### Optional Node Packages

* [**WooCommerce Core E2E Tests**](https://www.npmjs.com/package/@woocommerce/e2e-core-tests)
* [**WooCommerce API**](https://www.npmjs.com/package/woocommerce-api)
* [**WordPress E2E Utils**](https://www.npmjs.com/package/@wordpress/e2e-test-utils) - All of this package is exported from `@woocommerce/e2e-utils`. However, you can install it directly if you choose.

---

### Setup

Note: the following assumes an existing WooCommerce extension with an existing `package.json`. 

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
npm install --save-dev @woocommerce/e2e-environment @woocommerce/e2e-utils jest
```

* _Optional: Install the optional packages listed above_

```bash
npm install --save-dev @woocommerce/e2e-core-tests @woocommerce/api @wordpress/e2e-test-utils
```

* Spin up Docker using **`npm run docker:up`**.
* To confirm that the Docker instance is running, open **`http://localhost:8084`** on your browser to see the newly set up WordPress site.
* Run the example test using **`npm run test:e2e`**.
* Once you've completed testing, spin down docker using **`npm run docker:down`**.

---

### Customizing docker environment

- Docker Container - https://github.com/woocommerce/woocommerce/blob/master/tests/e2e/env/builtin.md
- WordPress - https://github.com/woocommerce/woocommerce/blob/master/tests/e2e/docker/init-wp-beta.sh

### Adding new tests

To add new E2E tests, add new files to **`tests/e2e/specs/`**. **`tests/e2e/specs/example.test.js`** in this repository may be used as a starting point. Tests can also be organized in folders under the `specs` directory. For example: **`tests/e2e/specs/{folder-name}/example.test.js`**.

- Files with the extensions **`.test.js`** or **`.spec.js`** are run automatically by `npm run test:e2e`.
- Adhoc tests can be created with the **`.js`** and executed with `npm run test:e2e tests/e2e/specs/test-example.js`.

### Relevant files

* **Docker Initialization Script** - `tests/e2e/docker/initialize.sh`. This can be used to set up your testing environment such as installing additional plugins via WP-CLI / importing data / running additional scripts. Reference - https://github.com/woocommerce/woocommerce/blob/master/tests/e2e/docker/initialize.sh
* **Jest Setup Script** - `tests/e2e/config/jest.setup.js`. Can be used to define custom scripts for Jest. You can also make use of the example script **`tests/e2e/config/jest.setup.example.js`** which requires **`@wordpress/e2e-test-utils`** package to be installed to run the defined functions. Please note that once the default template is enabled, all posts and products will be trashed, and local storage will be cleared between tests.
* **Test Variables** - To override the default test variables, create a new JSON file at **`tests/e2e/config/default.json`**. Reference - https://github.com/woocommerce/woocommerce/blob/master/tests/e2e/env/config/default.json.

### Important References

* **WooCommerce E2E Tests** - https://github.com/woocommerce/woocommerce/blob/master/tests/e2e/README.md
* **WooCommerce E2E Environment** - https://www.npmjs.com/package/@woocommerce/e2e-environment
* **WooCommerce E2E Core Tests** - https://www.npmjs.com/package/@woocommerce/e2e-core-tests
* **WordPress E2E Test Utils** - https://www.npmjs.com/package/@wordpress/e2e-test-utils

### Changes made to the original WooCommerce E2E Test suite

* Removed the folders `tests/e2e/api`, `tests/e2e/core-tests`, `tests/e2e/utils`, `tests/e2e/env` since these are standalone packages.
* Removed the folder `tests/e2e/bin` (build scripts for E2E Components)
* Removed `tests/e2e/docker/init-wp-beta.sh`
* Renamed `tests/e2e/config/jest.setup.js` to `tests/e2e/config/jest.setup.example.js`.
* Modified `tests/e2e/docker/initialize.sh` to include additional WooCommerce store setup.
