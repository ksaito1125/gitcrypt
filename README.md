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

## gitのコミットに署名

下記の結果をGitHubに登録します。

```
gpg --armor --export <HASH>
```

```
git config user.signingkey <HASH>
GPG_TTY=$(tty)
export GPG_TTY
git commit -S -m test
```

```
$ echo test | gpg --clearsign
-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA512

test
-----BEGIN PGP SIGNATURE-----

iQIzBAEBCgAdFiEE1fay9HrndAM0rjtxWo5ssOGpk5sFAl2u3sEACgkQWo5ssOGp
k5vAaw/+KwQ+Yy8SryBekRSJQfPJSieVz7eYDUtdC1ACU8/lGULj6iVxWJQzXOx8
7qO2p0nCsYlNKZ/sRWiHlQ1LhrNMihj9XYA7CNizQT0S6Gufp74WMmzuvOf23m9G
THFGPOyEHaCxT1UQmBnzWViL4h6+e1IBMlZXWxNcZJ/FXbtQH9TWb7/mV9uBarN6
IvG54Yix6A/+BHdXV/E2cvwVvdH2MyE6ixS8Ki+ilvIYykB2Z24f70+gAV0sHL79
R76pcEs/gG8nFugCAYyzM1ClOa2JtgDpPFs4e0T9zvu9T0lzmKju0ut1TkvRgJG0
LDDR5yoznfnosMyR5sWRl4rIhKxRCx3UNy3fsQ0hP/mbIifL6987Tczdz8J58BRr
sRr9b7ZSr7ERtQTu7SSr6D0xRd3kpsS7he4A+k0WJ3yfwVHZTJZ7JYUjOt/MkYR7
mnYA4WLZsXwXYbzLnIWgsA7WvqjqWlucHJv1ZcyWcwwBmmbq3fYAWGlroviNkVHU
8ncCPHrx00xfJ5mElM6kAdYSlUkE6sRYSxH2JVKZCnrtg700bvUXKK9tS4RP0ZOm
Wr4oNhWxjkYerk5LThV6+dnDvxGX3Q4oOtvp4tK/Y2kVmzY1D3Z+hECw2GuUZisQ
MEN5wuIZVF11P8049yS5lYNKgScwAcP/ga1gNTt1d256HA8Gkdk=
=7Rkp
-----END PGP SIGNATURE-----
```

以上
