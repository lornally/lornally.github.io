> 苹果开始不能quicklook了

### 报错

```sh
"xxx.qlgenerator" can’t be opened because Apple cannot check it for malicious software.
This software needs to be updated. Contact the developer for more information.

Homebrew Cask downloaded this file on November 16, 2022.
```

###  解决方案一

- system preferences
- security & privacy
- general
- 此时有allow anyway这个选项, 同意就好了

### 解决2

```sh
xattr -r -d com.apple.quarantine /Library/QuickLook/*.qlgenerator 
xattr -r -d com.apple.quarantine ~/Library/QuickLook/*.qlgenerator 
xattr -cr ~/Library/QuickLook/*.qlgenerator
qlmanage -r
qlmanage -r cache

```

