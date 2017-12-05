r## Sass ?
CSS pre-processor로써, 코드의 재활용성 및 코드의 가독성을 높여 유지보수를 쉽게 하도록 돕는다.

## CSS pre-processor란 ?
Compiler를 통해 브라우저에서 사용할 수 있는 일반 CSS 문법으로 변환하여 준다. 

## Sass Compiler 

- Ruby Sass
오리지널로써 설치 및 컴파일은 다음과 같이 진행한다.
~~~
$ gem install sass // 설치
$ sass style.scss style.css // 컴파일
~~~

- libsass <br>
C 언어로 된 컴파일러로써 성능이 Ruby Sass에 비하여 우수하다. 하지만, libsass 는 Ruby Sass의 기능을 100%지원하지 않기 때문에 Ruby Sass의 기능이 업데이트 될때까지 기다려야 한다. 자세히는 [sass-compatibility](http://sass-compatibility.github.io/)를 확인하면 된다.

설치 및 컴파일은 다음과 같다.
~~~
$ npm install node-sass // 설치

$ node-sass style.scss -o // 컴파일

$ node-sass style.scss -w -o // 컴파일 및 수정시 자동으로 리컴파일
~~~

## Sass와 Scss의 차이
Sass와 Scss는 버전 및 문법의 차이가 있으며 버전 3부터 .scss로 css의 상위집합에 속하는 문법을 사용하여 작성한다. 

## 개발환경설정

### node-sass 설치
~~~
$ npm install node-sass --save-dev
~~~

위와 같이 node-sass를 설치하고 project root directory에서 node-sass 컴파일 명령어를 실행하면 command를 찾을 수 없다는 메시지와 함께 아무런 동작도 일어나지 않는다. 

### nodemon 설치
nodemon은 scss file 의 수정을 감시하기 위해서 사용하는 모듈이다. 
scss가 변경되면 css로 변경사항을 곧바로 compile하여 반영한다. 
~~~
$ npm install nodemon -D
~~~
-D 플래그는 package.json의 devDependencies에 write 하라는 의미로써, --save-dev 옵션과 동일

### folder 생성
~~~
$ mkdir public scss && public/css
~~~

### npm script 설정
package.json에서 "scripts" 프로퍼티를 찾아서 다음의 코드를 작성하라

~~~
“scripts”: {
  “build-css”: “node-sass --include-path scss scss/main.scss   public/css/main.css”
},
~~~

그리고 command-line에서 다음과 같이 실행하면 public/css에 main.css 파일이 생성된다. 

~~~
$ npm run build-css
~~~

## Watch Sass
package.json의 "scripts" 객체에 다음 프로퍼티를 추가한다. 
-e(or --ext)는 nodemon이 찾고자 하는 file extension을 의미하며 아래에서는 scss를 의미한다. 
~~~
“watch-css”: “nodemon -e scss -x \”npm run build-css\””
~~~

그리고 다음의 명령어를 실행하면 nodemon이 실행되어 scss파일의 변화를 감지하여 자동으로 compile을 수행하여 css 파일을 생성한다. 

~~~
$ npm run watch-css
~~~

h## npm script 파일 생성
npm script의 내용을 개별 file을 생성하여 만들어 보자 

다음과 같은 명령어로 파일을 생성 한 후
~~~
$ touch bin/build-css bin/watch-css
~~~

build-css, watch-css 의 파일 내용을 다음과 같이 바꾼다. 
file로 script를 쓰면, 따옴표를 생략하여 표현할 수 있다. 

~~~
node-sass --include-path scss scss/main.scss public/css/main.css
nodemon -e scss -x npm run build-css
~~~

위와같이 만들어진 파일을 excutable하게 만들기 위해 다음의 명령어를 실행한다. 
~~~
chmod +x bin/build-css
chmod +x bin/watch-css
~~~

npm script의 내용을 아래와 같이 바꾼다. 
~~~
“scripts”: {
 “build-css”: “./bin/build-css”,
 “watch-css”: “./bin/watch-css”
}
~~~

## 참고자료
- [Velopert Sass 강좌](https://velopert.com/1712)
- [Watch & Compile your Sass with npm](https://medium.com/@brianhan/watch-compile-your-sass-with-npm-9ba2b878415b)