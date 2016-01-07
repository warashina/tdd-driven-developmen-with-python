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