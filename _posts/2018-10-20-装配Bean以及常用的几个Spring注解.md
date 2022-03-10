# 装配Bean以及常用的几个Spring注解

---

* **@Component注解**   用于声明为一个组件类，让spring自动将该类创建为一个Bean,并且不用再显式配置该类为一个bean  【使用该注解必须在配置类上搭配@ComponentScan一起使用，需要扫描到组件，也可以使用XML启动组件扫描<context:component-scan base-package="xxx">】

* **@AutoWired注解**     当存在一个bean需要依赖另一个bean的时候，可以在方法上声明该注解

```java
/** 本段代码中CDPlayer bean需要
CompactDisc bean,所以需要添加autowired注解
**/
@Component
public class CDPlayer implementes MediaPlayer{
		priavte CompactDisc cd;
		
		@AutoWired
		public CDPlayer(CompactDisc cd){
			this.cd = cd;
		}
}
```

> 如上代码中，注解@Component声明在类上，当spring自动创建CDPlayer的Bean时候，需要依赖CompactDisk bean，当spring找到CompactDisk bean时，会自动装配到CDPlayer构造方法中。当存在多个满足CDPlayer的依赖bean时，Spring会抛出错误（NoUniqueBeanDefinitionException），表明没有指定依赖哪个Bean，简称自动装配的歧义性。**消除spring bean歧义性的方法，通过@Primary注解选择首选bean**。代码如下所示：

```java
// 使用component注解方式搭配Primary消除歧义性
@Component
@Primary
public class Cookie implements Dessert{....}

// 使用bean注解方式搭配Primary消除歧义性
@Bean
@Primary
public class Cookie implements Dessert{
		return new Cookie();
}
```

> 上方代码中存在一个问题，当首选的bean数量不止一个时，应该怎样操作呢？答：使用Spring提供的@Qualifier()注解，该注解可以绑定bean的id作为参数，但是缺点是与bean id（也就是类名）属于紧耦合，当重构了类名后，qualifier就会失效。代码如下所示：

```java
// component注解生成Cookie bean
@Component
public class Cookie implements Dessert{....}

// qualifier限定符中的小写开头cookie是spring为bean创建的默认id
@Autowired
@Qualifier("cookie")
public void setDessert(Dessert dessert){
		this.dessert = dessert;
}
```

> 上方代码也存在一个问题，就是qualifier注解与类名是紧耦合，不在选择类名作为限定符，而是选择这个类的特征作为限定符，比如曲奇的特征是“高热量”（high heat）,改进代码如下:

```java
// qualifier注解设置特征限定符
@Component
@Qualifier("high heat")
public class Cookie implements Dessert{....}

// qualifier限定符中的参数搭配cookie类的qualifier标注的特征，减轻耦合度
@Autowired
@Qualifier("high heat")
public void setDessert(Dessert dessert){
		this.dessert = dessert;
}
```

> 上方代码仍旧存在问题，如果是出现了另一个类，也是属于high heat的特征，如果想要使用qualifier标记这个类的另一个特征“辣”（hot），但java不允许出现两个相同的注解在一个类上，无法通过编译（如下代码片段2-1所示），spring又会对于bean的识别出现歧义性，解决方案是使用自定义的注解来标记bean的特征，代码片段（2-2）如下所示：

```java
// 代码片段2-1
@Component
@Qualifier("high heat")
@Qualifier("hard")
public class Cookie implements Dessert{....}

@Component
@Qualifier("high heat")
@Qulifier("hot")
public class IceCream implements Dessert{....}

@Autowired
@Qualifier("high heat")
@Qualifier("hard")
public void setDessert(Dessert dessert){
		this.dessert = dessert;
}

// 上面代码无法通过编译，java不允许出现2个相同类型注解在一个类或者方法上
```

```java
// 代码片段2-2 自定义注解解决qualifier歧义性

// hot的自定义注解
@Target({ElementType.CONSTRUCTOR,ElementTtpe.FIELD,ElementType.METHOD,ElementType.TYPE})
@Retention(RententionPolicy.RUNTIME)
@Qualifier
public @interface Hot{}

// hard的自定义注解
@Target({ElementType.CONSTRUCTOR,ElementTtpe.FIELD,ElementType.METHOD,ElementType.TYPE})
@Retention(RententionPolicy.RUNTIME)
@Qualifier
public @interface Hard{}

// high heat的自定义注解
@Target({ElementType.CONSTRUCTOR,ElementTtpe.FIELD,ElementType.METHOD,ElementType.TYPE})
@Retention(RententionPolicy.RUNTIME)
@Qualifier
public @interface Highheat{}


@Component
@Highheat
@Hard
public class Cookie implements Dessert{....}


@Component
@Highheat
@Hot
public class IceCream implements Dessert{....}


// 使用自定义的注解特征类型标记注入的bean,解决了歧义性问题，也减轻耦合度。
@Autowired
@Highheat
@Hard
public void setDessert(Dessert dessert){
		this.dessert = dessert;
}
```

* **@Bean注解**   该注解会告诉spring这个方法将会返回一个对象，该对象要注册为一个spring应用上下文的bean，该bean的id，与被bean注解的方法名一样。

```java
@Bean
public CompactDisc jayChouCD(){
		return new jayChouCD();
}

@Bean
public CDPlayer cdPlayer(CompactDisc cd){
		return new CDPlayer(cd);
}
```

> 如上第一段代码所示，Bean注解将告诉spring这个方法会返回一个jayChouCD对象，并且把该对象注册为一个spring应用上下文的bean，这个bean的id=jayChouCD。第二段代码则通过将cd做为参数，创建cdPlayer bean。

* **@Import和@ImportResource注解**      该注解用于引入存在不同位置的配置类，import注解引入java配置类，importResource引入xml文件类型的配置类

>@Component注解和@Bean注解区别：两者都是可以在声明的Spring的Bean对象，但是**Bean注解标记在方法上，且生成的bean是方法最后返回的那个对象，而Component注解是标记在类上的，生成的Bean是标记的类**

