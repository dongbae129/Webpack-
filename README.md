-목차-
1. webpack 소개


2. webpack.config.js(설정파일) 구성요소


3. 자주 사용하는 loader, plugin

4. Babel


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


const path = require("path");

module.exports = {

  mode: "development",
  
  entry: {
  
    main: "./src/app.js",    
  },
  
  output: {
  
    filename: "[name].js",    
    path: path.resolve("./dist"),    
  },  
  module: {},  
  plugins: [],  
}

webpack.config.js에서 알아야할 4가지는 mode, entry, output, module, plugins 이다.

2.1 mode :  개발모드, 배포모드를 결정할수 있으며 mode: "development" 또는 mode: "production"으로 사용할수 있다.

2.2 entry : 어플리케이션의 진입점이며, 무슨 파일을 실행시킬지를 작성한다.

2.3 output : 결과물이다. filename: 무슨이름으로 만들어질지, path: 어느 위치에 생성할지 결정한다.

2.4 module : module들의 규칙을 결정한다. 여기서 js파일의 규칙, css파일의 규칙, 이미지파일의 규칙 등등을 결정할수 있다.


    module: {
     rules: [
       {
         test: /\.css$/,
         use: ["style-loader", "css-loader"],
       },
       {
         test: /\.(png|jpg)$/,
         loader: "file-loader",
         options: {
         publicPath: "./dist/",
         name: "[name].[ext]?[hash]",
        },
     ],
  },
  
css파일에 대해서는 css-loader, style-loader를 순으로 적용을 시키며,
png, jpg 파일에 대해서는 file-loader를 적용시키며 생성위치는 ./dist/ 생성이름은 파일이름.확장자 이다.

2.5 plugins : loader와 다르게 bundle파일에 대해 처리하며 class 형태이기 때문에 new연산자를 이용하여 생성한다.


  const { CleanWebpackPlugin } = require("clean-webpack-plugin"); 
  
  
  module.exports = {
  
    plugins: [new CleanWebpackPlugin()],
  }
  


3. 자주사용하는 loader, plugins


3.1
    
      css-loader : css파일을 js파일에서 불러와 사용하기 위하여 필요하다.(css사용하기 위하여)
  
      style-loader : 스타일시트를 DOM에 추가시켜 브라우저가 해석할수 있도록 해준다.(css 적용하기 위하여)
      
      file-loader : css와 마찬가지로 파일을 모듈화 시키기 위하여, output에 해당파일을 복사하여 위치시켜놓는다
      
      url-loader : 네트워크 자원소비를 낮추기 위하여 크기가 작은 파일에 대해 Data URI Schema방식을 이용하여 이미지를 Base64 인코딩을
      
      통하여 문자열 형태로 javascript 코드에 삽입한다.
      
3.2
  
      BannerPlugin : bundle 파일에 banner를 등록할수 있다.(주로 commit 버전, author, build정보를 등록한다.)
  
      HtmlWebpackPlugin : html파일을 후처리 하기 위하여 사용한다.
      
      CleanWebpackPlugin : build 할때 refresh를 위하여 사용한다. output폴더가 삭제후 재생성 되기때문에 build 이전값을 자동으로 관리해준다.
      
      MiniCssExtractPlugin : 스타일시트가 점점 많아져 하나의 자바스크립트 bundle로 만들기 부담스러울때 사용한다. 번들 결과에서 스트일시트 코드만 뽑아서 별도의 CSS 파일로 만든다.
      
      

4. Babel

 4.1 babel이란?
 
 babel은 javascript 컴파일러 이다. 말 그대로 소스 대 소스 컴파일러로서 결과물을 javascript로 바꿔준다. 그렇다면 왜 babel을 쓰는걸까?
 
 javascript코드를 현재 및 과거의 브라우저 환경에서 호환되는 버전으로 변경시키기 위하여 사용된다.
 
 babel은 파싱, 변환, 출력 3단계로 이루어지며 파싱(를 하나의 토큰으로 분리시켜), 변환(브라우저 환경에 맞게 코드를 변환), 출력(변환된 코드로 출력) 한다.
 
 babel은 파싱, 출력만 담당하며 plugin이 변환을 담당한다.
 
 webpackp에서 설명했듯이 babel도 동일하게 babel.config.js 처럼 설정파일을 사용한다.
 
 module.exports =  {
 
  plugins: [
  
    "@babel/plugin-transform-block-scoping",
    "@babel/plugin-transform-arrow-functions",
    "@babel/plugin-transform-strict-mode",
  ],
}
 
plugins 배열안에 사용할 plugin을 넣으면 된다. 하지만 일일이 플러그인을 설정하는 행위는 매우 귀찮으므로 이런것들을 세트로 모아놓은 preset이 존재한다.

module.exports = {

  presets: [
  
    [
	
      "@babel/preset-env",
	  
      {
        targets: {
          chrome: "79",
          ie: "11", 
        },
      },
    ],
  ],
}

"@babel/preset-env"는 기본적으로 필요한 plugins을 모아놓은 세트이다. presets배열 안에 자신이 사용할 preset을 넣고 위 코드와 같이 설정을 추가하고 싶다면 사용할 preset을

배열로 변경하고 객체로 설정을 넣으면된다. 위와같이 ie11, chrome79까지 돌아가게 하려면 target객체를 만들어 넣으면 된다.


4.2 폴리필(polyfill)

폴리필은 최신 ECMAScript 환경을 만들기 위해 코드가 실행되는 환경에 존재하지 않는 빌트인, 메소드 등을 추가하는 역할을 한다.
예를들어 Promise는 일반적으로 babel을 돌려도 코드를 변환시키지 않는다. 빌트인, 메소드에 존재하지 않기 때문이다.
하지만 대체할수 없을뿐이지 구현할수는 있다.

module.exports = {

  presets: [
  
    [
      "@babel/preset-env",
      {
        useBuiltIns: "usage", // 폴리필 사용 방식 지정
        corejs: {
          // 폴리필 버전 지정
          version: 2,
        },
      },
    ],
  ],
}
		
useBuiltIns는 "usage" , "entry", false 세가지를 사용할수 있으며 default는 false이기 때문에 기본적으로 폴리필이 작동하지 않았던 것이다.
usage나 entry를 설정하면 폴리필 패키지 중 core-js를 모듈로 가져와 Promise를 다른 브라우저에서 동작할수 있도록 적절하게 구현시켜준다.

위 설정파일로 컴파일 할시 

require("core-js/modules/es6.promise");
require("core-js/modules/es6.object.to-string");
에러가 발생하며 이는 Promise를 구현하기 위하여 2가지를 require했다는 뜻이다. 그러나 우리는 위의 두가지를 아직 설치하지 않았기 때문에 에러가 뜨는것 뿐이다.
그러므로 npm으로 설치만 해주면 된다. npm i core-js@2

마지막으로 babel을 이렇게 일일이 실행시키지 않는다.
간단하게 webpack에서 loader로서 js파일에 대해 babel-loader를 적용시켜주면 된다.


//webpack.config.js

module.exports = {

  module: {
  
    rules: [
	
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: "babel-loader",
      },
    ],
  },
}
		
exclude는 말그대로 제외할 목록인데 모든파일에 대해 해당 loader를 적용시지 말고 node_modules밑에 있는 파일들은 제외하고 적용시킨다.

		











