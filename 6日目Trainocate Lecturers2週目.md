# SpringBoot

## Gradle(グレードル)とは

![image](https://user-images.githubusercontent.com/97214466/150923148-d8f299cf-5d5c-4b9c-9f6e-8e516256fa24.png)

## 空文字のnull変換

覚えるor理解する必要はないが、下記コマンドで空文字をnullに変換でき、バリデーションの時に役立てる。
@ContollerAdviceとはContorollerというクラスを横断した処理を作成できる。（共通処理の定義などを設定しておくときに便利）
```
@ControllerAdvice
public class WebMvcControllerAdvice {

	/*
	 * This method changes empty character to null
	 */
    @InitBinder
    public void initBinder(WebDataBinder dataBinder) {
        dataBinder.registerCustomEditor(String.class, new StringTrimmerEditor(true));
    }
    
```
# Code

```
@Contoroller
@RequestMapping("/sample")   //URLのsample配下で行う
public class SampleController{

	@GetMapping("/test")　　　//URLのtestでGETアクションを行う
	public String index(Model model){　 //Modelクラス：ModelクラスとはWebページで使用するデータを管理するクラスである。
		model.addAttribute("title","Inquiry Form");　
		//addAttributeは第一引数でViewに渡す名前を、第二引数で値を設定する。今回はtitleというキーを元にViewにInquiryFormを埋め込む。
		
		return test;　//test.htmlが呼び出される。
	}
}
```

下記コマンドでは、map.get(カラム名）でテーブルの値を取得している。HTMLデータに埋め込むためにmodelを渡している。
```
model.addAttribute("email",map.get("name"));
model.addAttribute("email",map.get("email"));
```

下記コマンドのように、application.ymlでh2.console.enableをtrueにすることで、ブラウザからh2データベースのデータを確認できるようになる。
```application.yml
  h2.console.enabled: true
```

下記コマンドのように　sqlで取り出すDBを設定し、そのsqlをMapで配列に変換し、addAttributeでViewファイルへDBを出力することができる。
```
String sql = "SELECT id, name, email "
		+ "FROM inquiry WHERE id = 1";
Map<String, Object> map = jdbcTemplate.queryForMap(sql);

model.addAttribute("title", "Inquiry Form");
model.addAttribute("name",map.get("name"));
model.addAttribute("email",map.get("email"));
model.addAttribute("id",map.get("id"));
```

## Form
Formからの情報をまとめるクラス。一般的にフォームに関わることはフォームクラスにまとめる。  
<img width="400" alt="image" src="https://user-images.githubusercontent.com/97214466/151087253-a14c717f-35c1-4a00-b87d-8991ec44e542.png">  

下記コマンドのreturnはinquiryフォルダーのformファイルをreturnする。  
```
@GetMapping("/form")
public String form(Model model) {
	model.addAttribute("title","Inquiry Form");
	return "inquiry/form";　//inquiryフォルダのformファイル
}
```
下図のように、inputタグにname属性を付与することで、バリデーションを設定できる。  
<img width="700" alt="image" src="https://user-images.githubusercontent.com/97214466/151087876-637a31bd-deba-4c1d-87e5-5c7ee160dfea.png">  

下記コマンドで、BindingResult resultを記入することで、POSTコマンドを行い、バリデーションに引っかかった時に、BindingResultがTrueとなりバリデーションメッセージが表示される。  
```
@PostMapping("/confirm")
public String confirm(@Validated InquiryForm inquiryForm,
		BindingResult result,
		Model model) {
	model.addAttribute("title","Confirm Page");
	return "inquiry/confirm";
}
```
下図の赤線のコマンドでエラーをチェックする。
![image](https://user-images.githubusercontent.com/97214466/151094237-c541a53e-e691-425b-8b42-89c591714617.png)

下記コマンドのth:valueで戻るボタンを押しても、値が保存される状態にする。
```
<input id="name" name="name" type="text" th:value="*{name}"><br>
```

## フラッシュスコープ
リダイレクト後に１度だけ値を保持して表示できる。  
フラッシュスコープを実現するためには、下記コマンドが必要

```
RedirectAttributes redirectAttributes
```
addFlashAttribute()はリクエストを隔ててデータを保管する仕組みであるセッションという機能を利用している。

フラッシュスコープを受け取るためには、formのGetMappingで@ModelAttributeと記述する必要がある。
<img width="350" alt="image" src="https://user-images.githubusercontent.com/97214466/151103716-8a7125b5-6a4e-4c8f-915a-bd94efacf6e7.png">

## ステートレス
システムが現在の情報を表すデータを保持しないことであり、基本的にはアクセスごとにデータは失われる。そのため、次のページへデータを渡すのにはhiddenタグを用いる。
