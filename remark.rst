* おさらい

この本を使用した学習から2ヶ月離れていました。久しぶりに取り組むため、情報を整理します。

** 何を勉強していたんだっけ?

テスト駆動開発の手法でPythonを使ったWebアプリを作成していた。

** どうやって?

*** 使っているツールなど

Pythonは3系、WebフレームワークはDjango、テストはSelenium WebdriverでFirefoxを自動操作していた。

djangoやseleniumの導入にはpipを使っているが、virtualvenvは使っていない。

テストもコードも、折々でgitにコミットしている。

*** 作業

failするテストを書く、テストを実行して失敗を確認する、テストの部分を実装する、テストを実行する、の繰り返しである。

大きいループとしてfunctional testsが、小さいループとしてunit testsを行う。それぞれ、unittestモジュールをインポートして書いたfunctional_tests.pyとDjangoのdjango.testをインポートして書いたtests.pyにテストを書いている。

*** Djangoプロジェクトとアプリについて

プロジェクトとアプリを混同しやすい。

まずはプロジェクトを作成する。プロジェクトの名前はここではsuperlistである。manage.pyを含む作業ディレクトリの名前と、urls.pyなどを含むディレクトリの両方がこの名前である。ここはちょっと後で復習する。

次にDjangoアプリを作成する。ここではアプリはlistsである。

今までは、主にアプリのviews.pyを実装している。ここに1つの画面に対応するレンダーメソッドを実装している。引数はリクエストオブジェクト、これとテンプレートを処理して、HTTpReqaponseオブジェクトを返す。

テンプレートはアプリのtemplatesディレクトリにhtmlファイルとして作成する。

プロジェクトにあるulrs.pyでアプリのviewsをインポートし、urlの正規表現とviewsにあるレンダーメソッドのマップを追加する。

** どこまで進んでいた？

Chapter 5: Saving User Inputの途中までである。
そう、MVCフレームワークのうち、View(テンプレートが相当する)とController(views.pyやurls.pyが相当する)までしか触っていないのだ。

かなり重複部分があるとは思うが、Chapter 5の初めから再開しよう。

具体的な再開箇所はfunctional_test.pyやtests.pyの内容、実行結果でわかってくるはずである。

** 多様するコマンド

開発サーバーの実行
```
$ python3 manage.py runserver
```

functional test
```
$ python3 functional_test.py
```

ユニットテスト
```
$ python3 manage.py test 
```
