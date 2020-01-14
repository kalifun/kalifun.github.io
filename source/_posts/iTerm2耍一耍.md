---
title : iTerm2耍一耍
date: 2018-10-24 17:28:12
categories: 工具
tags :
	- iTerm2
	- mac
---
# iTerm2耍一耍
## 安装[iTerm2](https://www.iterm2.com/)
```
brew cask install iterm2
```
## 配置iTerm2主题
### 最常用的主题就属于它了[Solarized Dark theme](https://ethanschoonover.com/solarized/)果断下载。
#### 打开iTerm2,到达preferences  
![color](iTerm2耍一耍/color.png)  
## 配置[Oh My ZSH](https://ohmyz.sh/)
```bash
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
![zsh](iTerm2耍一耍/zsh.png))  
#### 出现这个说明安装成功。然后修改.zshrc，修改主题为ys
```
ZSH_THEME="ys"
```
#### 重新打开窗口。  
![ys](iTerm2耍一耍/ys.png)
#### 或者你可以修改为agnoster。
```
ZSH_THEME="agnoster"
```
- #### 下载字体[Meslo](https://github.com/powerline/fonts/blob/master/Meslo%20Slashed/Meslo%20LG%20M%20Regular%20for%20Powerline.ttf)
- #### 安装上字体。
![font](iTerm2耍一耍/font.png)
![agnoster](iTerm2耍一耍/agnoster.png)

### 有人说需要自己安装才能自动提示，我安装完tab自动补全，自动提示都有了。没有的自行百度。风格就看自己喜欢什么的的来选吧。
