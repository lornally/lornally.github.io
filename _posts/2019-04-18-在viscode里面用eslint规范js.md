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

