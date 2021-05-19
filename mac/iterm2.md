# iTerm2
iTerm2은 맥의 터미널 보조 프로그램

### zsh 설치
* zsh은 bash의 확장된 유닉스 셸((MacOS Catalina 버전부턴 default로 설치되어 있음)
* zsh를 설치하기 위해선 homebrew가 필요. 
    * homebrew란 MacOS에선 리눅스의 apt-get이나 yum와 같은 package manager 
`brew install zsh`

### Oh my ZSH 설치
ZSH를 더 쉽게 사용해주는 플러그인
`sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`

### Color theme 적용하기 
* 원하는 color theme 선택
    * [!iterm2colorschemes](https://iterm2colorschemes.com/)
* 원하는 테마를 찾으면 아래와 같이 테마의 이름을 클릭

### Theme 변경하기
* 사용할 테마는 agonster라는 테마로 현재 checkout중인 branch를 쉽게 알 수 있는 테마임
* `vi ~/.zshrc` 입력
* `ZSH_THEME`을 찾아 `ZSH_THEME="agnoster"`로 변경
    * 수정 후에 테마가 적용이 안됐다.
    * `source ~/.zshrc`로 zshrc을 reload하니 쌍따옴표가 하나 더 들어가 있었다;

### 터미널에 사용자 이름 삭제하기
* `heykim@hihi-MacBook-Pro  ls` 이런식으로 사용자 이름이 길게 나오니 지워보자
* `vi ~/.zshrc`열고 아래 코드 추가
    * ```
    prompt_context() {
        if [[ "$USER" != "$DEFAULT_USER" || -n "$SSH_CLIENT" ]]; then
            prompt_segment black default "%(!.%{%F{yellow}%}.)$USER"
        fi
    }```
* 바로 적용 안되면 reload
    * `source ~/.zshrc`


### Syntax Highlight 적용
syntax highlight는 사용 가능한 명령어들에 highlight를 넣어주는 기능입니다.
```bash
# brew를 통해 설치해줍니다.
brew install zsh-syntax-highlighting

# ~/.zshrc에 들어가서 아래의 코드를 입력해줍니다.
vi ~/.zshrc

source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

### 쉘에 이모티콘 적용하기
```bash
# ~/.zshrc에 들어가서 아래의 코드를 입력해줍니다.
vi ~/.zshrc

prompt_context() { 
  # Custom (Random emoji) 
  emojis=("⚡️" "🔥" "🇰" "👑" "😎" "🐸" "🐵" "🦄" "🌈" "🍻" "🚀" "💡" "🎉" "🔑" "🚦" "🌙")
  RAND_EMOJI_N=$(( $RANDOM % ${#emojis[@]} + 1)) 
  prompt_segment black default "{하고싶은이름} ${emojis[$RAND_EMOJI_N]} " 
}
```


## 참고
* [!iTerm2로 터미널 커스텀하기](https://ooeunz.tistory.com/21)