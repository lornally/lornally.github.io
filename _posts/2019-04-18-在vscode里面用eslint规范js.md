https://blog.echobind.com/integrating-prettier-eslint-airbnb-style-guide-in-vscode-47f07b5d7d6a

```sh
cnpm install eslint -g  
eslint index.js #这时报错, 需要config
eslint --init # 这里有一系列配置, 如下: 


How would you like to use ESLint? To check syntax, find problems, and enforce 
code style
? What type of modules does your project use? CommonJS (require/exports)
? Which framework does your project use? React
? Where does your code run? (Press <space> to select, <a> to toggle all, <i> to 
invert selection)Browser
? How would you like to define a style for your project? Use a popular style gui
de
? Which style guide do you want to follow? Airbnb (https://github.com/airbnb/jav
ascript)
? What format do you want your config file to be in? YAML




```

然后有个npm的install问题, 隔壁写了解决方案.

###### 最终的做法

使用了同事提供的配置文件和依赖包.

```sh
npm run lint    # 在package.json里面配置了lint脚本: 
"lint": "eslint --ext .js js index.js", # 这个就是pacakage.json里面的对应脚本.
# 然后会报高危漏洞, 需要 npm audit fix, 但是运行这个会报错, 需要: 
cnpm audit fix
```

.eslintrc.js示例

```js
module.exports = {
	extends: ['wind/node'],
	globals: {
		FileReader: false,
		DOMException: false,
		Headers: false,
		fetch: false,
	},

	rules: {
		'strict': [0],
		'vars-on-top': 0,
		'global-require': 0,
		"indent": 0,
		"no-tabs":0,
		"prettier/prettier": 0,
		"spaced-comment":0,
		'new-cap':0,
	},
};

```



package.json示例

```json
{
  "name": "uds",
  "version": "0.0.1",
  "description": "调度算法的node版本",
	"main": "index.js",
	"type": "module",
  "dependencies": {
    "json5": "latest",
    "pm2": "latest",
    "debug": "latest",
    "request-promise": "latest",
		"request": "latest",
		"stampit": "latest",
		
  },
  "devDependencies": {
    "eslint-config-wind": "git+ssh://git@code.timehouse.build:ext-lib/eslint-config-wind.git#master",
    "husky": "^1.3.1",
    "lint-staged": "^8.1.5",
    "mocha": "^6.1.4",
    "nyc": "latest",
    "riteway": "^6.0.3",
    "tap-nirvana": "latest",
    "tape": "latest"
  },
  "scripts": {
    "testinterface": "node ./testcase/interfacetest.js",
    "testmocha": "mocha",
    "test": "nyc riteway test/riteway/*-rt.js | tap-nirvana",
    "pm2start": "pm2 start ./index.js --name uds",
    "start": "node ./index.js",
    "stop": "pm2 stop  uds",
    "lint": "eslint --ext .js js index.js",
    "eslint": "eslint --fix --ext .js js index.js",
    "lint-staged": "lint-staged"
  },
  "repository": {
    "type": "git",
    "url": ""
  },
  "author": "",
  "license": "",
  "lint-staged": {
    "*.js": [
      "eslint --fix",
      "git add"
    ]
  },
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  }
}

```

