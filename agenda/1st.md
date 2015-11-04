# 第1回 Gitハンズオン勉強会

## アジェンダ

1. 本勉強会についての説明
1. Git、Githubについて簡単な説明
1. 基本的なGitコマンド操作の学習①
    * リモートリポジトリのcloneからpushまで
    * ブランチを切って、merge、pushまで
1. 基本的なGitコマンド操作の学習② （※時間があれば）
    * ローカルリポジトリの作成からリモートリポジトリへのpushまで
1. 質疑応答（随時）

## What is Git ? What is Github ?

### Git

* https://git-scm.com/
* 分散型バージョン管理システム
* 全履歴を含んだリポジトリの完全な複製をローカルに持つ
    * リモートリポジトリ、ローカルリポジトリ
    * オフラインでも作業が可能
* 柔軟なブランチ操作
* 柔軟なワークフロー
    * Git Flow
    * Github Flow

### Github

* https://github.com/
* ソフトウェア開発プロジェクトのためのソース管理・共有ウェブサービス
    * 現代のソース管理・共有サービスのデファクト
* バージョン管理システムにGitを採用
* 分かりやすいUI
* Issue によるタスク管理
* Pull Request により誰でもソースコードへのcontributeが可能
    * 開発者同士のコミュニケーション
    * コードレビューの文化
* API 及び Webhook による多様な外部サービスとの連携

## 基本的なGitコマンド操作

### 事前準備

* Github上に、`study-git`リポジトリを作成する
* `~/.gitconfig`を作成

    ```
    [user]
        name = Githubのユーザ名
        email = Githubのメールアドレス
    [color]
        ui = auto
    [merge]
        ff = false
    ```

### リモートリポジトリのcloneからpushまで

#### 1. リモートリポジトリのclone

リモートリポジトリをローカルに複製します。

```bash
git clone <リモートリポジトリのURL>
```

*実行例*

```bash
# リモートリポジトリをclone
git clone https://github.com/kawanamiyuu/study-git.git
cd study-git

# リモートリポジトリの情報を確認
git remote -v

# リポジトリに含まれるブランチを確認
git branch -a
```

#### 2. ソースコードの編集

*実行例*

```bash
cd study-git
vi README.md
```

#### 3. ローカルリポジトリへのcommit

```bash
git add [<file> ...]
git commit [-m "コミットメッセージ"]
```

2つのコマンドにはそれぞれ

* `git add` →　変更したファイルをステージングエリアに追加し、次回のコミットの対象とする
* `git commit` → ステージングエリアにあるファイルをローカルリポジトリにコミットする

という意味がありますが、`add`と`commit`は通常セットで実行するため、あまり厳密に考える必要はありません。

*実行例*

```bash
# 変更が発生したファイルを確認
git status

# コミット
git add .
git commit

# コミットログを確認
git log --stat
```

* `git add .`とすると、変更したすべてのファイルがコミットの対象になります
* コミットメッセージを省略して`git commit`を実行すると、コミットメッセージ入力画面(`vi`)が立ち上がります
    

#### 4. リモートリポジトリへのpush

ローカルで行った変更をリモートリポジトリに反映します。

```bash
git push origin <ブランチ名>
```

*実行例*

```bash
git push origin master
```

Githubのリポジトリに、コミットした内容が反映されていることを確認してください。

### ブランチを切って、merge、pushまで

次の操作を行います。

1. masterブランチからdevelopブランチを切る
1. developブランチでソースコードを編集し、developブランチにcommitする
1. developブランチをmasterブランチにマージする
1. masterブランチをGithubにpushする

模式図

```
           1                     3
master ----x---------->----------o---->
            \                   /
develop      >--------o-------->
                      2
```


#### 1. ブランチを切る

```bash
git checkout -b <new branch> [<base branch>]
```

* `base branch`から`new branch`を作成し、作業ブランチを`new branch`に切り替えます
* `base branch`を省略した場合、カレントブランチから`new branch`を作成します

*実行例*

```bash
# カレントブランチ(master)からdevelopブランチを作成し、作業ブランチをdevelopブランチに切り替え
git checkout -b develop

# ブランチ確認
# *が付いているのがカレントブランチ
git branch
* develop
  master
```

##### (補足) 

上記の`git checkout -b <new branch> [<base branch>]`は次の2つのコマンドを連続して実行したのと同じ

```bash
git branch <new branch> [<base branch>]
git checkout <new branch>
```

*実行例*

```bash
# masterブランチからdevelopブランチを作成
git branch develop master

# ブランチ確認（カレントブランチはまだmaster）
git branch
  develop
* master

# 作業ブランチをdevelopブランチに切り替え
git checkout develop

# ブランチ確認（カレントブランチはdevelop）
git branch
* develop
  master
```

#### 2. 作成したブランチでソースコードの編集＆commit

*実行例*

```bash
git checkout develop

# 既存のファイルを更新
vi README.md
# 新しいファイルを追加
touch newfile.txt

# 変更が発生したファイルを確認
git status

# diffを確認
git diff

git add .
git commit
```

#### 3. ブランチのmerge

```bash
git merge <ブランチ名>
```

* 指定したブランチをカレントブランチにマージコミットします
* マージコミットメッセージ入力画面が立ち上がります

*実行例*

```bash
# masterブランチに切り替え
git checkout master

# 変更前のソースコードであることを確認
# README.mdが未更新、newfile.txtが存在しない
less README.md
ls .

# developブランチとのdiffを確認
git diff develop

# developブランチをmasterブランチにマージ
git merge develop

# 変更後のソースコードであることを確認
# README.mdが更新済み、newfile.txtが存在する
less README.md
ls .

# ログを確認
git log --graph --oneline
```

#### 4. リモートリポジトリへのpush

*実行例*

```bash
git push origin master

# マージ済みの不要なブランチを削除
git branch -d develop
```

Githubのリポジトリに、マージしたブランチの内容が反映されていることを確認してください。

### ローカルリポジトリの作成からリモートリポジトリへのpushまで

#### 1. リモートリポジトリの作成

* Github上に`study-git2`リポジトリを作成します

#### 2.  ローカルリポジトリの作成

カンレントディレクトリをGitリポジトリとして初期化します。

```bash
git init
```

*実行例*

```bash
mkdir study-git2
cd study-git2
git init
```

#### 3. ソースコードの編集＆commit

*実行例*

```bash
vi README.md
git add .
git commit
```

#### 4. リモートリポジトリの登録

ローカルリポジトリに対応する、リモートリポジトリのURLを登録します。

```bash
git remote add origin <リモートリポジトリのURL>
```

*実行例*

```bash
# リモートリポジトリの情報を確認（結果は空）
git remote -v

# originとしてリモートリポジトリを登録
git remote add origin https://github.com/kawanamiyuu/study-git2.git

# リモートリポジトリの情報を確認
git remote -v
```

#### 5. リモートリポジトリへのpush

*実行例*

```bash
git push origin master
```

Githubのリポジトリに反映されていることを確認してください。

## 勉強会中、アンケートでの質問と回答

* **Q.** 各個人の環境にもリポジトリがあること（分散リポジトリ）のメリットが良く分からない
    * **A.** 各個人の環境で履歴管理できるため、開発中のソースコードでもこまめにコミットし差分を管理する事ができる。
（たとえば、ある機能開発のうち、ある部分の修正は完了しているが、別の部分では色々と試行錯誤しながら実装している
場合など）
SVN等の中央サーバーで唯一の履歴を管理する形では、コミットが他の開発者全員に影響してしまうため、こうした開発中のこまめなコミットがし辛い。

* **Q.** Windowsでファイルを`git add`する際、改行コードについて警告が出た
    * **A.** gitには改行コードを自動変換する機能があり、期待する改行コードない場合、警告が表示されます。
Windows上でgitを使う場合、デフォルト設定では改行コードは自動で`CRLF`に変換されます。
実運用ではリポジトリは`LF`で管理する方が良いので、改行コードの変換をOFFにする[設定](https://git-scm.com/book/ja/v2/Git-%E3%81%AE%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%9E%E3%82%A4%E3%82%BA-Git-%E3%81%AE%E8%A8%AD%E5%AE%9A#書式設定と空白文字)が必要です。

* **Q.** コミットした覚えの無いログ「Initial commit」がコミットログにあるが、これは一体何か？
    * **A.** Github上でリポジトリを新規作成した際に自動でコミットされたもの。
リポジトリ作成時に、README.mdを生成するオプションをONにすると、このコミットが自動で行われる。

* **Q.** `origin`や`master`といった名前は一切入力した覚えが無いが、これは何か？
    * **A.** `origin`はリモートリポジトリのURLに対する短縮名で慣例的に良く使用されるデフォルト名。`git clone`する際にgitコマンドが自動で名前を設定する。
`master`は本流ブランチ（svnでのtrunkにあたる）の名称として慣例的に使用されるデフォルト名で、`git init`する際にgitが自動で作成するブランチ。

* **Q.** ~/.gitconfig って結局なんなの？
    * **A.** ログインユーザーが実行するgitコマンド全体に対して有効になるgitの設定ファイル。
各リポジトリ毎に設定する場合は、ワーキングディレクトリ直下に`.git/config`を設定すれば、個別に有効になる。

* **Q.** `git remote add`する際にリポジトリ名「origin」をスペルミスしてしまった。どうすれば直せるのか？
    * **A.** `git remote rename <old> <new>`

* **Q.** コミットするたびにGithubのユーザーIDとパスワードを聞かれるが、公開鍵認証などにはできないのか？
    * **A.** 可能。Github上で端末の公開鍵を登録すれば、パスワード認証なしで運用する事ができる。

* **Q.** コミットコメントなしでcommitすることはできない？
    * **A.** はい。できません。commit実行しても警告メッセージが表示されコミットは行われません。

* **Q.** 変更したファイルをコミットせずにブランチを切り替えたら、切り替え先のブランチでもそのファイルが変更された状態だった
    * **A.** 変更したファイルをコミットせずにブランチを切り替えると、その変更は切り替え先のブランチにそのまま移行します。
ブランチAでの変更はまだコミットしたくないけど、ブランチBに切り替えたい場合は、`git stash`コマンドで、ブランチAでの変更を一時的に退避しておく必要があります。

    *実行例*

    ```bash
    git checkout branch-A
    　
    # (ブランチAのソースコードを変更)
    　
    # ブランチAの変更を"スタック"と呼ばれる領域に一時退避
    git stash
    　
    # ブランチBに切り替え
    git checkout branch-B
    　
    # (ブランチBで作業)
    　
    # ブランチAに切り替え
    git checkout branch-A
    　
    # 一時退避していた変更をワーキングディレクトリに適用
    git stash apply
    　
    # (git stashする前の状態から作業を再開できる)
    ```
* **Q.** `git stash`を使用するのはどのようなケースですか？
    * **A.** 様々な用途で使用できますが、自分の担当機能の開発でソースコードを編集中に、別の誰かのコードレビューを依頼された場合や、不具合調査のために別のブランチに切り替える必要が生じた場合などに、一時的に自身の作業を退避させるケースが多いと思います。

* **Q.** `git stash apply` と `git stahs pop`の使い分けは？
    * **A.** 厳密なルールはありません。`apply`も`pop`も双方一時的に退避させていた変更を復活させるコマンドですが、復活させる際にstash領域に残り続けるか、削除されるかの違いがあります。（講師は）操作ミスなどで変更を失ってしまうのが怖い場合などは`apply`、そうでない場合は`pop`を使用しています。

* **Q.** 誤って`git add`してしまったファイルを元に戻したい場合はどうすれば良い？
    * **A.** `git reset HEAD <ファイル名>`で`git add`する前の状態に戻すことができます。同じようなケースで、一度コミットしてしまったファイルをリポジトリの管理対象から外す場合は`git rm --cached <ファイル名>`を使用します。

* **Q.** `git branch -D`を使うのはどんなケースですか？
    * **A.** 様々なケースが想定されますが、あるブランチにおいて一時的にファイルの変更を行ったものの、ブランチ自体が不要になってしまった場合などが相当します。Gitを使用して開発を行う場合、どのような些細な変更でもブランチを作成してから行うことが多く、たとえば実験的にソースコードの一部を変更して動作を確認する、といったような場合にもブランチを作成したりするので、そのような場合に-Dオプションを使用する事が多いかと思います。

* **Q.** `git clone`したディレクトリの名称を変更しても大丈夫か？
    * **A.** 問題ありません。トップ階層のディレクトリ名の変更はGitの振る舞いに影響を与えません。

## その他の補足

* よく使うコマンドは~/.gitconfigにaliasを設定しておくと便利

    ```
    [alias]
        st = status
        co = checkout
        cm = commit
        fh = fetch
        mg = merge
        ba = branch -a
        tree = log --graph --date=short --pretty='format:%C(yellow)%h%Creset %s %Cgreen(%an)%Creset %Cred%d%Creset'
    ```
    
    **実行例**
    
    ```bash
    # git checkout -b develop
    git co -b develop
    ```
    
    ```bash
    # git logをブランチの分岐も含めてグラフィカルに表示
    git tree
    ```
