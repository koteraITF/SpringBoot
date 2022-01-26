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
