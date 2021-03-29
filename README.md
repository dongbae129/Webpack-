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
      
      













