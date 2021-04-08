# subEx( )功能

## 客戶需求 : APConfig提供訂閱_指定Product_的功能

使用者必須提供 Product，不然會傳回錯誤訊息(**SendErrMsg**)，參考 Ex-cmd like setExProduct ,  etc ....。 除此之外的Company, User，由SessionObject so.getUniqueId( ) 取得。

``` markdown
__只需訂閱品 及 使用者頻道__， 公司頻道已在 sub() 訂閱過。
```

## 開發遇到的問題

### Cmd顯示錯誤

>**應當是 "subEx"，卻顯示 "sub"**
>訂閱功能本身正常，產品或是使用者的新增/修改，都可以收到訊息通知，但也就是這個訊息通知是有問題的。 問題出在回覆格式裏頭的 "c"(command)，**其顯示的值卻是"sub"。**

傳給客戶的訊息，是誰發送的 ? 

> Handler 裏頭的 sub or subEx 的 channelSubscribe的 CallBack's Fumction

### 取消訂閱

>若取消訂閱的Channel ，之前並沒有訂閱的話，會出現 40層以上的錯誤，嘗試catch 這個錯誤卻沒辦法處理。

### 執行問題

> 響應函數的執行順序，得看 訂閱函數的底層是怎麼寫的。
> 有可能是多執行緒或是單一執行緒，都有可能。

# unsubEx

## 開發遇到問題

### NullPointer

> 假設遇到沒訂閱過的頻道，則取消訂閱的回傳將會是 Null，
> 執行 Result res.isSuccessful( ) 就會出現 NullPointer的問題

[ArrayList && LinkedList](https://ithelp.ithome.com.tw/articles/10213112)

