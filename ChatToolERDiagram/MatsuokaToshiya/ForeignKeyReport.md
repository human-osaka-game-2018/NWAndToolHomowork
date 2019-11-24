﻿### 外部キー制約

まず、外部キー制約は、  
テーブルのあるカラムのデータを他のテーブルのデータに依存するようにカラムに設定する制約のこと。  

依存している側は、依存先に存在していない値を制約をつけているカラムに設定できなくなる。
この制約には依存先のデータが更新削除された時に依存しているデータをどのようにするかを設定できる。

* ON UPDATE  
親テーブルの外部キーに関係するカラムが更新された時,子テーブル 外部キーの挙動設定  
* ON DELETE  
親テーブルの外部キーに関係するカラムが削除された時,子テーブル 外部キーの挙動設定  

更新時と削除時には以下の4つの挙動を設定できる。

|値|意味|  
|:--|:--|  
|RESTRICT|親テーブルに変更を加えた時にエラーを返す|  
|NO ACTION|RESTRICTと一緒|  
|CASCADE|親テーブルの変更がそのまま子テーブルへも反映される|  
|SET NULL|親テーブルの更新・削除->子テーブルへ NULL が設定される|  


以上のことから今回の制作において外部キー制約は如何様にすべきか考えていく。

#### 制約をどうするか？

* **更新**  
まず、OnUpdateにはCASCADEを設定するのが良いだろう。  
そうしておくと、依存先データの書き換え時に依存しているカラムの値が更新されるので、依存先のデータの書き換えが容易になる。  

* **削除**  
OnDeleteには、RESTRICTを設定するのが良いと思う。
なぜなら、そのデータを削除し、仮にSET NULLを設定していた場合は依存している側のそのデータはNULLになってしまう。  
NULLになっているそのデータがもしNULLデータを許容できない扱い方をしていた場合に、DB側からエラーを吐くことになる。  
削除をするのであれば、その前に参照されていない状態にしてから削除をしたほうが、意図しない変更を防ぐことができるのではないかと考える。  
そのかわりに、データの削除をするときには一手間加える必要がある。(しかし、基本的に論理削除で進めるデータならば必要な手間であると思う)
