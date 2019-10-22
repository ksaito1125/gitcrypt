# git-crypt

git-cryptでコードを暗号化します。

## 準備

### git-cryptのインストール

```
sudo apt udpate
sudo apt install -y git-crypt
```

### gitリポジトリの作成と設定

gitリポジトリは、通常と同じように作成します。

```
git init gitcrypt-test
cd gitcrypt-test/
```

git-cryptを初期化して暗号化用の公開鍵を設定指定します。

```
$ git crypt init
Generating key...
$ git crypt add-gpg-user ID
[master 2a11651] Add 1 git-crypt collaborator
 2 files changed, 4 insertions(+)
 create mode 100644 .git-crypt/.gitattributes
 create mode 100644 .git-crypt/keys/default/0/***.gpg
$
```

## 暗号化するファイルの作成と設定

``.gitattributes``に暗号化するファイルを指定してファイルと共にコミットします。

```
echo 'Hello, git-crypt!' > secrets.txt
echo 'secrets.txt filter=git-crypt diff=git-crypt' >> .gitattributes
git add secrets.txt .gitattributes
git commit
```

## テスト

``git crypt status``で、どのファイルが暗号化されているか確認できます。

作業領域のファイルは、シームレスに暗号化／復号化されます。
``git crypt lock/unlock``コマンドで、作業領域のファイルも暗号化／復号化をすることができます。

```
$ cat secrets.txt 
Hello, git-crypt!
$ git crypt lock
$ cat 
git crypt lock
$ echo $(cat -v secrets.txt)
^@GITCRYPT^@^Q NM-^E]M-,M-^^M-qM-Bh^GMM-RM-PM-jM-^LM-B{M-$M-DM-"M-fM-tM-0M-rNM-!M-vM-:M-c
$ git crypt unlock
$ cat secrets.txt 
Hello, git-crypt!
$
```

以上
