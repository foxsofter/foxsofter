---
layout: post
title: 在xcode中优雅的TODO和FIXME
excerpt: "这些xcode小技巧可以提高效率"
modified: 2020-03-24
categories: articles
tags: [objective c][xcode]
comments: true
share: true
author: foxsofter
---

TODO 和 FIXME 是编码中经常用到的一个小技巧。

## Xcode 的支持

Xcode 已经对 TODO、FIXME、MARK 做了内置的支持，在代码中添加类似如下的注释：

```
// TODO: Need to be done in the feture.
```

![df9e79b5f334f7e62d0cdcf46d876a24.png](evernotecid://CF249C4B-64BA-4C7E-96BA-FC73E5C8447B/appyinxiangcom/4761577/ENResource/p512)

```
// FIXME: Need to be fixed in the feture.
```

![8c4d8c117c40cf9c6a65b1d9b4691a0e.png](evernotecid://CF249C4B-64BA-4C7E-96BA-FC73E5C8447B/appyinxiangcom/4761577/ENResource/p514)

```
// MARK: Just for mark.
```

![8b6b627c09f41fa17918873bd81ffe1c.png](evernotecid://CF249C4B-64BA-4C7E-96BA-FC73E5C8447B/appyinxiangcom/4761577/ENResource/p515)

```
// ???: Have questions here.
```

![8d9ebac5946e558f3b0ea39c967579ea.png](evernotecid://CF249C4B-64BA-4C7E-96BA-FC73E5C8447B/appyinxiangcom/4761577/ENResource/p516)

```
// !!!: Oh my god.
```

![a92b1667c687ecfe5437041b3b6bfaeb.png](evernotecid://CF249C4B-64BA-4C7E-96BA-FC73E5C8447B/appyinxiangcom/4761577/ENResource/p517)

通过这种方式，在当前选中的文件是可以很快定位到相关的标记项，全局可以通过查找的方式，大部分情况下也能满足了。

## 更优雅的提醒

我们其实可以通过脚本，更优雅的给自己提个醒，比如 TODO：
![f31e6ac086c2926ca4fec57cad8bae71.png](evernotecid://CF249C4B-64BA-4C7E-96BA-FC73E5C8447B/appyinxiangcom/4761577/ENResource/p518)

```Shell
TODOMARK=" TODO:"
find "${SRCROOT}" \( -name "*.h" -or -name "*.m" -or -name "*.swift" \) -print0 | xargs -0 egrep --with-filename --line-number --only-matching "($TODOMARK).*\$" | perl -p -e "s/($TODOMARK)/ warning: \$1/"
```

TODO 在编译后会被当成 warning
![52e1e762d3369af4c90e8c1ff118063a.png](evernotecid://CF249C4B-64BA-4C7E-96BA-FC73E5C8447B/appyinxiangcom/4761577/ENResource/p519)

类似的 FIXME 也可以这样操作

```Shell
FIXMEMARK=" FIXME:"
find "${SRCROOT}" \( -name "*.h" -or -name "*.m" -or -name "*.swift" \) -print0 | xargs -0 egrep --with-filename --line-number --only-matching "($FIXMEMARK).*\$" | perl -p -e "s/($FIXMEMARK)/ warning: \$1/"
```

也可以设置成编译后当成 error

```Shell
ERRORMARK=" !!!:"
find "${SRCROOT}" \( -name "*.h" -or -name "*.m" -or -name "*.swift" \) -print0 | xargs -0 egrep --with-filename --line-number --only-matching "($ERRORMARK).*\$" | perl -p -e "s/($ERRORMARK)/ error: \$1/"
```

![cd8d625882fa53f58b19f792d0eac234.png](evernotecid://CF249C4B-64BA-4C7E-96BA-FC73E5C8447B/appyinxiangcom/4761577/ENResource/p520)

整合成一个脚本

```Shell
WARNINGS=" TODO:| FIXME:"
ERRORS=" !!!:"
find "${SRCROOT}" \( -name "*.h" -or -name "*.m" -or -name "*.swift" \) -print0 | xargs -0 egrep --with-filename --line-number --only-matching "($WARNINGS).*\$|($ERRORS).*\$" | perl -p -e "s/($WARNINGS)/ warning: \$1/" | perl -p -e "s/($ERRORS)/ error: \$1/"
```
