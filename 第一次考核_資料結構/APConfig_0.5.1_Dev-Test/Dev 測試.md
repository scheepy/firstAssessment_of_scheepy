# Dev 測試

## subEx 

> 同個使用者的其他產品訂閱

```java
INFO [pool-1226-thread-1] [f10768294707] - [APC]Handle {"s":"APConfig","c":"subEx","d":{"product":"gg"},"r":"subEx功能"}, "r":subEx功能
INFO [pool-1226-thread-1] [f10768294707] - [APC]subEx's uid pdpf:ss2:ff:historydata
INFO [pool-1226-thread-1] [f10768294707] - [APC]subExOut {"p":{},"u":{"test subEx":1}}
INFO [pool-1226-thread-1] [f10768294707] - [APC]subEx (true, true)
```



```java
 [APC]Handle {"s":"APConfig","c":"subEx","d":{"product":"gg"},"r":"subEx功能"}, "r":subEx功能
 [APC]subEx's uid pdpf:ss2:ff:historydata
 [APC]subExOut {"p":{"subEx product":7},"u":{"subEx ur":10}}
 [APC]subEx (true, true)
 check permission : svc = APConfig, cmd = unsubEx, perm = true
[APC]Handle {"s":"APConfig","c":"unsubEx","d":{"product":"gg"},"r":"unsubEx 測試"}, "r":unsubEx 測試
[APC]subEx's uid pdpf:ss2:ff:historydata
[APC]Product Channel success ?true, User Channel success ?true)
```

