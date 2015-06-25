レールに乗る
============

コンテクスト
------------

機能を設計または実装しようとしている。

問題
----

実装したものの、Railsが想定外の処理を行い、解決のために調査したり余分なコードを追加したりして時間を浪費する。

問題の原因
----------

Railsは特定の実装には最適な方法があるという仮定に基いて設計されており、その仮定に沿った実装を[推奨している](http://guides.rubyonrails.org/getting_started.html)。
Convention over Configurationと呼ばれるソフトウェア設計のポリシーもその一部であり、例えばデータベースの「products」テーブルを扱うクラスの名前は「Product」を前提にする。
Railsではこのような「Rails Way」と呼ばれる慣例に従うことでコードの量が最小になるように作られている。
Rails Wayの範囲はデータベース層からプレゼンテーション層まで全域に広がっており、開発者が把握していない部分でRails Wayに反した決定をしてしまうと無駄が発生する。

フォース
--------

* Rails WayはRailsの開発思想そのものであり、変更できない。
* Rails開発の入門者には開発思想が定着していないばかりでなく、開発思想の存在すら知らないことがある。

解決方法
--------

他のフレームワークでは自由に選択できる設計や実装であっても、Rails Wayがないか確認し、可能な限り従う。

例
--

#### 1. 命名規則

命名規則はRails Wayの代表である。
例えば、クラスの定義を含むファイルのパスはクラス名を元に走査される。
逆に、クラス名から推測不能なパスにファイルを置き、明示的にパスを指定しなければLoadErrorが発生する。
よって、特に理由がなければ命名規則に従うべきである。

#### 2. カラムの予約語

データベースのカラムに予約語がある。
「type」というカラムは、一つのテーブルに複数の型を格納する仕組み（Single Table Inheritance）に使われるため、それ以外の用途では避けなければならない。

#### 3. Assets Pipeline

JavaScriptとスタイルシートは、開発段階では複数のファイルに分けておけるが、運用時には転送量削減のためにAssets Pipelineという機構によって自動的に圧縮、連結される。
この機構を知らないと、運用時の環境で発生した問題を解決できないことがある。
Assets Pipelineはウェブアプリケーションに必須の機構ではなく最適化に含まれるが、Railsは明示的に外さない限り最適化を行う方針を採っている。

#### 4. コールバック

HTTPリクエストやDBアクセスを契機としたトランザクションの前後のコールバックには典型的な使い方がある。
例えば、認証にはトランザクションの前のコールバックを利用する。

その他のRails Wayは、[Rails Guides](http://guides.rubyonrails.org/)や[RailsによるアジャイルWebアプリケーション開発](http://www.amazon.co.jp/dp/4274068668)に記載されている。

結果
----

最小のコードで最大の生産性を得られる。
また、Rails Wayに沿ったコードは書いた本人以外の開発者にも読みやすく、保守性が高まる。