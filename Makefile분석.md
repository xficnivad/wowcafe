# calyoso Makefile 분석

## Git Hooks

git hooks에 대한 좀 더 자세한 내용은 [Git맞춤 - Git Hooks](1) 참고해라.

### githooks-commit
> bin/pre-commit을 .git/hooks/pre-commit으로 symbolic link를 한다.<br>
js와 jsx파일이 변경되었으면, eslint를 실행해서 문법 오류 검사를 하고 실패했을 때는 
Commit이 되지 않도록 한다.

### githooks-push
> bin/pre-push를 .git/hooks/pre-push로 symbolic link를 한다.<br>
현재 branch가 master가 아니면 push하는 것이 정말 맞는 지 y/n으로 확인한다.

## install

### node_modules

node_modules는 package.json이 변경됐을 때 실행된다.

### node_modules/%

node_modules 중 전체가 아닌 개별 module만 install하도록 한다.
각 module의 이름만 추출하는 shell function인 $(notdir $@)를 이용한다.

### eslint or lint

lint관련 module을 설치한 후에 npm run lint를 실행하여 문법 오류 검사를 실행한다.

## 변수

- CALYPSO_ENV : development / stage / production / wpcalypso 등

## Node 모듈

- [Autoprefixer](2) : CSS에 vendor prefix를 추가해주는 기능 
- [RTLCSS](3) : 일반적인 LTR CSS를 RTL로 변환하는 기능
- [semver](4) : The semantic versioner for npm - 문자열 버전 비교하는 기능

### FILES

## SASS_FILES

client, assets 폴더 내에 있는 *.scss 파일들

## JS_FILES

eslint를 위한 js, jsx 파일들 

## MD_FILES

문서를 위한 md 파일들

## 실행 스크립트

- BUNDLER
- RECORD_ENV
- GET_I18N
- LIST_ASSETS
- ALL_DEVDOCS_JS

## 생성되는 파일

- .env 파일 : 최근에 빌드한 CALYPSO_ENV의 값을 저장하고 있다.
- client/config/index.js : CALYPSO_ENV에 맞게 이 파일이 생성된다.


[1]:https://git-scm.com/book/ko/v2/Git%EB%A7%9E%EC%B6%A4-Git-Hooks
[2]:https://github.com/postcss/autoprefixer
[3]:https://github.com/MohammadYounes/rtlcss
[4]:https://github.com/npm/node-semver