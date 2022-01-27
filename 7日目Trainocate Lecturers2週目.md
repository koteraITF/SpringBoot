# SpringBoot

## DAOパターン
①Controllerが検索用の値や保存用のEntityをBL（BusinessLogic）に渡す。  
②BLは対応するDAOクラスを見つけて、そこにEntityを渡し依頼をかける。  
③DAOは（今回は）JDBC（RDBMSと接続するライブラリ）にアクセスして、データを取得。  
④DAOからBLへ結果のEntityを返し、BLはそのデータをContorollerに渡す。  

## DAOクラス
DAOクラスは二つのファイルから構成される。  
①〇〇〇Dao.java（インタフェース）  
②〇〇〇DaoImpl.java（クラス）  

@Repositoryでデータを扱うクラスであることを明確にする。
<img width="692" alt="image" src="https://user-images.githubusercontent.com/97214466/151267794-4330c160-70d7-4075-9f96-63f76ffcf278.png">

下記コマンドでは1行目でListの中にMapが格納されており、2行目ではオブジェクト指向を実現されるためにはMapをEntityクラス（Inquiryクラス）に置き換える必要がある。

```
  List<Map<String, Object>> resultList = jdbcTemplate.queryForList(sql);
  List<Inquiry> list = new ArrayList<Inquiry>();
```

getAllで全てのデータを取り出すことができる。
<img width="717" alt="image" src="https://user-images.githubusercontent.com/97214466/151272596-96d0e244-7460-4155-a85e-37f1fa6c1947.png">



