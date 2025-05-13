# Git、SSH環境構築

## 1. SSHの設定

1. `.ssh\config`ファイルを以下の内容で作成する

``` shell
Host github.com.mk # mk
  HostName github.com
  User git
  Port 22
  IdentityFile ~/.ssh/id_ed25519_mk
  TCPKeepAlive yes
  IdentitiesOnly yes

Host github.com.hiron # hiron
  HostName github.com
  User git
  Port 22
  IdentityFile ~/.ssh/id_ed25519_hiron
  TCPKeepAlive yes
  IdentitiesOnly yes

Host bitbucket.org # bb .bitbucket
  HostName bitbucket.org
  IdentityFile ~/.ssh/id_ed25519_bitbucket
  User git
  TCPKeepAlive yes
  IdentitiesOnly yes
```

2. ターミナル画面から、下記のコマンドを実行することで`~/.ssh`フォルダが作られる。（ファイル名、メールアドレスを指定しておく）  
```shell
ssh-keygen -t ed25519 -f id_ed25519_hiron -C hiron.de.dosukoi@gmail.com
```

3. `github.com`の `Settings|SSH and GPG keys`にて、公開鍵を登録する  

## 2. bashrcを編集

1. すでにある `.bashrc`ファイルに、下記の内容を追記する  

```shell
# hiron added.
alias cdg='cd /mnt/d/git/github.com/mk-system'
alias gs='git status'

gu()
{
  if [ $# -ne 1 ]; then
    echo 'Usage: gu [mk|hiron|bb]'
  elif [ "$1" = 'mk' ]; then
    git config --global user.name 'h-satoh'
    git config --global user.email 'h-satoh@mksc.jp'
    cdg
    echo switched to mksc account@github
  elif [ "$1" = 'hiron' ]; then
    git config --global user.name 'Hiron68k'
    git config --global user.email 'hiron.de.dosukoi@gmail.com'
    cd /mnt/d/git/github.com/hiron68k/
    echo switched to hiron@github
  else
    echo 'Usage: gu [mk|hiron|bb]'
  fi
}

gitdelbranch()
{
  git branch --merged|egrep -v '\*|develop|main|master'|xargs git branch -d
}
```

2. `source ~/.bashrc`を実行し、編集内容を有効化する  

## 3. git cloneするときの留意点

1. 下記のように、ホスト名を変更する必要がある

```shell
# githubから、clone用の文字列をコピーした状態
git clone git@github.com:hiron68k/edcb_recordmanager.git

# ホスト名を、ssh_configで設定した内容に置き換える
git clone git@github.com.hiron:hiron68k/edcb_recordmanager.git
```
