前提：
  我们要知道一点：发布到npm上的组件是经过编译的代码。es6不行,jsx不行，stylus/sass/less都不行，你可以用gulp/webpack配合各种插件loader去编译你的源码，然后发布即可。

页面中使用：
import Hello from 'react-cat';
<Hello />

1.创建React组件

  创建react-cat目录：
    mkdir z-test-react
    cd z-test-react
    yarn init //生成package.json
  安装依赖包:
    yarn add babel-core babel-loader style-loader css-loader -D
    yarn add react babel-preset-env babel-preset-react clean-webpack-plugin -D
    yarn add webpack webpack-cli -D
  项目目录
  ├── node_modules
  
  ├── .gitignore //手动添加
  ├── src
  │ ├── commpents
  │ │ ├── hello.js
  │ │ └──hello.css
  │ └── index.js
  ├──  webpack.config.js
  ├── .babelrc//手动添加
  ├── README.md//手动添加，建议英文书写
  └── webpack.config.js//手动添加
  package.json
  {
  "name": "react-cat",
  "version": "1.0.0",
  "main": "lib/index.js",//这里是我们组件的入口文件。开发者在 import 我们的组件的时候会引入这里 export 的内容
  "files": ["lib"],//files: 申明将要发布到 npm 的文件。如果省略掉这一项，所有文件包括源代码会被一起上传到 npm
  "author": "pengjielee",
  "license": "MIT",
  "scripts": {
    "build": "webpack --progress",//加入一个 build 指令来运行 webpack，此时运行的 webpack 是这个当前文件夹内安装的 webpack 而不是 global 的 webpack
    "watch": "webpack --watch --progress"
  },
  "devDependencies": {
    "babel-core": "^6.26.0",
    "babel-loader": "^7.1.4",
    "babel-preset-react": "^6.24.1",
    "clean-webpack-plugin": "^0.1.19",
    "css-loader": "^0.28.11",
    "react": "^16.2.0",
    "style-loader": "^0.20.3",
    "webpack": "^4.2.0",
    "webpack-cli": "^2.0.12"
  }
}

.gitignore
/node_modules
webpack.config.js
const path = require('path');
const CleanWebpackPlugin = require('clean-webpack-plugin');

module.exports = {
  mode: 'production', 
  
  entry: {
    index: './src/index.js'
  },

  output: {
    filename: '[name].js',
    path: path.resolve(__dirname, "lib"),
    libraryTarget: 'commonjs2'//使 test project 可以找到我们打包后的组件
  },

  module: {
    rules: [
      { 
        test: /\.js$/, 
        exclude: /node_modules/, 
        loader: 'babel-loader?presets[]=env&presets[]=react' 
      },
      { 
        test: /\.css$/, 
        use: ['style-loader','css-loader']
      }
    ]
  },

  plugins: [
    new CleanWebpackPlugin(['lib'])
  ]
}

构建项目：
yarn run build
创建项目链接 
yarn link
//测试
在本地新建测试项目，链接react-cat项目  
yarn link ‘react-cat’
打开pblog/src/App.js，引入react-cat组件
运行项目
yarn run start

2.发布到npm

  1.首先，我们需要一个npm的账号。这个可以直接登录npm官方网站去注册，免费的哦。https://www.npmjs.com/signup
  进入项目：
  npm login 
    //如果你以前使用过npm淘宝镜像的下载链接，这个时候要将下载链接设置回来 npm config set registry http://registry.npmjs.org 
    //npm config set registry https://registry.npm.taobao.org //因为这个官方npm加载速度慢，之后再切换回来
  npm publish //发布、更新,但要注意的是，每次更新时，必须修改版本号后才能更新，比如将1.0.0修改为1.0.1后就能进行更新发布了。

  包目录
  ├── package.json
  ├── README.md//建议英文书写
  └── index.js//主线代码

6.管理员权限
（1）查看管理包的所有者
npm owner ls <package name>
(2)添加/删除包的拥有者
npm owner add <user> <package name>
npm owner rm <user> <package name>
7.分析包
使用<npm ls>，可分析出当前路径下能够通过模块路径找到的所有包，并生成依赖树。
在使用NPM过程中，如果不能确认当前目录下能否通过require()顺利引入想要的包，这时可以执行<npm ls>分析包。

推荐阅读：
如何创建React组件并发布到npm？https://www.jianshu.com/p/23b7ae876d03



