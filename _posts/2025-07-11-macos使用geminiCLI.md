>   使用这玩意竟然还要技巧

###### 安装啥的都看官方

* https://github.com/google-gemini/gemini-cli?tab=readme-ov-file



###### 第一个问题: 登录不进去

* 即便全局代理, 都不一定解决问题
* 申请个apikey就OK了: https://aistudio.google.com/apikey?pli=1
* 然后把key配置到:  export GEMINI_API_KEY="你的key"



###### 第二个问题: 报错: ✕ [API Error: User location is not supported for the API use. (Status:   FAILED_PRECONDITION)]

* 全局代理可以解决问题.

* 更灵活的方案: 

```sh
  brew install proxychains-ng
  # 编辑 
  code /opt/homebrew/etc/proxychains.conf
  # 配置: 
  http 127.0.0.1 7890
  # 使用
  proxychains4 gemini
```

```json
// 详细报错内容 
Debug Console (ctrl+o to close)
 ... first 42 lines hidden ...
      errno: -60,
      code: 'ETIMEDOUT',
      syscall: 'connect',
      address: '216.239.32.223',
      port: 443
    }
  ]  
} (x7)
```



### 无效的方法

* zshrc直接设置http代理
