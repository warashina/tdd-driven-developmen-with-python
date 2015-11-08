"Test Driven Development with Python" 学習用ノート
#####################################################

個人的な復習用にノートを作成しています。日本語を母語とする読者が本書購入検討のをする際に助けになれば幸いです。

* まえがき

  * Python 3を利用する
  * HTMLとJavaScriptに関して少し理解があることを求める
  * 必要なソフトウェアのインストール

    * Firefox

      Seleniumが標準で操作できる

    * Git

      Windowsの場合Git for Windowsに付属するGit Bashがシェルとして有用

    * Python 3

      WindowsやMacは公式のインストーラを使用する

    * Gitのデフォルトエディタと基本的な設定(git config --global user.name と git config --global user.email)
    * pip(pip3)でパッケージ導入

      * django
      * selenium

  * IDEは使わないことを推奨

    この本が終わったら使いましょう。

* Part I. The Basics of TDD and Django

  コピペは避けよう。写経して"your muscle memory"に刻み込んだほうがいい。どうせPDFからコピペしてもろくなことにならないしね。

#. ファンクショナル・テストを使ってDjangoをセットアップしよう

  * Testing Goatに従え
  * functional_test.pyというファンクショナル・テスト用のスクリプトを作成する

    * seleniumでFireFoxの起動とページの表示を行う
    * assertでページタイトルをテストする。"Django"の文字列が含まれるか
    * 最初に書いたテストは予想通り失敗する。これでアプリを作り始めてよい

  * $ django-admin.py startproject project-nameでプロジェクト作成。プロジェクト名でネストされたディレクトリができるが混乱しないように。上位のプロジェクト名ディレクトリが作業ディレクトリになる
  * manage.py はDjangoにおける便利道具
  * $ python3 manage.py runserverでローカルでDjangoが走る
  * 今度のfunctional_test.pyはFirefoxを起動する。Djangoが動いている。何も出力しない
  * 成果をgitにコミットする

    * gitリポジトリを初期化する
    * "db.sqlite3","___pycache__","\*.pyc"を.gitignoreに加える。すでに"git add"したものは"git rm --cached"する
    * git statusしてからgit addおよびgit commit

#. unittestモジュールを使ってファンクショナル・テストを拡張する

  * To-Doリストアプリをつくることにする
  * ファンクショナル・テスト(呼び方は色々ある)は人間が読めるストーリーを持っている必要がある
  * コメントの形でfunctional_test.pyに最小限の利用ストーリーを書く。テストは今まで書いたものをストーリーに当てはめ、少し変更した部分以外は足していない
  * しかし変更部分があるのでテストは失敗する
  * assertのみでは表示がわかりにくく、またテストの前処理や後処理に不便なのでunittestモジュールを使う
  * functional_test.pyにunittestモジュールを導入する

    * unittest.TestCaseを継承したクラス
    * setUp(self)とtearDown(self)
    * test_で始まる名前を持ったメソッド
    * unittestのassertInやfail
    * "if __name__ == '__main__':"
    * unittest.main(warnings='ignore')の引数は省略可能。試す

  * Seleniumのimplicitly_wait()
  * gitにcommmitする

    * git status
    * git diff
    * git commit -a

#. ユニットテストによるシンプルなホームページのテスト
#. Chapter 4
