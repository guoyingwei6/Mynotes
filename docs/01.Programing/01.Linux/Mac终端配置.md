# Mac终端配置

```zsh linenums="1"

# 安装插件 
git clone https://github.com/robbyrussell/oh-my-zsh.git ~/.zsh/plugins
git clone https://github.com/zsh-users/zsh-autosuggestions ~/.zsh/plugins
# 启用插件
source ~/.zsh/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
source ~/.zsh/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh

# 启用彩色提示符
autoload -U colors && colors

# 每次刷新提示符
setopt prompt_subst

# 彩色
export CLICOLOR=1

# 设置提示符
PROMPT=$'%{$fg_bold[red]%}%{$reset_color%} %{$fg_bold[magenta]%}%M%{$reset_color%} %{$fg_bold[blue]%}%n%{$reset_color%} %{$fg_bold[cyan]%}%D %*%{$reset_color%} %{$fg_bold[green]%}%d%{$reset_color%} %{$fg_bold[blue]%}$(git branch --show-current 2&> /dev/null | xargs -I branch echo "(branch)")%{$reset_color%}\n%{$fg_bold[yellow]%}%#%{$reset_color%} '

# proxy
alias proxy='export all_proxy=socks5://127.0.0.1:7890'
alias unproxy='unset all_proxy'
```