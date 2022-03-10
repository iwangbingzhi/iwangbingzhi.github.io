# EffectiveJava读书笔记14实例变量不能由public修饰

---

```java
class Point{
    public double x;
    public double y;
}
```

> 如果按照上述代码，将x,y设置为public，如果是非final的，本类将会失去对于x,y的值的控制，将成员变量设置成私有，并且采用getter和setter方法设置x,y值

```java
class Point{
    private double x;
    private double y;

    public Point(double x,double y){
        this.x = x;
        this.y = y;
    }

    public double getX(){return x;}
    public double getY(){return y;}

    public void setX(double x){
        this.x = x;
    }

    public void setY(double y){
        this.y = y;
    }
}
```

