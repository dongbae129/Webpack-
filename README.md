-목차-
1. webpack 소개


2. webpack.config.js(설정파일) 구성요소


3. 자주 사용하는 loader, plugin

4. polyfill


1.1 webpack이란? 

한마디로 말하면 webpack은 모듈 번들링이다.
주로 javascript에 대해서 번들링을 처리하며, 해당 동작을 설명하자면
여러개의 javascript 파일을 하나의 javascript 파일로 만들어주는 것을 말한다.

![K-003](https://user-images.githubusercontent.com/36911316/112828786-234de380-90cb-11eb-9137-dd4f88224056.png)


1.2 사용이유

  * 모듈 시스템의 필요성 : 여러개의 js파일을 분리시킬수 있으며 이를 통해 가독성과 유지보수 효율성 증가
  * 네트워크 병목현상 : 하나의 html에서 여러개의 javascript 파일을 로드할 경우, 네트워크에 병목현상이 발생할수 있다. 그렇기 때문에 여러 js파일들을 모듈로서 분리시키고 번들링으로서 하나     의 js파일을 만들어 이를 최소화 할수 있다.
  * 브라우저에 상관없이 : 나중에 나올 babel을 이용하여 es6파일을 es5로 변환시켜 브라우저와 상관없이 최신 javascript 사용가능




2. webpack 설정파일(webpack.config.js)

  const path = require("path")

  module.exports = {
    mode: "development",
    entry: {
      main: "./src/app.js",
    },
    output: {
      filename: "[name].js",
      path: path.resolve("./dist"),
    },
    module: {
    
    },
    plugins: [
    
    ],
  }

webpack 설정파일에서 알아야할것은 mode, entry, output, module, plugins 총 4개이다.












