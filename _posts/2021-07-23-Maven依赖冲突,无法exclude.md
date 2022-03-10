# Maven依赖冲突,无法exclude

---

* 错误提示

```java
The class hierarchy was loaded from the following locations:
		feign.Request.Options: file:/Users/wangbingzhi/Documents/mavenrepository/com/wo/wo-sdk-14/1.0.28/wo-sdk-14-1.0.28.jar
Correct the classpath of your application so that it contains a single, compatible version of feign.Request$Options
```

* 解释：在使用springcloud.openfeign的时候和第三方sdk自带的feign产生冲突，一直提示使用的是第三方sdk中的feign，这是因为maven的优先加载原则导致，在同一个pom.xml中，springcloud.openfeign是位于第三方的sdk之后的位置，就会导致先加载第三方的sdk中的feign，在这个时候只需要将springcloud的feign依赖写在第三方sdk之前，即可解决该问题