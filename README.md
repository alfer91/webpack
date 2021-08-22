# Webpack
1. [프로젝트 생성](#프로젝트-생성)
1. [entry, output](#entry-output)
1. [plugins](#plugins)
1. [정적 파일 연결](#정적-파일-연결)
1. [module](#module)
1. [SCSS](#scss)
1. [Autoprefixer/PostCSS](#autoprefixerpostcss)
1. [babel](#babel)
1. [Netlify 배포](#netlify-배포)
1. [NPX, Degit](#npx-degit)

# Webpack
## 프로젝트 생성
```bash
$ npm init -y
$ npm i -D webpack webpack-cli webpack-dev-server@next
```
`package.json`
```json
"scripts": {
  "dev": "webpack-dev-server --mode development",
  "build": "webpack --mode production"
},
```
`index.html`
```html
<head>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/reset-css@5.0.1/reset.min.css">
</head>
<body>
  <h1>Hello Webpack!!</h1>
</body>
```
`./js/main.js`
```js
console.log('Webpack!')
```
`webpack.config.js`

## entry, output
[webpack - DOCUMENTATION - Configuration - Entry and Context - entry](https://webpack.js.org/configuration/entry-context/#entry)  
[](https://webpack.js.org/configuration/output/#outputpath)  
[webpack - DOCUMENTATION - Configuration - Ouput - path](https://webpack.js.org/configuration/output/#outputpath)  
[webpack - DOCUMENTATION - Configuration - Ouput - clean](https://webpack.js.org/configuration/output/#outputclean)  
`webpack.config.js`
```js
// import
const path = require('path') // node.js 전역 module

// export
module.exports = {
  // parcel index.html
  // 파일을 읽어들이기 시작하는 진입점 설정
  entry: './js/main.js',

  // 결과물(번들)을 반환하는 설정
  output: {
    // path: path.resolve(__dirname, 'dist'), // 절대 경로
    // filename: 'main.js',
    clean: true
  }
}
```
```bash
$ npm run build
```

## plugins
```bash
$ npm i -D html-webpack-plugin
```
`webpack.config.js`
```js
// import
const path = require('path') // node.js 전역 module
const HtmlPlugin = require('html-webpack-plugin')

// export
module.exports = {
  // parcel index.html
  // 파일을 읽어들이기 시작하는 진입점 설정
  entry: './js/main.js',

  // 결과물(번들)을 반환하는 설정
  output: {
    // path: path.resolve(__dirname, 'dist'), // 절대 경로
    // filename: 'main.js',
    clean: true
  },

  // 번들링 후 결과물의 처리 방식 등 다양한 플러그인들을 설정
  plugins: [
    new HtmlPlugin({
      template: './index.html'
    })
  ],
  
  devServer: {
    host: 'localhost'
  }
}
```
```bash
$ npm run dev
```

## 정적 파일 연결
`./static/images/logo.png`
`./static/favicon.ico`  

`index.html`
```html
<body>
  <h1>Hello Webpack!!</h1>
  <img src="./images/log.png" alt="HEROPY"/>
</body>
```
```bash
$ npm i -D copy-webpack-plugin
```
`webpack.config.js`
```js
// import
const path = require('path') // node.js 전역 module
const HtmlPlugin = require('html-webpack-plugin')
const CopyPlugin = require('copy-webpack-plugin')
// export
module.exports = {
  // parcel index.html
  // 파일을 읽어들이기 시작하는 진입점 설정
  entry: './js/main.js',

  // 결과물(번들)을 반환하는 설정
  output: {
    // path: path.resolve(__dirname, 'dist'), // 절대 경로
    // filename: 'main.js',
    clean: true
  },

  // 번들링 후 결과물의 처리 방식 등 다양한 플러그인들을 설정
  plugins: [
    new HtmlPlugin({
      template: './index.html'
    }),
    new CopyPlugin({
      patterns: [
        { from: 'static' }
      ]
    })
  ],
  
  devServer: {
    host: 'localhost'
  }
}
```
```bash
$ npm run dev // 'Ctrl + F5' cahce 강제로 새로고침
$ npm run build
```

## module
`index.html`
```html
<head>
  <link rel="stylesheet" href="./css/main.css">
</head>
```
`./static/css/main.css`
```css
body {
  background-color: orange;
}
```
```bash
npm run dev
```
---
`./css/main.css`

`main.js`
```js
import '../css/main.css'

console.log('Webpack!')
```
```bash
$ npm i -D css-loader style-loader
```
`webpack.config.js`
```js
// import
const path = require('path') // node.js 전역 module
const HtmlPlugin = require('html-webpack-plugin')
const CopyPlugin = require('copy-webpack-plugin')
// export
module.exports = {
  // parcel index.html
  // 파일을 읽어들이기 시작하는 진입점 설정
  entry: './js/main.js',

  // 결과물(번들)을 반환하는 설정
  output: {
    // path: path.resolve(__dirname, 'dist'), // 절대 경로
    // filename: 'main.js',
    clean: true
  },

  module: {
    rules: [
      {
        test: /\.css$/,
        use: [ // 순서 주의
          'style-loader', // html style tag에 삽입
          'css-loader' // js에서 css 해석
        ]
      }
    ]
  },

  // 번들링 후 결과물의 처리 방식 등 다양한 플러그인들을 설정
  plugins: [
    new HtmlPlugin({
      template: './index.html'
    }),
    new CopyPlugin({
      patterns: [
        { from: 'static' }
      ]
    })
  ],
  
  devServer: {
    host: 'localhost'
  }
}
```
```bash
$ npm run dev
```

## SCSS
`./css/main.css` -> `./scss/main.scss`  
```bash
$ npm i -D sass-loader sass
```
`webpack.config.js`
```js
// import
const path = require('path') // node.js 전역 module
const HtmlPlugin = require('html-webpack-plugin')
const CopyPlugin = require('copy-webpack-plugin')
// export
module.exports = {
  // parcel index.html
  // 파일을 읽어들이기 시작하는 진입점 설정
  entry: './js/main.js',

  // 결과물(번들)을 반환하는 설정
  output: {
    // path: path.resolve(__dirname, 'dist'), // 절대 경로
    // filename: 'main.js',
    clean: true
  },

  module: {
    rules: [
      {
        test: /\.s?css$/,
        use: [ // 순서 주의
          'style-loader', // html style tag에 삽입
          'css-loader', // js에서 css 해석
          'sass-loader'
        ]
      }
    ]
  },

  // 번들링 후 결과물의 처리 방식 등 다양한 플러그인들을 설정
  plugins: [
    new HtmlPlugin({
      template: './index.html'
    }),
    new CopyPlugin({
      patterns: [
        { from: 'static' }
      ]
    })
  ],
  
  devServer: {
    host: 'localhost'
  }
}
```
`./js/main.js`
```js
import '../scss/main.scss'

console.log('Webpack!')
```
`./scss/main.scss`
```scss
$color--blaock: #000;
$color--white: #fff;

body {
  background-color: $color--blaock;
  h1 {
    color: $color--white;
    font-size: 40px;
  }
}
```

## Autoprefixer/PostCSS
`./scss/main.scss`
```scss
$color--blaock: #000;
$color--white: #fff;

body {
  background-color: $color--blaock;
  h1 {
    color: $color--white;
    font-size: 40px;
    display: flex;
  }
}
```
```bash
$ npm i -D postcss autoprefixer postcss-loader
```
`webpack.config.js`
```js
// import
const path = require('path') // node.js 전역 module
const HtmlPlugin = require('html-webpack-plugin')
const CopyPlugin = require('copy-webpack-plugin')
// export
module.exports = {
  // parcel index.html
  // 파일을 읽어들이기 시작하는 진입점 설정
  entry: './js/main.js',

  // 결과물(번들)을 반환하는 설정
  output: {
    // path: path.resolve(__dirname, 'dist'), // 절대 경로
    // filename: 'main.js',
    clean: true
  },

  module: {
    rules: [
      {
        test: /\.s?css$/,
        use: [ // 순서 주의
          'style-loader', // html style tag에 삽입
          'css-loader', // js에서 css 해석
          'postcss-loader',
          'sass-loader'
        ]
      }
    ]
  },

  // 번들링 후 결과물의 처리 방식 등 다양한 플러그인들을 설정
  plugins: [
    new HtmlPlugin({
      template: './index.html'
    }),
    new CopyPlugin({
      patterns: [
        { from: 'static' }
      ]
    })
  ],
  
  devServer: {
    host: 'localhost'
  }
}
```
`package.json`
```json
"browserslist": [
  "> 1%",
  "last 2 versions"
]
```
`.postcssrc.js`
```js
module.exports = {
  plugins: [
    require('autoprefixer')
  ]
}
```
```bash
$ npm run dev
```

## babel
```bash
$ npm i -D @babel/core @babel/preset-env @babel/plugin-transform-runtime
$ npm i -D babel-loader
```
`webpack.config.js`
```js
// import
const path = require('path') // node.js 전역 module
const HtmlPlugin = require('html-webpack-plugin')
const CopyPlugin = require('copy-webpack-plugin')
// export
module.exports = {
  // parcel index.html
  // 파일을 읽어들이기 시작하는 진입점 설정
  entry: './js/main.js',

  // 결과물(번들)을 반환하는 설정
  output: {
    // path: path.resolve(__dirname, 'dist'), // 절대 경로
    // filename: 'main.js',
    clean: true
  },

  module: {
    rules: [
      {
        test: /\.s?css$/,
        use: [ // 순서 주의
          'style-loader', // html style tag에 삽입
          'css-loader', // js에서 css 해석
          'postcss-loader',
          'sass-loader'
        ]
      },
      {
        test: /\.js$/,
        use: [
          'babel-loader'
        ]
      }
    ]
  },

  // 번들링 후 결과물의 처리 방식 등 다양한 플러그인들을 설정
  plugins: [
    new HtmlPlugin({
      template: './index.html'
    }),
    new CopyPlugin({
      patterns: [
        { from: 'static' }
      ]
    })
  ],
  
  devServer: {
    host: 'localhost'
  }
}
```
`package.json`
```json
"browserslist": [
  "> 1%",
  "last 2 versions"
]
```
`.babelrc.js`
```js
module.exports = {
  presets: ['@babel/preset-env'],
  plugins: [
    ['@babel/plugin-transform-runtime']  // 비동기 처리
  ]
}
```
`main.js`
```js
import '../scss/main.scss'

console.log('Webpack!')

async function test() { // 비동기 함수
  const promise = Promise.resolve(123)
  console.log(await promise)
}
test()
```
```bash
$ npm run dev
```

## Netlify 배포
`.gitignore`
```bash
$ git init
$ git add .
$ git commit "webpack"
$ git remote add origin https://github.com/alfer91/webpack.git
$ git push origin master
```

[netlify](https://app.netlify.com/) - New site from Git  
Basic build settings  
- Build command - npm run build  
- Publish directory - dist/

## NPX, Degit
버전관리가 되어있지 않은 채로 가져옴
```bash
// npx : digit 을 설치하지 않고 사용하게 해줌
$ cd .\Desktop\ 
$ npx degit alfer91/webpack webpack-test
$ cd webpack-test
$ code . -r
```