* 訪問するのに最低限必要なサイトを作る
先の章で見つけた問題を速やかに解決しよう。

  * テスト後処理(Isolation)をファンクショナルテスト内で保証する

    * 前の章で古典的なテストの問題に直面した、ファンクショナルテストとの分離（後処理の実行）をどうするか。
    * ユニットテストはDjangoのtest runnerが自動で新しいデータベースを作成し、個別のテストごとに安全にリセットしてくれる。しかし、ファンクショナルテストでは「本物の」データベースを走らせている。
    * 対応のひとつは"roll our own"という手法で、functional_tests.pyにコードを足してクリーンナップを行う。setUpとtearDownメソッドはこの順序を完璧にこなしてくれる。
    * Django 1.4からはLiveServerTestCaseが導入されている。これは(ユニットテストのように)テスト用データベースを自動で作成してくれる。
    * LiveServerTestCaseはmanage.pyのtest runnerで実行される。Django 1.6ではtestで始まるどんなファイルもtest runnerが見つける。
    * アプリのようなfunctional_testsフォルダを作成する。Djangoは__init__.pyがある正しいPythonパッケージであることを要求する。
    ```
    $ mkdir functional_tests
    $ touch functional_tests/__init__.py
    ```
    * git mvを使ってfunctional_tests.pyをfunctional_tests/tests.pyにリネームしながら移動させる。
    ```
    $ git mv functional_tests.py functional_tests/tests.py
    $ git status
    ```
    * これからはmanage.pyでファンクショナルテストを行う
    ```
    $ python3 manage.py test functional_tests
    ```
    * ファンクショナルテストはユーザー視点なのでアプリをまたいで良い。listsアプリのテストを含むということ。
    * functional_tests/tests.pyにLiveServertestCaseを使うようNewVisitorTestクラスを変更しよう。
    * localhost port 8000とハードコーディングされていた箇所はLiveServerTestCaseが提供する属性のlive_server_urlとする。
    * Djangoのtest runnerが面倒をみてくれるため、 __name__=='__main__'を削除してもよい
    ```
    $ git status
    $ git add functional_tests
    $ git diff --staged -M
    $ git commit
    ```
    * git diffの-Mフラグは便利です。移動を検出するという意味で、fuctional_tests.pyとfunctional_tests/tests.pyが同じファイルてあることに気がつきます。この方がいい感じのdiffを見ることができます。(試してみると、かなり効果覿面だった。フラグなしだと別ファイルとして全行緑や赤となる)

  * ユニットテストだけを走らせる

    * いまでは manage.py testを実行するとDjangoはファンクショナルテストとユニットテストの両方を走らせます
    * ユニットテストを行いたい時は、明示的にlistsアプリを指摘して実行るすこと
    * ファンクショナルテストを走らせる
    ```
    $ python3 manage.py test functional_tests
    ```
    * ユニットテストを走らせる
    ```
    $ python3 manage.py test lists
    ```
    * ファンクショナルテストの意図的に失敗させている箇所に、複数リスト対応のシナリオが書いてあります。これを実装していこう。

  * 必要な時に小さな設計(今日はここまでPDF P.115)