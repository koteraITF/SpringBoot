# SpringBoot

## pojoクラス
ごく普通のJavaオブジェクト

## データ
一般的にデータはmap型で入っているので、Map型のデータをインスタンスに詰め替える必要がある。

## インタフェース
機能の概要
クラスを自動生成することもある（MyBatis）
## ショートカット

```
Shift+ctrl+O でimportのショートカット
```

```
Sysoutと入力してからctrl + Spaceキーで
System.out.printlnが出力される
```

## シングルトン
クラスのインスタンスが必ず１つであることを保証するデザインパターン
下図のようにコンストラクタ以降はインスタンスをnewすることはできない。  
<img width="500" alt="image" src="https://user-images.githubusercontent.com/97214466/150093696-710d3e70-4085-4ef8-9196-1584d8520e94.png">
