# DDD

## Domain Model

複雑な業務ドメインの中からシステムに必要な概念を適切に抽出する方法。

## Domain

広義では、チームが取り組んでいる事業全体

## Entity

一意なものを表現する概念。ライフサイクルをもち変化する。

### Domain Model貧血症

DB項目のプロパティしか存在しない機能不足なモデル。

## Value Object

変更を管理する必要がないもの。 (色、電話番号、郵便番号、氏名、誕生日...)

ドメインの何かを計測、定量化、説明した結果。

不変。

等価性の判定は各属性が持つすべての値が同じか。


## Transaction Script

呼び出し/リクエストごとに処理を記述する。DDDでは、業務をobjectの振る舞いとして表現する。
