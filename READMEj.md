# PlantUML CGI

[English](/README.md) | Japanese

PlantUML_CGI は、PlantUML エンコードを png 画像に変換する CGI スクリプトです。
Apache と Python で動作し、非常にシンプルです。

## 前提条件

- plantuml.jar が使えるようになっていること。
- Apache サーバが起動し、cgi が有効になっていること。
- cgi で Python のコードを実行できること。

## インストール

plantuml を cgi置き場 に配置して下さい。
- 例： `/usr/lib/cgi-bin/plantuml`

## 使い方

上記でインストールした plantuml の CGI に、クエリパラメータ puencode で PlantUML エンコードを渡して実行してください。CGI は plantuml.jar でレンダリングして得られた png 画像のイメージを返します。例えば、ブラウザから下のようなURLへアクセスし、png 画像を得ます。
- 例： `http://<cgiサーバ>/<cgi置き場>/plantuml?puencode=<PlantUMLエンコード>`

## Redmine から PlantUML_CGI を利用する

https://github.com/gelin/plantuml-redmine-macro がシンプルで素晴らしい。
これをベースに、init.rb を修正して Redmine を再起動すれば PlantUML_CGI を利用できる。

1. plantuml-redmine-macro を導入する
2. plantuml-redmine-macro の init.rb の 
   ```ruby
   image_tag(URI.join(url, encoded).to_s)
   ```
   を
   ```ruby
   image_tag(URI.join(Setting.plugin_plantuml_macro['plantuml_url'], '?puencode=' + encoded).to_s)
   ```
   に修正する。
   ※ベースとなった init.rb のリビジョンは「658f599972125d928f9b718ec9e22c554590641b」
3. Redmine を再起動する
4. Redmine に管理者でログイン、管理からプラグインを開き plantuml-redmine-macro の設定ページから PlantUML_CGI のURLを指定する
   - 例： `http://XXX.XXX.XXX.XXX/cgi-bin/plantuml`

## モチベーション

ローカルサーバの Redmine 内で PlantUML を使う際、同サーバに PlantUML サーバを立てて利用したかった。
しかし apache maven、jetty、Tomcat、Docker、をインストールしたくなかった。
そこで、PlantUML をローカル実行できるようにし、シンプルに CGI で PlantUML サーバを実装することにした。

## Python のバージョン

Raspbian buster および Ubuntu 22.04 LTS のそれぞれでデフォルトの Python を使いたかったため、コードは Python2 でも Python3 でも動作するように実装した。

## 動作確認環境

| Hardware    | Raspberry-Pi 3B                | GPD-WIN          |
| ---         | ---                            | ---              |
| PRETTY_NAME | Raspbian GNU/Linux 10 (buster) | Ubuntu 22.04 LTS |
| Python      | 2.7.16                         | 3.10.4           |
| PlantUML    | 1.2022.13                      | 1.2022.13        |
| PostgreSQL  | 11.14                          | 14.4             |
| Redmine     | 4.2.3                          | 5.0.2            |
| Ruby        | 2.7.4                          | 3.1.2            |
| Apache      | 2.4.38                         | 2.4.52           |
| gem         | 3.1.6                          | 3.3.7            |
| passenger   | 6.0.15                         | 6.0.14           |
| Graphviz    | 2.40.1                         | 2.43.0           |
| Java        | 11.0.16                        | 11.0.17          |

## 設計

アプリケーション間通信

![Sequence Diagram](http://www.plantuml.com/plantuml/png/ymZnzL7GjLCeo4dCAodDpL6mKWW0CKE1mgvvoVafgLo9oIMPPOabgN0rN306ciQKL91w5DcIrDo2jCoSLA1iVa5g7jmic0HErM2JO7naU_Io4ekWyd3JK2Iva3beBYp8I-TAISMd3TCXEVd5gK1D-5qE2aRSvWC0 "PlantUML_CGI")

エンコード/デコード

![Encode/Decode](http://www.plantuml.com/plantuml/png/VL0z3u8m4DtxAnec66GY30uEHZTDN9n9eHT3KjgcFHP_lGUAoALnQdhtFkuzhmBsNU-LHPdT33ttwqLsJaCcqhkdwLksEwe8TIN1_kCjM_48RlGoEt_-p5Nk3jnCxkUtxDpW0yIO5u8ZYCIk858x3ygshjupud7GncnbHWmb1cMZKGWv2JJe6Z_Xni4K0gp-nZW1Z_6ZpUsuyY8voPDByZuMTPDBp-RfFbYlIuaQrXgd82y0 "PlantUML Encode/Decode")

## References

- PlantUML Server 公式ページ
  https://plantuml.com/ja/server
- PlantUML Server ソースコード
  https://github.com/plantuml/plantuml-server
- PlantUML テキスト エンコード
  https://plantuml.com/ja/text-encoding
- PlantUML Server
  http://www.plantuml.com/plantuml
- plantuml-redmine-macro
  https://github.com/gelin/plantuml-redmine-macro

## License

[LICENSE](/LICENSE)
