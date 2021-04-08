### APConfig

APConfig這個服務的功能是 ?

>首先，APConfig 是已完成註冊的使用者，可以使用APConfig這個服務做**儲存資訊**，資訊有特定的格式，cpup。
>
>cpup分別代表Company, Product, User, Profile。
>使用者分成的指令(cmd)分成兩類，gne-cmd, Ex-cmd，分別對應高低權限。
>兩者的差異，在於資訊格式cpup的自由度。gne-cmd 只可以寫 profile，ex-cmd 可以寫 pup。**Company一律由登入訊息載入(SessionObject so.getUniqueId( ) )**



假設今天有人要我在 APConfig裏頭加上什麼功能，會在哪裡加?

> 如果是添加功能的話，會加在 APConfigJobHandler裡面，所有客戶端的功能都集中在那裏。

### ISyncPersistent

> 操作符號
>
> 範例一
> Document filter = new Document();
> filter.append("product", new Document("$regex", product.toLowerCase)); // where product like %product.toLowerCase%
> or
> ArrayList<Document> al = new ArrayList<>();
> al.add(new Document("v", old_version);
> al.add(new Document("v", new Document("$exists", false)));
> filter.put("$or", al); // where v = old_version or v 這個欄位不存在(意思是根本沒有在 where 裏頭寫出 v 的篩選條件)

### 登入訊息

SessionObject so.getUniqueId( )

> pdpf:ss2:ffm:historydata