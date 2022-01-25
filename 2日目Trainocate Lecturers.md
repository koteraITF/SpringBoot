# SpringBoot

## 設定
@Configurationとかくと、そのファイルが設定ファイルであることを明示する。
@Controllerとかくと、必要な時に自動的にインスタンス化してくれる。

## MVC
Model：SpringBootではModelをリクエストスコープという。ViewがHTMLデータを作るときに必要なデータを与える。  
Contoroller : 〇〇Mapping（"/ルーティングX"）という風にかくと、/ルーティングXに移動した際にアクションが起こる。
InquiryForm: バリデーションを扱うクラス @Validatedをおくと、バリデーションに反応し、BindingResultにtrueまたはfalseが返される。
RedirectAttributes: リダイレクト後に一度だけ入力値を保持して表示する。

下図のように、controllerのtitleとViewのtitleが一致すると、そのアクションを起こす。
<img width="500" alt="image" src="https://user-images.githubusercontent.com/97214466/150245164-aa713e97-f371-4133-bdc4-d6c75726e0f5.png">

下記のようにdivタグ内に th:object="${inquiryForm}"とかかれており、このdivタグ内ではinquiryFormのルールが適用される。
```
<div th:object="${inquiryForm}">
<p th:text="*{name}"></p>
<p th:text="*{email}"></p>
<p th:text="*{contents}"></p>
<form method="post" action="#" th:action="@{/inquiry/form}">
	<input type="hidden" name="name" th:value="*{name}">
	<input type="hidden" name="email" th:value="*{email}">
	<input type="hidden" name="contents" th:value="*{contents}">
	<input type="submit" value="戻る">
</form>
<form method="post" action="#" th:action="@{/inquiry/complete}">
	<input type="hidden" name="name" th:value="*{name}">
	<input type="hidden" name="email" th:value="*{email}">
	<input type="hidden" name="contents" th:value="*{contents}">
	<input type="submit" value="Complete">
</form>
</div>
```
下記はバリデーションに関する記述である。もし、バリデーションにひっかかれば、  
hasErrorsにひっかかり、inqury/formにreturnされる。  
ひっかからなければ無事に inqury/confirmにreturnされる。
```
	@PostMapping("/confirm")
	public String confirm(@Validated InquiryForm inquiryForm,
			BindingResult result,
			Model model) {
		if(result.hasErrors()) {
			model.addAttribute("title", "inquiry Form");
			return "inquiry/form";
		}
		model.addAttribute("title", "Confirm Page");
		return "inquiry/confirm";
```
form.htmlにおいて、#fieldsは暗黙オブジェクトといい、エラーがあった場合にはコントローラーのhasErrorsでbooleanを返す。  
trueの場合は、 th:errors="*{name}"でエラーメッセージを表示する。

```
	<div th:if="${#fields.hasErrors('name')}" th:errors="*{name}"></div>
```




## フラッシュスコープ
リダイレクト後に１度だけ値を保持して表示させる。

<img width="500" alt="image" src="https://user-images.githubusercontent.com/97214466/150290689-47eddd5d-18bd-4f87-816f-d2d4edbde930.png">

## H2データベース
<img width="500" alt="image" src="https://user-images.githubusercontent.com/97214466/150290998-6ec179a0-a5de-434d-a6d6-9bdf115f34d5.png">

### O/Rマッピング
EntityはすべてPrivateで変数を登録する。

### DAOパターン
DAO(Data Access Object)パターンとはデータベースへのアクセスを行うクラスを作り、そのクラスを通してデータベースへアクセスするデザインパターンです。 メインロジックのなかに記述されていたアクセス部分の処理を1つのクラスに集約し、データベースアクセスの窓口の役割を持たせます。  
<img width="600" alt="image" src="https://user-images.githubusercontent.com/97214466/150299086-4419d25b-711f-472c-8372-a66b29395008.png">

DAOパターンのメリット  
<img width="609" alt="image" src="https://user-images.githubusercontent.com/97214466/150299464-09d72ec8-8946-4a77-839b-17352a40bcbf.png">


まずcontrollerが検索用の値や保存用のEntityをBL（BusinessLogic）に渡し、BLは検索用の値や保存用のEntityをDAOに渡す。  
DAOはEntitityを受け取ってデータベースに値を保存したり、SQLを発行したりする。
<img width="600" alt="image" src="https://user-images.githubusercontent.com/97214466/150293138-2d0244de-62ce-426e-b856-78e7f6d7f77a.png">

### キャスト
型を変更することをキャストという。


