# git-crypt

git-cryptでコードを暗号化します。

## 準備

### git-cryptのインストール

```
sudo apt udpate
sudo apt install -y git-crypt
```

### gitリポジトリの作成と設定

```
$ git init gitcrypt-test
Initialized empty Git repository in /home/ksaito/tmp/gitcrypt-test/.git/
$ cd gitcrypt-test/
$ git crypt init
Generating key...
$ git crypt add-gpg-user ID
```
