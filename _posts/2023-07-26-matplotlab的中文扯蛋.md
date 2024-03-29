> matplot遇到中文问题, 网上一通搜索, chatgpt也是傻逼了, 
> 最终遇到这篇高质量文档: https://zhuanlan.zhihu.com/p/104081310

### 歧途

- 各种删除缓存
- 各种安装字体

### 正解

```python
import matplotlib.pyplot as plt
# * 下面两行二选一
plt.rcParams["font.sans-serif"] = ["PingFang HK"]  # 设置字体, 系统默认: 苹方
plt.rc('font', family='Lantinghei SC', size=13)  # 设置字体, 民族之光: 方正兰亭
plt.rc('font', family='Kai', size=13)  # 设置字体, '楷体'

```

### 更详细的正解

```python
import matplotlib.font_manager as fm
import textwrap

# 转字典再转列表是为了去除重复项目
m_font= ', '.join(sorted([*{f.name:True for f in fm.fontManager.ttflist}]))

# 用这个看系统所有的字体, 挑自己喜欢的使用就好了
print(textwrap.fill(m_font, width=80))

# 正常显示负号的代码
plt.rcParams["axes.unicode_minus"] = False  # 正常显示负号

### 还有更多其他的看各种系统信息的函数
import matplotlib as plt
print(plt.get_configdir())
print(plt.get_cachedir())
```

### 气人的苹果

1. font book显示的字体是没用的, 一堆pingfang没有能用的, 纯纯油饼
2. font的文件名和字体名几乎无关. 这帮傻叉
3. font的文件名就没有i18n的概念, 傻出天际
4. matplot脑子抽了, 默认字体不是系统的默认字体, 傻叉本叉

### 借助chatgpt, 所有中文字体如下

> 免责声明: 这是chatgpt给的, 谨慎使用

```
.Aqua Kana
.Keyboard
.New York
.SF Arabic
.SF Compact
.SF Compact Rounded
.SF NS Mono
Academy Engraved LET
Al Bayan
Al Nile
Al Tarikh
American Typewriter
Andale Mono
Apple Braille
Apple Chancery
Apple LiGothic
Apple SD Gothic Neo
Apple Symbols
AppleGothic
AppleMyungjo
Arial
Arial Black
Arial Hebrew
Arial Narrow
Arial Rounded MT Bold
Arial Unicode MS
Athelas
Avenir
Avenir Next
Avenir Next Condensed
Ayuthaya
BM Dohyeon
BM Hanna 11yrs Old
BM Hanna Air
BM Hanna Pro
BM Jua
BM Kirang Haerang
BM Yeonsung
Baghdad
Bangla MN
Bangla Sangam MN
Baskerville
Beirut
Big Caslon
Bodoni 72
Bodoni 72 Oldstyle
Bodoni 72 Smallcaps
Bodoni Ornaments
Bradley Hand
Brush Script MT
Chalkboard
Chalkboard SE
Chalkduster
Charter
Cochin
Comic Sans MS
Copperplate
Corsiva Hebrew
Courier
Courier New
DIN Alternate
DIN Condensed
Damascus
DecoType Naskh
DejaVu Sans
DejaVu Sans Display
DejaVu Sans Mono
DejaVu Serif
DejaVu Serif Display
Devanagari MT
Devanagari Sangam MN
Didot
Diwan Kufi
Diwan Thuluth
Euphemia UCAS
Farah
Farisi
Futura
Galvji
Geeza Pro
Geneva
Georgia
Gill Sans
Gujarati MT
Gujarati Sangam MN
Gurmukhi MN
Gurmukhi MT
Gurmukhi Sangam MN
HanziPen SC
Heiti TC
Helvetica
Helvetica Neue
Herculanum
Hiragino Maru Gothic Pro
Hiragino Mincho ProN
Hiragino Sans
Hoefler Text
ITF Devanagari
Impact
InaiMathi
Iowan Old Style
Kai
Kailasa
Kaiti SC
Kannada MN
Kannada Sangam MN
Kefa
Khmer MN
Khmer Sangam MN
Kokonor
Krungthep
KufiStandardGK
Lantinghei SC
Lao MN
Lao Sangam MN
LiSong Pro
Lucida Grande
Luminari
Malayalam MN
Malayalam Sangam MN
Marion
Marker Felt
Menlo
Microsoft Sans Serif
Mishafi
Mishafi Gold
Monaco
Mshtakan
Mukta Mahee
Muna
Myanmar MN
Myanmar Sangam MN
Nadeem
Nanum Brush Script
New Peninim MT
Noteworthy
Noto Nastaliq Urdu
Optima
Oriya MN
Oriya Sangam MN
Osaka
PT Mono
PT Sans
PT Serif
PT Serif Caption
Palatino
Papyrus
Party LET
Phosphate
PingFang HK
Plantagenet Cherokee
Raanana
Rockwell
STIXGeneral
STIXIntegralsD
STIXIntegralsSm
STIXIntegralsUp
STIXIntegralsUpD
STIXIntegralsUpSm
STIXNonUnicode
STIXSizeFiveSym
STIXSizeFourSym
STIXSizeOneSym
STIXSizeThreeSym
STIXSizeTwoSym
STIXVariants
Sana
Sathu
Savoye LET
Seravek
Shree Devanagari 714
SignPainter
Silom
Sinhala MN
Sinhala Sangam MN
Skia
Snell Roundhand
Songti SC
Sukhumvit Set
Superclarendon
Symbol
System Font
Tahoma
Tamil MN
Tamil Sangam MN
Telugu MN
Telugu Sangam MN
Thonburi
Times
Times New Roman
Toppan Bunkyu Midashi Mincho
Trattatello
Trebuchet MS
Tsukushi A Round Gothic
Tsukushi B Round Gothic
Verdana
Waseem
Webdings
Weibei SC
Wingdings
Wingdings 2
Wingdings 3
YuMincho
Zapf Dingbats
Zapfino
cmb10
cmex10
cmmi10
cmr10
cmss10
cmsy10
cmtt10
```

