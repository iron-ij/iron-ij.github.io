---
title: Mac 초기환경 세팅하기
date: 2021-12-05 21:15:00 +0900
categories: [etc]
tags: [etc]
use_math: true
---
- Mac 구매 또는 초기화 후에 개발환경을 세팅할 일이 간간히 있다. 2~3달에 한번정도는 하는 것 같은데, 그동안 매번 찾아서 하다가 나에게 맞는 세팅을 조금 맞춰놓고 싶어서 정리하게 되었다

# 1. Home brew 설치

```bash
$ /usr/bin/ruby -e "$(curl -fsSL [https://raw.githubusercontent.com/Homebrew/install/master/install](https://raw.githubusercontent.com/Homebrew/install/master/install))"
$ echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.bash_profile
```

# 2. 기본 프로그램 설치
- 주된 작업환경이 vsc이다보니 우선적으로 vsc와 그에 맞는 크롬 등을 설치한다.

1. brew cask install iterm2 google-chrome visual-studio-code
2. brew cask install homebrew/cask-fonts/font-cascadia
3. brew cask install homebrew/cask-fonts/font-d2coding

# 3. Iterm2 기본 설정

Iterm2 의 컬러 테마는 [이 곳](http://iterm2colorschemes.com/) 에서 다운로드해서 설치

- `Preferences` > `Profiles` > `Colors` > `Color Presets` > `Import...` 를 선택해서 다운로드 받은 컬러 테마의 압축 폴더 중에서 `schemes`에 있는 설정 파일을 선택
- dracula 테마설치

preferances

- `Appearance` > `Theme` : `Dark` 혹은 원하시는 Theme를 선택
- `Appearance` > `Windows` > `Hide scrollbars` : 사용함으로 선택
- `Appearance` > `Windows` > `Show line under title bar when the tab bar is not visible` : 사용하지 않음

- Iterm 추가설정(iterm에는 기본적으로 줄삭제, 분단 삭제가 없음)
- command + , 로 설정 들어간 후 profile → keys
- command ← / commad → 지워줌
    
    ### 왼쪽으로 단어 이동
    
    - keyboard short cut : command ←
    - action : Send Escape Sequence
    - esc+: b
    
    ### 오른쪽으로 단어 이동
    
    - keyboard short cut : command →
    - action : Send Escape Sequence
    - esc+: f
    
    ### 줄 **삭제**
    
    - Keyboard Shorcut: ⌘ + delete
    - Action: Send Hex Code
    - 0x15
    
    ### 단어 **삭제**
    
    - Keyboard Shorcut: ⌥ + delete
    - Action: Send Hex Code
    - 0x17


# 4. ZSH 설정

- `iterm2`에서 ZSH를 설치(`brew install zsh`) 
- `oh-my-zsh`을 설치(`sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`)
- `zsh` 플러그인을 설치
```bash
brew install zsh-completions fasd
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
```
- 설치된 플러그인을 ZSH에서 활성화 하기 위해서 `~/.zshrc` 파일의 `plugins` 항목에 `git`, `zsh-syntax-highlighting`, `zsh-autosuggestions`, `fasd` 를 설치
    - 주의 ,(Comma) 없이 넣어주면 됨

# 5. Python 설치
- 주로 사용하는 환경이 Python 이기 때문에 이에 맞는 설치를 진행

```jsx
brew install pyenv
```

```jsx
$ code .zshrc

// pyenv를 추가하세요.
plugins=(
    git
    zsh-syntax-highlighting
    zsh-autosuggestions
    fasd
    pyenv
)
```

```jsx
pyenv install 3.7.6
pyenv rehash
pyenv global 3.7.6
```

# 6. Notion, 1password 설치
- 이부분은 개인 취향 및 환경이 반영된 부분이기 때문에 참고만 해도 될 듯하다.

이렇게 내가 기본적으로 쓰는 환경을 정리해보았다.
시간이 지남에 따라 사용하는 부분도 많아질 테니 중간중간 업데이트할 예정이다.