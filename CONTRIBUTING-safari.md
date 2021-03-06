# Contributor guidelines

Thinking about contributing to RES? Awesome! We just ask that you follow a few simple guidelines:

1. To build the extension, see [Building development versions of the extension](#building-development-versions-of-the-extension).

1. To add new modules or hosts, see [Adding new files](#adding-new-files).

1. To check code style and autofix some formatting errors, see [Lint and test commands](#lint-and-test-commands).

## Building development versions of the extension

#### First time installation

1. Install [git](https://git-scm.com/).
1. Install [node.js](https://nodejs.org) (version >= 8).
1. [Clone this repository](https://help.github.com/articles/cloning-a-repository/).
1. Run `npm install` in that folder.

Once done, you can build the extension by running `npm start` (see [Build commands](#build-commands)).

To load the extension into your browser, see [Loading RES into your browser](#loading-res-into-your-browser).

#### Build commands

**`npm start [safari]`** will clean `dist/`, then build RES (dev mode), and start a watch task that will rebuild RES when you make changes. Only changed files will be rebuilt.

**`npm run once [safari]`** will clean `dist/`, then build RES (dev mode) a single time.

**`npm run build [safari]`** will clean `dist/`, then build RES (release mode). Each build output will be compressed to a .zip file in `dist/zip/`.

#### Lint and test commands

**`npm run lint`** will verify the code style (and point out any errors) of all `.js` files in `lib/` (except `lib/vendor/`) using [ESLint](http://eslint.org/), as well as all `.scss` files with [sass-lint](https://github.com/sasstools/sass-lint).

**`npm run lint-fix`** will autofix any [fixable](http://eslint.org/docs/user-guide/command-line-interface#fix) lint issues.

**`npm run flow`** will run [Flow](https://flowtype.org/) type checking, and start the Flow server so future runs will complete faster. Use `npm run flow -- stop` to stop the server, or `npm run flow -- check` to run Flow once without starting the server.

**`npm test`** will run unit tests (in `__tests__` directories) using [Ava](https://github.com/avajs/ava).

**`npm run integration -- safari [-f <testFileGlob>]`** will run integration tests (in `tests/`) using [Nightwatch.js](http://nightwatchjs.org/).

To run integration tests locally, you need to run an instance of [Selenium Standalone Server](http://www.seleniumhq.org/download/) and have either [ChromeDriver](https://sites.google.com/a/chromium.org/chromedriver/home) or [GeckoDriver](https://github.com/mozilla/geckodriver) on your `PATH`.
The [`selenium-standalone`](https://www.npmjs.com/package/selenium-standalone) package may help with this.
The default host and port (`localhost` and `4444`) should work for most local installations, but if necessary they can be overridden with the `SELENIUM_HOST` and `SELENIUM_PORT` environment variables.

#### Loading RES into your browser

##### Safari

1. WIP

## Project structure

#### Top level files and folders

  - `.github/`: Github templates
  - `browser/`: extension API files common to all browsers
  - `build/`: files handling automated browser deployments
  - `changelog/`: release changelogs
  - `chrome/`: Chrome-specific RES files
  - `dist/`: build output
  - `edge/`: Microsoft Edge-specific RES files
  - `examples/`: example code for new hosts/modules
  - `firefox/`: Firefox-specific RES files
  - `images/`: images for RES logo and CSS icons
  - `lib/`: all RES code
  - `lib/core/`: core RES code
  - `lib/css/`: RES css
  - `lib/environment/`: RES environment code
  - `lib/images/`: RES images
  - `lib/modules/`: RES modules
  - `lib/templates/`: RES templates
  - `lib/utils/`: RES utilities
  - `lib/vendor/`: RES vendor libraries (old libs not on npm)
  - `lib/**/__tests__`: unit tests
  - `locales`: RES i18n translations
  - `safari/`: Safari-specfic RES files
  - `tests/`: integration tests
  - `package.json`: package info, dependencies
  - `webpack.config.babel.js`: build script

## Adding new files

#### Modules

Create a new `.js` file in `lib/modules`.
It will automatically be loaded when the build script is restarted.

All user-visible text must be translated. See the [locales README](/locales/locales/README.md) for details.

#### Media hosts

See [`examples/host.js`](https://github.com/honestbleeps/Reddit-Enhancement-Suite/blob/master/examples/host.js) for an example.

Create a new `.js` file in `lib/modules/hosts`.
It will automatically be loaded when the build script is restarted.

If the host uses an API that does not support [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), you must add it to the browsers' manifests and the host's `permissions` property. For example, search for usages of `api.twitter.com`.

#### Stylesheets

Create a new `.scss` file in `lib/css/modules/` (with a leading underscore, e.g. `_myModule.scss`).
Import the file in `lib/css/res.scss` (e.g. `@import 'modules/myPartial';`).

For toggleable CSS, add `bodyClass: true` to an option or module, then wrap your CSS with `.res-moduleId-optionKey` (boolean options), `.res-moduleId-optionKey-optionValue` (enum options), or `.res-moduleId` (modules).

For example:
```scss
.res-showImages-hideImages {
	img {
		display: none;
	}
}
```
