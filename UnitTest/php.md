[Prophecy](https://github.com/phpspec/prophecy)

ProphecyではPartial Mockをサポートしていない。
https://github.com/phpspec/prophecy/issues/61
https://github.com/phpspec/prophecy/issues/101

これは良い記事
[PHPUnitのモックオブジェクトの使い方を仕組みから理解する](https://taisablog.com/archives/55)

何度も読み返したい
[現在時刻が関わるユニットテストから、テスト容易性設計を学ぶ](https://t-wada.hatenablog.jp/entry/design-for-testability)

PHP標準で用意されているDateTimeだと関数内部でインスタンスが生成されるようなケースでテストがしづらい。
ので、Carbonを使うか、DateTimeを拡張した独自クラスを作るか、したほうが良さそう。
