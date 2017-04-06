# serverspec

大きく分けると二つに分かれる

|名前|役割|
|:--|:--|
|serverspec|SpecinfraをRSpecのDSLで呼び出すためのラッパー|
|Specinfra|OSの違いや実行形式の違いを吸収してくれるコマンド実行レイヤー|

* serverspec
  * Matcher
    * serverspec/matcher 以下のコード
  * Resource Type
    * serverspec/resource 以下のコード（fileやpackageなど）
    * specinfra/runner.rbを呼び出す
* Specinfra
  * Runner
    * specinfra/processor.rbを呼び出す
  * Processor
    * バックエンドでのコマンド実行結果を処理するためのレイヤ
    * serverspecが実行した結果にさらに処理を行う必要があるときのみ動く
  * Command
  * Backend
    * commandレイヤーから取得したテスト対象ホストのOSに応じたコマンドを、指定された実行形式に応じて実行する