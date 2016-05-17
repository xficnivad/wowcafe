# Entry Point

## wpcalypso/index.js

1. Load boot module.
2.

## boot/
## server/pages/index.jade

##

find client
find assets

find package.json

bin/check-node-version

ln -s /bin/pre-commit  => .git/hooks/pre-commit
ln -s /bin/pre-push    => .git/hooks/pre-push

npm install

task build-server : webpack.config.node.js를 이용해서 bundle-$(ENV).js를 생성한다. (index/, server, client 코드를 webpack으로 묶는다.)
	mkdir -p build
	$(NODE_BIN)/webpack --display-error-details --config webpack.config.node.js

		entry : index.js
		resolve : /server, /client
		output : build/bundle-development.js and source map

webpack => /index.js : create http server
	require server/boot module
		boot is express app.
	require package.json
	require wp-calypso/config
		This dir is used to store `.json` config files. At boot-up time, the server decides which config file to use based on the `NODE_ENV` environment variable. The default value is `"development"`. The entire configuration is available on the server-side and certain keys are also exposed to the client.
		This will generate the appropriate `client/config/index.js` file.

/server/boot/index.js
	> attach /server/build module : make build-css
	> import /server/config module  : read /config directory and parsing to json.
	> attach the /server/pages module  : express server side *.jade
		require /client/lib/i18n-utils
		require /server/sanitize
		require /client/sections.js
			my-sites/theme
			my-sites/themes
			state/ui/actions
		require /server/isomorphic-routing, /server/isomorphic-routing/loader
			require /client/lib/mixins/i18n
			require /client/state
			require /client/controller.js
				require /client/state/ui/actions.js
				require /client/layout/logged-out
				require /client/state/current-user/selectors.js
					require /client/state/users/selectors.js
		require /server/render
		require /server/user-bootstrap
			require /server/user-bootstrap/sahred-utils.js => lib/user/shared-utils

	> create /server/bundler module :  bundler( app )
		This module contains the code for generating the JavaScript files that are sent to the browser. The code leverages [Webpack](http://webpack.github.io/), [uglifyjs](http://lisperator.net/uglifyjs/), and the sections module at `client/sections.js` that defines the various sections of the application.
		> require /server/bundler/utils.js
	> attach the /server/devdocs modules :
		Devdocs is a built-in search engine that allows developers and users to browse and search Calypso’s developer documentation.
	> attach the /server/api module
		set /server/api/oauth module.
	>
