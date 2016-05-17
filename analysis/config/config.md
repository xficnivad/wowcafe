Config
======

This dir is used to store `.json` config files. At boot-up time, the server decides which config file to use based on the `NODE_ENV` environment variable. The default value is `"development"`. The entire configuration is available on the server-side and certain keys are also exposed to the client.

config 디렉토리는 '.json' 설정파일을 저장하는데 사용된다. 실행 타임에 서버는 NODE_ENV 환경변수에 따라 어떤 설정파일을 사용할지 결정한다. 기본 값은 `development`다. 전체 설정이 서버 측에 제공되며 특정한 키는 클라이언트에 노출된다.

If it is necessary to access a `config` value on the client-side, add the property name to the `client.json` file, which is whitelist of config properties that will be exposed to the client.

이 클라이언트 측에`config` 값에 액세스 할 필요가있는 경우, 클라이언트에 노출되는 구성 속성의 화이트리스트 인`client.json` 파일에 속성 이름을 추가합니다.

Server-side and client-side code can retrieve a config value by invoking the `config()` exported function with the desired key name:
서버 측과 클라이언트 측 코드는 원하는 키 이름으로 'config()'  함수를 호츨하여 설정 값을 검색 할 수 있다.

```js
var config = require( 'config' );
console.log( config( 'redirect_uri' ) );
```

Feature Flags
-------------

The config files contain a features object that can be used to determine whether to enable a feature for certain environments. This allows us to merge in-progress features without launching them to production. The config module adds two methods to detect this: `config.isEnabled()` and `config.anyEnabled()`. Please make sure to add new feature flags alphabetically so they are easy to find.

설정 파일은 특정 환경을 위한 기능을 사용할지 여부를 결정하는데 사용될 수있는 feature 오브젝트를 포함한다. 이것은 production을 시작하지 않고 진행중인 features 를 병합할 수 있다. config 모듈이 이를 감지하는 두가지 방법을 추가 : `config.isEnabled()`와 `config.anyEnabled()`. 그들은 쉽게 찾을 수 있도록 알파벳순으로 새로운 기능 플래그를 추가 할 수 있는지 확인하십시오.

```json
{
	"features": {
		"manage/posts": true,
		"reader": true
	}
}
```

If you want to temporarily enable/disable some feature flags for a given build, you can do so by setting the `ENABLE_FEATURES` and/or `DISABLE_FEATURES` environment variables. Set them to a comma separated list of features you want to enable/disable, respectively:

일시적으로 특정 빌드에 대한 몇 가지 기능 플래그 enable/disable 할 경우 `ENABLE_FEATURES` and/or `DISABLE_FEATURES` 환경 변수를 설정하여 수행 할 수 있습니다. 당신은 각각 enable/disable 할 기능을 쉼표로 구분 된 목록으로 설정합니다 :

`bash
ENABLE_FEATURES=manage/plugins/compatibility-warning DISABLE_FEATURES=code-splitting,reader make run
`

This will generate the appropriate `client/config/index.js` file. Afterwards, when you run `make run` without setting these variables, `client/config/index.js` will regenerate according to the selected config file - making this method ideal for quickly reviewing PRs or reproducing bugs.

이것은 적절한 `client/config/index.js` 파일을 생성합니다.
이러한 변수를 설정하지 않고 "make run"을 실행하면 선택한 설정 파일에 따라 "client/config/index.js" 재생성 됩니다 - 빠르게의 PR을 검토하거나 버그를 재생하는이 방법 이상적이다.
