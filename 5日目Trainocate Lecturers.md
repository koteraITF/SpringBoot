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
# 先に教える@Contorollerの秘密まで
