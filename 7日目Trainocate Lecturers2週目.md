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

## 例外処理
Update機能を実装したが、反応がない場合には例外処理を用いる。  
queryForMapで１件も取得できないなどのエラーはデフォルトのエラー画面が表示されるので問題ないが、エラーが起こってもエラー画面が出力されない事態のために例外処理機能を実装する。

## DBのLISTの取得

下記コマンドでMapリストからデータ一覧を取得できる。
```
（TaskDaoImpl.java）
List<Map<String,Object>> resultList = jdbcTemplate.queryForList(sql);
```

下記コマンドでMapリストから個別にデータを取得できる。
```
（TaskDaoImpl.java）
List<Map<String,Object>> resultList = jdbcTemplate.queryForMap(sql,id);
```

<img width="564" alt="image" src="https://user-images.githubusercontent.com/97214466/151317119-603186bb-ff71-4a75-8265-71e26dc64dbb.png">

## Optionalについて

下記コマンドはOptionalに関するコマンドである。普通はstr=nullであるため、これをsysoutするとエラーが発生するが、Optinaolを用いることで、エラーを回避することができる。  
```
  String　str = null;
  Optional<String> strOpt = Optional.ofNullable(str);
  if(strOpt.isPresent()) {
    String message = strOpt.get();
    System.out.println(message.length());
  }
```

## Schemeについて
下記コマンドはデータに関するものであるが、taskのtype_idはtask_typeテーブルとtaskテーブルをJOINさせるために追加された。
```
CREATE TABLE task_type (
  id int(2) NOT NULL,
  type varchar(20) NOT NULL,　　　　
  comment varchar(50) DEFAULT NULL,
  PRIMARY KEY (id)
);

CREATE TABLE task (
  id int(5) NOT NULL AUTO_INCREMENT,
  user_id int(5) NOT NULL,
  type_id int(2) NOT NULL,   ///////JOIN用に追加
  title varchar(50) NOT NULL,
  detail text,
  deadline datetime NOT NULL,
  PRIMARY KEY (id)
) ;
```

そのため、下図のDaoファイルでは結合されている。
<img width="403" alt="image" src="https://user-images.githubusercontent.com/97214466/151320434-608a2471-d743-4495-b4f6-be7c7df030fb.png">

下記コマンドはデータベースのものであるが、下のOptionalでラッピングすることによって、データにNullがある可能性があることを示している。
```
Map<String, Object> result = jdbcTemplate.queryForMap(sql,id);

Task task = new Task();
task.setId((int)result.get("id"));
task.setUserId((int)result.get("user_id"));
task.setTypeId((int)result.get("type_id"));
task.setTitle((String)result.get("title"));
task.setDetail((String)result.get("detail"));
task.setDeadline(((Timestamp) result.get("deadline")).toLocalDateTime());

//taskをOptionalでラップする
Optional<Task> taskOpt = Optional.ofNullable(task); //////ここ！

```

## バリデーションについて
データベースの例外処理において、アノテーション（@Nullなど）が対応していないものはServiceImpl.javaで個別にバリデーション設定を作る必要がある。  
なんてめんどうなんだ...
ex)
```
try {
  return dao.findById(id);
} catch (EmptyResultDataAccessException e) {
  throw new TaskNotFoundException("指定されたタスクが存在しません");
}
```

## CRUD処理 Controller

```
/task GET　            //新規登録フォームとタスク一覧を表示
/task/insert POST      //タスクの新規登録
/task/{id} GET        //タスクの個々の変種画面の表示
/task/update POST     //編集画面から一件のタスクを更新
/task/delete POST     //タスク一件を削除
```

@Digits（integer = 1, fraction=0） :桁１、少数０の意味、つまり１桁のみということ。  

クラス内で重複したり、煩雑になりそうな処理はPrivateメソッドにして、コントローラーの読みやすさを上げたり、メンテナンスしやすくする必要がある。  






