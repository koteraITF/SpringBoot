# SpringBoot

謎のエラーが出現。daoフォルダ配下のものをすべてコメントアウトすれば解決したが、次にlocalhostを立ち上げると、すべて４０４ページになっていた。
(おそらく消してはいけないコマンドやファイルを消したため。)
２週目に解決する予定。
<img width="822" alt="image" src="https://user-images.githubusercontent.com/97214466/150457642-6ae90278-f93e-4087-9b64-472cecc98ab5.png">

## DIコンテナについて
DIコンテナに同名のクラスが定義されている場合エラーとなる。

## 例外処理
404（not found）や500(intenal server error)に対する画面表示.
上記の謎のエラーのため、ハンズオンは２週目に回す予定。

①コントローラーのメソッド内で個別にtry-catch文を記述  
②一つのコントローラ全体の例外を処理するメソッドを作成  
③すべてのコントローラーの例外を処理するContorollerAdviceクラスを作成  

<img width="584" alt="image" src="https://user-images.githubusercontent.com/97214466/150464703-1c3760aa-01e9-4252-b46d-3ca00406989a.png">

<img width="500" alt="image" src="https://user-images.githubusercontent.com/97214466/150464883-588cc3c7-5e95-4c98-8a38-172259c73c5c.png">


