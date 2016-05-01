
# start make run

# 1. Check node version and npm version

# 2. bundle-development.js 생성
## 2.1. build-****
	- build
	- build-$(CALYPSO_ENV)
	- build-development
		- build-server client/config/index.js server/dev/docs/search-index.js build-css
	- build-server
		- @CALYPSO_ENV=$(CALYPSO_ENV) $(NODE_BIN)/webpack --display-error-details --config webpack.config.node.js

# 2.2 css 파일 생성
	- style.css
	- style-rtl.css

# 3. wp-calypso/index.js 실행

# 4. Root index.js file.
	- boot

# 5. boot 모듈에서 express middleware setup
	- build
	- bundler
	- devdocs
	- api
	- pages
	- static : assets, client, desktop, public

## 5.1 build
Tiny Express.js middleware that spawns `make build-css --jobs n` from the application root directory for every request to regenerate the CSS.

Returns a "build middleware", which runs `make build-css` upon each HTTP request. Meant for use in "development" env.

- setup make build-css

## 5.2 bundler
This module contains the code for generating the JavaScript files that are sent to the browser. The code leverages [Webpack](http://webpack.github.io/), [uglifyjs](http://lisperator.net/uglifyjs/), and the sections module at `client/sections.js` that defines the various sections of the application.

- README.md 참고 Webpack으로 여러가지 작업을 처리함

## 5.3 devdocs

-	In-app documentation browser (server)

Devdocs is a built-in search engine that allows developers and users to browse and search Calypso’s developer documentation.

## 5.4 api

List of supported endpoints:

* `/version` - it serves version fetched from *package.json*.
* `/oauth` - proxies an oauth login to the WP.com API. Only enabled with the `oauth` feature
* `/logout` - removes the wpcom_token cookie and redirects to the logout_url

## 5.5 pages

### use middleware `isomorphic-routing`
This module renders the main HTML document and handles appending a query string to `build.js` and `style.css` that changes when those files are modified.
Like Cloudup, we are using [jade](http://jade-lang.com/) as the templating language, although at some point, we may switch to using React server-side.

## 5.6 static