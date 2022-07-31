## Iterm2
## zsh
## oh-my-zsh
### 安装
### 主题
powerlevel10k
#### 字体
### 插件
都需要在 ~/.zshrc 中配置 plugins=(插件名称1 插件名称2)
#### autojump
brew install autojump
#### zsh-syntax-highlighting
git clone [https://github.com/zsh-users/zsh-syntax-highlighting.git](https://github.com/zsh-users/zsh-syntax-highlighting.git) ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
#### zsh-autosuggestions
git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
更改补全的快捷键
在 ~/.zshrc 加 bindkey ',' autosuggest-accept 将快捷键改为逗号捡
#### vscode
git clone [https://github.com/valentinocossar/vscode.git](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fvalentinocossar%2Fvscode.git) ${ZSH_[CUSTOM:-~/.oh-my-zsh/custom](https://links.jianshu.com/go?to=CUSTOM%3A-%7E%2F.oh-my-zsh%2Fcustom)}/plugins/vscode

命令别名
cd ~
vim .zshrc
alias ll='ls -lF'
alias lla='ls -AlF'
source ~/.zshrc

