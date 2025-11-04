---
title: 'Astroブログの作り方とNetlifyでの公開手順'
description: 'SSGのAstroとNetlifyを使い、無料で高速な技術ブログを立ち上げる全手順を解説します。GitHub連携から独自ドメイン設定まで、実際の手順をまとめました。'
pubDate: '2025-11-04'
# heroImage: '/images/astro-netlify-hero.png' 
# (画像が不要な場合は、この行は削除またはコメントアウトしたままにします)
---

## はじめに

こんにちは、TechBulkです。
このブログは、高速なSSG（静的サイトジェネレーター）である **Astro** で作成し、**Netlify** というホスティングサービスで無料公開しています。

この記事では、私自身が行った「**Astro + Netlify で技術ブログを公開するまで**」の全手順を、備忘録としてまとめます。

## 1. 全体の流れ

大まかな流れは以下の通りです。
1.  ローカルPCでAstroのひな形を作成
2.  GitHubにソースコードをアップロード
3.  NetlifyとGitHubを連携させて自動デプロイ
4.  （おまけ）URLのカスタマイズ

## 2. 準備したもの

### 必要なツール
* Node.js (Astroを動かすため)
* Git (GitHubにアップするため)
* VSCode (お好みのエディタ)

### 必要なアカウント（Webサービス）
* GitHub アカウント
* Netlify アカウント (GitHub連携でOK)

## 3. 手順1：Astroプロジェクトの作成

まず、ローカルPCでAstroのコマンドを実行し、ブログのひな形を作ります。

```bash
# ターミナルで実行
npm create astro@latest

#上のコマンドを実行すると以下のようにフォルダを作成するように表示されます
> npx
> create-astro


 astro   Launch sequence initiated.

   dir   Where should we create your new project?
         ./former-force

```


入力をすると次にプロジェクトを以下から選択します。ブログの作成の場合"Use blog template"を選択します。

```bash
 tmpl   How would you like to start your new project?
         > A basic, helpful starter project (recommended)
         — Use blog template
         — Use docs (Starlight) template
         — Use minimal (empty) template

        #選択した後は以下の内容についてYESを選択

  deps   Install dependencies?
         Yes

   git   Initialize a new git repository?
         Yes

```
以上でローカル環境でのプロジェクトの作成は完了しました。
ここで以下のコマンドを実行すると、ローカルの開発サーバーが起動し、ターミナルにローカルサーバーのURLが表示されるのでそこにアクセスをすると確認できます。
```bash
npm run dev
#ブラウザで http://localhost:4321 などアクセス
```


## 4. 手順2：GitHubへのアップロード
ローカルPCで作成したプロジェクト（ブログのひな形）を、Netlifyと連携させるためにGitHubにアップロードします。

### 4-1. GitHubで空のリポジトリを作成
まず、GitHub にアクセスし、新しいリポジトリを作成します。

右上の「＋」から「New repository」を選択します。

Repository name を決めます（例: my-tech-blog）。

Public を選択します（Netlifyの無料プランで連携させるため）。

「Add a README file」などのチェックはすべて外した状態で、「Create repository」ボタンを押します。

### 4-2. ローカルリポジトリとGitHubを紐付け
リポジトリを作成すると、…or push an existing repository from the command line という見出しの下に、実行すべきコマンドが表示されます。

VSCodeのターミナル（npm run dev を Ctrl + C で停止した後）で、以下のコマンドを（表示された内容に合わせて）実行します。

```bash

# 1. リモート（GitHub）の場所を 'origin' という名前で登録
# ※ URLはご自身のGitHubのものに書き換えてください
git remote add origin https://github.com/TechBulk/my-tech-blog.git

# 2. ブランチ名を 'main' に変更 (Astroの初期設定で Git init 済みのため)
git branch -M main

# 3. GitHubにプッシュ（アップロード）
git push -u origin main
```

これで、GitHubのリポジトリページをリロードして、Astroのファイル（astro.config.mjs など ）が表示されていれば成功です。

## 5. 手順3：Netlifyでのデプロイ
いよいよ、GitHubにアップロードしたソースコードをNetlifyで公開します。

Netlify にアクセスし、GitHubアカウントでログインします。

ダッシュボードで、「Add new site」→「Import an existing project」を選択します。

Git provider として「GitHub」を選択します。

リポジトリの一覧が表示されるので、先ほど作成したリポジトリ（my-tech-blog）を選択します。

ビルド設定の画面が表示されます。NetlifyがAstroのプロジェクトであることを自動で検知してくれるため、以下の設定が自動入力されているはずです。

**Build command: npm run build**

**Publish directory: dist**

上記を確認し、そのまま「**Deploy site**」ボタンをクリックします。

数分待つとデプロイが完了し、[ランダムな文字列].netlify.app というURLが発行されます。 このURLにアクセスし、ローカルで確認したものと同じサイトが表示されていれば、公開完了です。

## 6. （おまけ）URLのカスタマイズ
発行された [ランダムな文字列].netlify.app というURLは、分かりにくいものです。 Netlifyの「Site configuration」>「Change site name」から、この [ランダムな文字列] の部分を好きな名前に変更できます。

私は techbulk.netlify.app に変更しました。

## 7. まとめ

以上が、AstroとNetlifyを使って技術ブログを公開する全手順です。
一度この連携（Git Push → 自動デプロイ）を構築してしまえば、あとはローカルで記事（.md ファイル）を追加・編集して git push するだけで、自動で本番サイトが更新されるようになります。