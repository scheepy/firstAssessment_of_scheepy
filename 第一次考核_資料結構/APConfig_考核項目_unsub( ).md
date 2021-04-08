# unsub( )

sub( )主要功能。
訂閱主要根據使用者的SessionObject so. getUniqueId( )來進行**分組訂閱**。

若要新增功能於APConfigJobHandler，除了另新增一個方法並將指令加進去handle()入口之外，~~還需要在APConfig 的 ACCmd的預設權限加上 unsub指令。~~ 這裡有個問題，如果服務上面沒有設定檔，Code就不會取不到設定檔，進而會用程式生成一個，這時候才會用到預設權限。 所以我剛剛就是沒有發現這點，所以APConfig就用網路上的設定檔傳送訊息給AC，所以才沒有sub。現在我已經把服務上面的設定檔做新增了。之後 AC 在 group裡面詢問APConfig有哪些**命令權限**，才能得到正確回應。 另一方面才能請管理者幫我在AC 裡面新增 unsub( ) 功能。

以下為**確認 unsub 指令有發送給 AC**

```java
021-03-30 12:00:01.737 [pool-10-thread-1] INFO  APConfig - ACCmd recv serviceID=dd1057ebeed8-DC7-1-ac channel=ac-query-perms doc={"s":"ac","c":"qry-perms","ts":1617076800001,"d":null}
//下面perms裏頭有 "unsub" 指令
2021-03-30 12:00:01.743 [pool-10-thread-1] INFO  APConfig - ACCmd send svc=ac ch=ac-ch perm={"s":"ac","c":"svc.perms.upd","r":"apconfig","d":{"APConfig":{"name":"APConfig","perms":["unsub","sub","search","getCompany","setCompany","delCompany","getProduct","setProduct","delProduct","getUser","setUser","delUser","getExCompany","setExCompany","delExCompany","getExProduct","setExProduct","delExProduct","getExUser","setExUser","delExUser"]}},"ts":1617076801738}
```



以下為加入unsub() 功能的分解步驟:

- [x] **確認**訂閱後，有人新增/修改資料的話，會回傳訊息給客戶端。
- [x] 取消訂閱寫進sub() 功能正不正常
- [x] APConfigJobHandler's handle裡頭加 unsub() 的指令，把取消訂閱功能獨立出來



---

- BOCmd's delDoc's sendClient(參數) 無法正常執行

  > **因為**sendClient(參數) 要能寄送訊息，**需要知道同時滿足三個條件**，才會季送訊息給客戶。
  >
  > 1. Company != null
  > 2. Result 有 version 
  > 3. Result's version 比 old_version還要大
  >
  > delDoc 後面的 ConfigDao的 delDoc ，必沒有把 version 的訊息加進去 Result裡面，所以sendClient 不會季送訊息給客戶。



channelUnsubscribe( ) 需要哪一些參數 ? 

> 這部分我完全參照 doSubAction 
>
> ```java
> private void doSubAction(SessionObject so, JSONObject jso, String cmd, String r) throws Exception
> ```
>
> 

客戶在這一次連線開啟訂閱功能，下次登入還需要開啟登入功能嗎?
這次登入開啟了訂閱功能，將Eclipse乾淨斷開之後，再次登入發現
訂閱功能失效了。 這是不是為什麼沒有，unsub( )功能的原因，只需要重登，訂閱功能就會消失。

> **訂閱功能相依於連線Session**，意思是每次連線之後的訂閱都是獨立的。最直接的證明是`pMgr.channelSubscribeString("APConfig",SessionObject so)`，看的出來，頻道訂閱所需參數是SessionObejct ，也就是相依於連線。

```java
private void unsub(SessionObject so, JSONObject jso, String cmd, String r) throws Exception {
		String uid = so.getUniqLoginId();
		logit("unsub's uid" + uid);
		String[] uids = uid.split(":");

		//告知使用者他取消了哪些頻道
//		Document outDoc = new Document();
//		outDoc.append("取消訂閱哪一些頻道", "===========>");
//		outDoc.append("Company Channel", uids[1]);
//		outDoc.append("Product Channel", uids[1] + ":" + uids[2]);
//		outDoc.append("User Channel", uids[1] + ":" + uids[2] + ":" + uids[3]);
//		so.sendJson(this, cmd, outDoc, 0, r);

		Result res1 = pMgr.channelUnsubscribe("APConfig", so, uids[1]);
		Result res2 = pMgr.channelUnsubscribe("APConfig", so, uids[1] + ":" + uids[2]);
		Result res3 = pMgr.channelUnsubscribe("APConfig", so, uids[1] + ":" + uids[2] + ":" + uids[3]);
		logit("unsub (Company Channel success ?" + res1.isSuccessful() + ", Product Channel success ?" + res2.isSuccessful() + ", User Channel success ?" + res3.isSuccessful() + ")");

	}

```

## 檢討點

1. Eclipse 的 tab 按鍵改成空白組成

   1. 由 Preference 進去 改 General > Editors > Text Editors > Insert spaces for tabs 勾選
   2. 同樣 Preference， Java > Code Style > Formatter Add new Profile 
      Indentation Tab policy > Spaces only 套用

2. SourceTree 需要  Atassian Bitbucket 帳號。

   新增帳號參數設定
   Host
   	Hosting Service : Bitbucket
   	Preferred Protocol : HTTPS
   Credentials
   	Authentication: OAuth
   	Username: 按下之下的 Refresh OAuth Token 自動帶入

3. 調整 SourceTree's  Diff 工具，選成WinMerge

[SourceTree 參考](https://ithelp.ithome.com.tw/articles/10206852)