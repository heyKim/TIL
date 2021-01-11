# Webpack
 * [Output](#output)
 * [Optimization](#optimization(번들-쪼개기))
 * [Webpack Plugin](#webpack-plugin)
 * [environment variables 설정](#environment-variables-설정)
 * [Code Spliting](#code-spliting)
 * [Change relative paths to absolute paths](Change-relative-paths-to-absolute-paths)

## Output
### output.filename substitutions
* webpack에서 output 파일명을 치환하는 셋팅을 제공한다.
* 왜 필요할까? 불필요한 트래픽을 줄이기 위한 caching을 위해서!
* hasing 종류
  * `[hash]` is a "unique hash generated for every build"
  * `[chunkhash]` is "based on each chunks' content"
  * `[contenthash]` is "generated for extracted content"
* Ex)
`webpack.config.js`:
```js
entry: {
  index: ["./src/index.js", "./src/index.css"],
  vendors: ["./src/vendors.js"]
},
output: {
  filename: "[name].[hash].js"
}
plugins: [
  new MiniCssExtractPlugin({
    filename: "[name].[hash].css"
  })
]
```
이렇게 선언하면 Chunk가 `index`와 `vendors` 두 개가 생긴다.  
`css`파일의 경우 `MiniCssExtractPlugin`를 이용해 빌드될 때 seperate files로 export 된다. 
물론 webpack입장에서 `index.js`와 `index.css`는 같은 chunk라고 인식된다.

`[hash]`를 사용하는 경우:  
 ![webpack_output_hash](https://github.com/heyKim/myMemo/blob/master/images/react/webpack_output_hash.png)  
 모든 파일은 같은 hash를 가짐(작성한 source기반으로 hash가 생성되기 때문에)  
 소스 변경없이 re-build하면 outputfile의 hash값은 그대로, 하지만 단 하나의 파일만 수정해도 모든 bundle파일의 hash명이 변경됨.  
 
`[chunkhash]`를 사용하는 경우:  
 ![webpack_output_hash](https://github.com/heyKim/myMemo/blob/master/images/react/webpack_output_chunkhash.png)
 chunkhash는 주어진 chunk기반으로 hashing을 한다.  
 `index.css`을 수정하면 같은 chunk인 `index.js`도 영향을 받아 `index` chunk는 새로운 hash값이 부여된다.
 `vendors` chunk는 기존과 동일한 hash값을 가진다.

`[contenthash]`를 사용하는 경우:  
 ![webpack_output_hash](https://github.com/heyKim/myMemo/blob/master/images/react/webpack_output_contenthash.png)  
 모든 bundle된 파일이 unique한 hash값을 가진다.(각 파일의 content를 기반으로 hash가 생성되기 때문)
 `index.css`을 수정하면 `index.css`파일만 새로운 hash값을 부여받는다.  
 
-----------------------------

## Optimization(번들 쪼개기)
여러 청크들 간에 공통적으로 사용되는 모듈들을 따로 하나로 모아두는 plugin인 CommonsChunkPlugin이 웹팩4에서 optimize 속성의 splitChunks로 이전됨  청크간에 겹치는 패키지들을 별도의 파일로 추출해주는 코드, `vendor`를 만들 때 유용  
 => A 청크가 (a, b, c) 패키지를 가지고 있고, B 청크가 (a, b, d) 패키지를 가지고 있다면, a와 b 패키지가 겹치기 때문에 두 번 불러와서 쓸데없는 용량을 잡아먹는다. 이런 것은 vendor~A~B (a, b)로 만들어주고, A 청크는 (c), B 청크는 (d)로 만들어 중복을 최소화해줌  
 ```
   optimization: {
      /** 번들 쪼개기 설정 */
      splitChunks: {
        /** 한 bundle당 최대 사이즈(byte단위) */
        // maxSize: 300000, //300kb, 약 292kib
        cacheGroups: {
          /** node_modules 폴더를 vendors.bundle.js로 만듬 */
          node_vendors: {
            test: /[\\/]node_modules[\\/]/,
            name: 'vendors',
            chunks: 'all' //values: all/initial/async
          },
          /** public 폴더를 images.bundle.js로 만듬 */
          public_images: {
            test: /[\\/]public[\\/]/,
            name: 'images',
            chunks: 'all'
          }
        }
      }
    },
 ```
 * chunks 옵션
  1) `async`: dynamic import된 module만 optimization  
  2) `initial`: dynamic import이든 non-dynamic import이든 optimization해. 단, non-dynamic import module은 공유할수 있어도 단일 bundle로 만듬
  3) `all`: 아 몰랑 dynamic이든 non-dynamic 이든 알아서 해죠. 그정돈 할 수 있잖아?^_^  
  ※ 자세한 내용은 [블로그](https://medium.com/dailyjs/webpack-4-splitchunks-plugin-d9fbbe091fd0) 참고
 
## Webpack Plugin
* `DefinePlugin`: 컴파일할 코드에서 특정 문자열을 설정한 값으로 치환해주는 기능을 함.
* `UglifyJsPlugin`: 컴파일한 결과 코드의 용량을 줄이고 읽기 어렵게 만드는 역할을 한다.
 => webpack4 부터는 optimization의 minimize로 대체됨.
* `ExtractTextPlugin`: css를 app.bundle.css와 같이 단일 파일로 만들어줌. html 파일에서 `<style>` 태그로 include안해도됨.
* `EnvironmentPlugin`: DefinePlugin on process.env keys을 사용하는 간단한 방법(?)
-----------------------------
### environment variables 설정
문제: 중요한 정보들(DB password, API KEY)이 github에 올라가면 안됨.
해결: environment variables를 선언하고 사용한다.

#### 방법1. Using npm scripts to set environment variables
webpack과 webpack-cli를 설치하고 ```package.json```파일의 ```script```부분에 아래와 비슷한 코드가 있을 것임.
```
{
  // package.json 내용(이하 생략)
  scripts: {
    "dev": "webpack --config webpack.config.dev.js",
    "build": "webpack --config webpack.config.build.js"
  }
}
```
여기에 ```--env``` flag를 사용하여 환경변수 설정
```
{
  // the rest of your package.json
  scripts: {
    "dev": "webpack --env.API_URL=http://localhost:8000 --config webpack.config.dev.js",
    "build": "webpack --env.API_URL=https://www.myapi.com --config webpack.config.build.js"
  }
}
```
이렇게 선언해두면 Component에서 ```process.env.API_URL```로 호출해서 사용가능하다.
```
const App = () => <h1>{process.env.API_URL}</h1>;
```
근데 이 방법은 코드를 compile할 때 placeholders로서 환경변수를 전달하는 것이기 때문에 비추천함

#### 방법2. `.env` file + `dotenv` + `DefinePlugin`
* ```.env```파일 생성
```API_URL=http://localhost:8000```

* Handle the .env file
유명한 ```dotenv``` npm package로 .env 파일을 다룰수 있음.(CRA에서 사용됨)
```$ npm install --save-dev dotenv```
그러나 dotenv는 server-side에서만 동작하기 때문에 client-side쪽에서 이용하기 위해서는 추가 설정이 필요

* ```DefinePlugin``` 플러그인 설치 및 설정
```
const webpack = require('webpack');
const dotenv = require('dotenv');

module.exports = () => {
  // call dotenv and
  //config() will read your .env file, parse the contents, assign it to process.env, and return an Object with a parsed key 
  const env = dotenv.config().parsed;
  
  // reduce it to a nice object, the same as before
  const envKeys = Object.keys(env).reduce((prev, next) => {
    prev[`process.env.${next}`] = JSON.stringify(env[next]);
    return prev;
  }, {});

  return {
    plugins: [
      new webpack.DefinePlugin(envKeys)
    ]
  };
 ```
 ```require('dotenv').config();``` 와 같이 ```dotenv```의 ```.config```를 호출하면 ```.env``` 파일에서 셋팅한 environment variables이 담긴 Object를 return 한다.   
아래와 같이 호출 가능
 ```const App = () => <h1>{process.env.API_URL}</h1>;```
 
 * 참고사이트: https://medium.com/@trekinbami/using-environment-variables-in-react-6b0a99d83cf5

#### 방법3. `.env` file + `dotenv-webpack`
* 사실 `dotenv-webpack`은 `dotenv`과 `Webpack.DefinePlugin`를 wrapping한 것
* https://github.com/mrsteele/dotenv-webpack
* `.env`:
```
 API_KEY=test1111
```
* `webpack.config.js`:
```
const Dotenv = require('dotenv-webpack');
module.exports = {
 /** 내용 생략 */
 plugins: [
    new Dotenv({
        path: './.env', // Path to .env file (this is the default)
        safe: false // load .env.example (defaults to "false" which does not use dotenv-safe)
    }),
],
```

#### 방법4. EnvironmentPlugin
* EnvironmentPlugin은 process.env keys에 DefinePlugin를 사용한 것과 같다.
* ```new webpack.EnvironmentPlugin(['NODE_ENV', 'DEBUG']);```는 아래의 소스와 같다.
```
 new webpack.DefinePlugin({
   'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV),
   'process.env.DEBUG': JSON.stringify(process.env.DEBUG)
 });
```

### 어떤 방법이 제일 좋을까?
* dotenv의 document에 의하면, .env파일에서 내용을 읽어 parsing 해주는 config method를 사용하지 말라고 한다.
* webpack을 사용한다면 `dotenv-webpack` package를 추천

-----------------------------
### Container안에서 App을 실행할때 .env설정을 어떻게 가져오지?
#### 방법1. docker Image를 만들때 Pass variables(2가지 방법)
* using the option -e
`$ sudo docker run  [...] -e my_connection_string="xxxxx" -e my_password="xxx" [...]`
* using env-file docker option
`my_env.list`:
```
my_connection_string=xxxxxx
my_password=yyyyyyyy
my_secret=zzzzz
```
`$ sudo docker run [...] --env-file ./my_env.list [...]`

Application에서 variable을 쓰려면
`console.log('Hello ' + process.env.NAME)`

#### 방법2. `Dockerfile`에 정의하기
```
> cat Dockerfile
FROM ubuntu
ENV API_KEY \
    ANOTHER_SECRET

> cat env.file
API_KEY=ab3da3bda4d4a4c4c4                                                                                                                   
ANOTHER_SECRET=shhh!
```
=> 근데 이방법은 결국 github에 .env파일을 올려야하는 것아닌가?

#### 방법3. configmap and secrets in kubernetes
-----------------------------
### Sensitive Environment Variables
#### Encrypt and decrypt your .env file  
* dotenv-vault  
* dotenvenc  

#### ExtractTextPlugin  
Since webpack v4 the extract-text-webpack-plugin should not be used for css. Use mini-css-extract-plugin instead.  
`npm install --save-dev mini-css-extract-plugin`

-----------------------------
### Code Spliting
* Code Spliting 왜 필요한가
webpack 을 이용하여 spa 애플리케이션을 만들면 결국 모든 소스파일이 하나의 파일로 (ex, index.bundle.js) 번들링되는데 개발이 진행되면서 여러가지 라이브러리들을 사용하게 되면 번들파일의 용량이 4~5MB 이상으로 금방 무거워질 수 있다.  
이렇게 될 경우, 네트워크 상황에 따라 해당 js를 처음 내려받는데 오랜 시간이 걸릴 수 있다.  
이럴 경우에는 index.bundle.js 파일을 splitting 하여 다운로드 받는데 걸리는 시간을 줄일 필요가 있다.  

* 정적 Spliting 
webpack 설정에서 진입점(entry) 를 2개 이상으로 설정. 즉, Multi Entry Point 설정을 한다.
```
entry: {
    index: ['@babel/polyfill','./src/index.js'],
    react : ["react", "react-dom", "react-router-dom", 'react-bootstrap'],
},
```
* 동적 Spliting (:eyes: TO-DO: 좀더 찾아볼 필요 있음)
  * `import()`을 이용해 사용이 필요한 시점에 모듈을 동적으로 로드하는 방법
  * optimization.splitChunks 옵션 
  
 
* 코드 스플리팅 시 file size
빌드를 하면 가끔 이런 메세지가 나온다.  
```
WARNING in asset size limit: The following asset(s) exceed the recommended size limit (244 KiB).
This can impact web performance.
Assets:
  index.5a6046f74d1e9a0d1614.bundle.js (254 KiB)
  vendors.9aac5f96d758fa17bc36.bundle.js (1.15 MiB)
```
-----------------------------
### Change relative paths to absolute paths
패키지 구조가 복잡해질 수록 `import`할때 상대경로를 사용하면 `import reducer from '../../../reducer';` 이렇게 쓸데없이 번거롭다.
* .babelrc에 module-resolver 설정
```js
{
    "presets": [
      "@babel/preset-env",
      "@babel/preset-react"
    ],
    "plugins": [
        ["@babel/plugin-proposal-class-properties"],
        [
          "module-resolver",{
            "root": ["./src"],
            "alias": {
              "public": "./public"
            }
          },
        ]
    ]
  }
```
