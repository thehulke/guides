# P.*.M.P your terminal - Use ZSH instead of Bash

First [Read me - what is zsh](https://www.howtogeek.com/362409/what-is-zsh-and-why-should-you-use-it-instead-of-bash/)
- Ignore the tutorial inside the link

Summary:
1. install zsh
2. install oh-my-zsh
3. install preferd fonts
4. install plugins and add ons to oh-my-zsh
5. configure ZSH
6. set favorite theme and color presets
7. restart your terminal

## install `zsh`
<details> <summary>Follow the steps here:</summary>

### Mac

- `brew update && brew upgrade`
- `brew install zsh`
- `chsh -s /bin/zsh`

### ubuntu

- `sudo apt-get install zsh`

### Other OS

`https://github.com/robbyrussell/oh-my-zsh/wiki/Installing-ZSH`
</details>

## Install oh-my-zsh

`sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`

## Install Preferd Fonts - `nerd-fonts` - fix it
<details> <summary>Follow the steps here:</summary>

- `git clone https://github.com/ryanoasis/nerd-fonts.git --depth 1`
- Go to repo. folder
- run `./install.sh`

**OR**

- `cd ~/Library/Fonts && curl -fLo "Meslo LG L Regular Nerd Font Complete.ttf" https://github.com/ryanoasis/nerd-fonts/blob/master/patched-fonts/Meslo/L/Regular/complete/Meslo%20LG%20L%20Regular%20Nerd%20Font%20Complete.ttf`


- Set 1 of the above `nerd-fonts` in the terminal settings:
- in Iterm go to preferences -> prfofiles tab-> text tab -> font
</details>

## Install Addons & plugins flow
<details> <summary>Follow the steps here:</summary>

- `git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions`
- `git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting`
- `source ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting`
- `git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k`

</details>

## Favorite terminal theme (fix it to be more clear)
<details> <summary>Follow the steps here:</summary>

- `git clone https://github.com/carloscuesta/materialshell.git --depth 1`
- Go to `shell-color-themes/macOS`
- paste the shell scheme to a premade shells themes folder
- apply to your terminal

**OR**

[Go to materialShell page and follow instructions](https://github.com/carloscuesta/materialshell)

</details>

## Configure ZSH - `.zshrc`
<details>
	<summary>Copy paste the following into your `~/.zshrc` file</summary>

```bash
 If you come from bash you might have to change your $PATH.
# export PATH=$HOME/bin:/usr/local/bin:$PATH
#### Only fot laptop config!
prompt_zsh_battery_level() {
  local percentage1=`pmset -g ps  |  sed -n 's/.*[[:blank:]]+*\(.*%\).*/\1/p'`
  local percentage=`echo "${percentage1//\%}"`
  local color='%F{red}'
  local symbol="\uf00d"
  pmset -g ps | grep "discharging" > /dev/null
  if [ $? -eq 0 ]; then
    local charging="false";
  else
    local charging="true";
  fi
  if [ $percentage -le 20 ]
  then symbol='\uf579' ; color='%F{red}' ;
    #10%
  elif [ $percentage -gt 19 ] && [ $percentage -le 30 ]
  then symbol="\uf57a" ; color='%F{red}' ;
    #20%
  elif [ $percentage -gt 29 ] && [ $percentage -le 40 ]
  then symbol="\uf57b" ; color='%F{yellow}' ;
    #35%
  elif [ $percentage -gt 39 ] && [ $percentage -le 50 ]
  then symbol="\uf57c" ; color='%F{yellow}' ;
    #45%
  elif [ $percentage -gt 49 ] && [ $percentage -le 60 ]
  then symbol="\uf57d" ; color='%F{blue}' ;
    #55%
  elif [ $percentage -gt 59 ] && [ $percentage -le 70 ]
  then symbol="\uf57e" ; color='%F{blue}' ;
    #65%
  elif [ $percentage -gt 69 ] && [ $percentage -le 80 ]
  then symbol="\uf57f" ; color='%F{blue}' ;
    #75%
  elif [ $percentage -gt 79 ] && [ $percentage -le 90 ]
  then symbol="\uf580" ; color='%F{blue}' ;
    #85%
  elif [ $percentage -gt 89 ] && [ $percentage -le 99 ]
  then symbol="\uf581" ; color='%F{blue}' ;
    #85%
  elif [ $percentage -gt 98 ]
  then symbol="\uf578" ; color='%F{green}' ;
    #100%
  fi
  if [ $charging = "true" ];
  then color='%F{green}'; if [ $percentage -gt 98 ]; then symbol='\uf584'; fi
  fi
  echo -n "%{$color%}$symbol" ;
}
lfcd () {
    tmp="$(mktemp)"
    lf -last-dir-path="$tmp" "$@"
    if [ -f "$tmp" ]; then
        dir="$(cat "$tmp")"
        rm -f "$tmp"
        [ -d "$dir" ] && [ "$dir" != "$(pwd)" ] && cd "$dir"
    fi
}
bindkey -s '^o' 'lfcd\n'
# zsh_internet_signal(){
#   #source on quality levels - http://www.wireless-nets.com/resources/tutorials/define_SNR_values.html
#   #source on signal levels  - http://www.speedguide.net/faq/how-to-read-rssisignal-and-snrnoise-ratings-440
#   local signal=$(airport -I | grep agrCtlRSSI | awk '{print $2}' | sed 's/-//g')
#   local noise=$(airport -I | grep agrCtlNoise | awk '{print $2}' | sed 's/-//g')
#   local SNR=$(bc <<<"scale=2; $signal / $noise")
#   local net=$(curl -D- -o /dev/null -s http://www.google.com | grep HTTP/1.1 | awk '{print $2}')
#   local color='%F{yellow}'
#   local symbol="\uf197"
#   # Excellent Signal (5 bars)
#   if [[ ! -z "${signal// }" ]] && [[ $SNR -gt .40 ]] ;
#     then color='%F{black}' ; symbol="\uf1eb" ;
#   fi
#   # Good Signal (3-4 bars)
#   if [[ ! -z "${signal// }" ]] && [[ ! $SNR -gt .40 ]] && [[ $SNR -gt .25 ]] ;
#     then color='%F{green}' ; symbol="\uf1eb" ;
#   fi
#   # Low Signal (2 bars)
#   if [[ ! -z "${signal// }" ]] && [[ ! $SNR -gt .25 ]] && [[ $SNR -gt .15 ]] ;
#     then color='%F{yellow}' ; symbol="\uf1eb" ;
#   fi
#   # Very Low Signal (1 bar)
#   if [[ ! -z "${signal// }" ]] && [[ ! $SNR -gt .15 ]] && [[ $SNR -gt .10 ]] ;
#     then color='%F{red}' ; symbol="\uf1eb" ;
#   fi
#   # No Signal - No Internet
#   if [[ ! -z "${signal// }" ]] && [[ ! $SNR -gt .10 ]] ;
#     # then color='%F{red}' ; symbol="\uf011";
#     then color='%F{red}' ; symbol="\uf204";
#   fi
#   if [[ -z "${signal// }" ]] && [[ "$net" -ne 200 ]] ;
#     # then color='%F{red}' ; symbol="\uf011";
#     then color='%F{red}' ; symbol="\uf204" ;
#   fi
#   # Ethernet Connection (no wifi, hardline)
#   if [[ -z "${signal// }" ]] && [[ "$net" -eq 200 ]] ;
#     then color='%F{blue}' ; symbol="\uf197" ;
#   fi
#   echo -n "%{$color%}$symbol " # \f1eb is wifi bars
# }
#### End of laptop config
# Path to your oh-my-zsh installation.
export ZSH="/Users/${whoami}/.oh-my-zsh"
ZSH_THEME="powerlevel9k/powerlevel9k"
################ start custom oh my zsh configs
POWERLEVEL9K_MODE='nerdfont-complete'
# Please only use this battery segment if you have material icons in your nerd font (or font)
# Otherwise, use the font awesome one in "User Segments"
# POWERLEVEL9K_CUSTOM_INTERNET_SIGNAL="zsh_internet_signal"
POWERLEVEL9K_BATTERY_ICON=`prompt_zsh_battery_level`
POWERLEVEL9K_PROMPT_ON_NEWLINE=true
POWERLEVEL9K_PROMPT_ADD_NEWLINE=true
POWERLEVEL9K_RPROMPT_ON_NEWLINE=true
POWERLEVEL9K_SHORTEN_DIR_LENGTH=2
POWERLEVEL9K_SHORTEN_STRATEGY="truncate_beginning"
POWERLEVEL9K_RVM_FOREGROUND="249"
POWERLEVEL9K_RVM_VISUAL_IDENTIFIER_COLOR="red"
POWERLEVEL9K_TIME_BACKGROUND="black"
POWERLEVEL9K_TIME_FOREGROUND="249"
POWERLEVEL9K_TIME_FORMAT="\UF43A %D{%I:%M  \UF133  %m.%d.%y}"
POWERLEVEL9K_RVM_BACKGROUND="black"
POWERLEVEL9K_RVM_FOREGROUND="249"
POWERLEVEL9K_RVM_VISUAL_IDENTIFIER_COLOR="red"
POWERLEVEL9K_STATUS_VERBOSE=false
POWERLEVEL9K_VCS_CLEAN_FOREGROUND='black'
POWERLEVEL9K_VCS_CLEAN_BACKGROUND='green'
POWERLEVEL9K_VCS_UNTRACKED_FOREGROUND='black'
POWERLEVEL9K_VCS_UNTRACKED_BACKGROUND='yellow'
POWERLEVEL9K_VCS_MODIFIED_FOREGROUND='white'
POWERLEVEL9K_VCS_MODIFIED_BACKGROUND='black'
POWERLEVEL9K_COMMAND_EXECUTION_TIME_BACKGROUND='black'
POWERLEVEL9K_COMMAND_EXECUTION_TIME_FOREGROUND='blue'
POWERLEVEL9K_FOLDER_ICON=''
POWERLEVEL9K_STATUS_OK_IN_NON_VERBOSE=true
POWERLEVEL9K_STATUS_VERBOSE=false
POWERLEVEL9K_COMMAND_EXECUTION_TIME_THRESHOLD=0
POWERLEVEL9K_VCS_UNTRACKED_ICON='\u25CF'
POWERLEVEL9K_VCS_UNSTAGED_ICON='\u00b1'
POWERLEVEL9K_VCS_INCOMING_CHANGES_ICON='\u2193'
POWERLEVEL9K_VCS_OUTGOING_CHANGES_ICON='\u2191'
POWERLEVEL9K_VCS_COMMIT_ICON="\uf417"
POWERLEVEL9K_MULTILINE_FIRST_PROMPT_PREFIX="%F{blue}\u256D\u2500%f"
POWERLEVEL9K_MULTILINE_LAST_PROMPT_PREFIX="%F{blue}\u2570\uf460%f "
POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=(context os_icon custom_internet_signal custom_battery_status_joined ssh root_indicator dir dir_writable vcs)
POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=(command_execution_time  status  time)
HIST_STAMPS="mm/dd/yyyy"
DISABLE_UPDATE_PROMPT=true
# battery
POWERLEVEL9K_BATTERY_CHARGING='yellow'
POWERLEVEL9K_BATTERY_CHARGED='green'
POWERLEVEL9K_BATTERY_DISCONNECTED='$DEFAULT_COLOR'
POWERLEVEL9K_BATTERY_LOW_THRESHOLD='10'
POWERLEVEL9K_BATTERY_LOW_COLOR='red'
POWERLEVEL9K_BATTERY_ICON=`prompt_zsh_battery_level `
source ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
plugins=(
  git
  iterm2
  macports
  man
  osx
  python
  composer
  zsh-autosuggestions
  zsh-syntax-highlighting
)
ZSH_AUTOSUGGEST_HIGHLIGHT_STYLE='fg=5'
alias suroot='sudo -E -s'
# source ~/.bash_profile
if [ -f ~/.bash_profile ]; then
    . ~/.bash_profile;
fi
if [ -f ~/.aliases ]; then
    . ~/.aliases;
fi
########################## end of custom config
source $ZSH/oh-my-zsh.sh
source ~/.bash_profile
# nvm setup
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```
</details>

