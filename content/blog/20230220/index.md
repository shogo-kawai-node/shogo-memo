---
title: gitで複数アカウントメモ
date: "2023-02-20T09:14:00.000Z"
description: gitで複数アカウント際のメモです。
tags: ["Git"]
---


## 公開鍵と秘密鍵を作成
メインは作成済み前提です。
```sh
$cd ~/.ssh
$ssh-keygen
 Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/(ユーザー名)/.ssh/id_rsa):id_rsa_sub
```
## 公開鍵を登録

```sh
cat id_rsa_sub.pub
```

## ~/.ssh/config にサブアカウントの情報を追加

```sh
#メインアカウント
Host github #任意のホスト名
  HostName github.com
  IdentityFile ~/.ssh/id_rsa #メインアカウントの鍵のファイル
  User git
  Port 22
  TCPKeepAlive yes
  IdentitiesOnly yes

#サブアカウント
Host github-sub #任意のホスト名
  HostName github.com
  IdentityFile ~/.ssh/id_sub_rsa #サブアカウントの鍵のファイル
  User git
  Port 22
  TCPKeepAlive yes
  IdentitiesOnly yes
```

## GitHubと SSH 接続ができているか確認

```sh
ssh -T github
Hi (メインのGitHubのアカウント名)! You've successfully authenticated, but GitHub does not provide she
ll access.

ssh -T github-sub
Hi (追加したいGitHubのアカウント名)! You've successfully authenticated, but GitHub does not provide shell
 access.
```

## .gitconfigファイル修正
```sh
$ cat .gitconfig
[user]
        email = 元からあるGitHubアカウントのユーザー名
        name = 元からあるGitHubアカウントのメールアドレス

[includeIf "gitdir:~/private/"]
    path = ~/.gitconfig_sub
$ cat .gitconfig_sub
[user]
        email = 追加したいGitHubアカウントのユーザー名
        name = 追加したいGitHubアカウントのメールアドレス
```

## git init

```sh
$ cd ~/private/
$ git init
$ git config user.name
$ git config user.email
```

## git remote
```sh
$ git remote add origin github-sub:ユーザー名/リポジトリ名.git
$ git remote -v
```
