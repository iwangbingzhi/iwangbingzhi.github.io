# EffectiveJava笔记之6消除过期对象引用

---

比如在出栈的过程中，已经出栈的元素不会被jvm当做垃圾回收，即使调用栈的程序不再使用这些弹出的栈对象，也不会回收，因为存在对象的过期引用，过期引用指永远不会被解除的引用，所以在这个时候为了避免内存溢出，需要我们自己清空这些对象，错误代码如下代码片段所述：

```java
// 本段代码并没有处理过期引用（有时也被称为游离对象），严重时会爆发OOM

public Object pop(){
		if(size==0){
				throw new EmptyStackException();
		}
		return elements[--size];
}
```

```java
// 正确的处理过期引用
public Object pop(){
		if(size==0){
				throw new EmptyStackException();
		}
		Object result = elements[--size];
		elements[size] = null;  // 过期引用处理
		return result;
}
```



