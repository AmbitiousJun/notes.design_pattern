# 1 概述
将一个复杂对象的构建与表示分离，使得同样的构建过程可以创建不同的表示。

# 2 角色
- 抽象建造者类（Builder）：这个接口规定要实现复杂对象的那些部分的创建，不涉及具体的对象部件的创建
- 具体建造者类（ConcreteBuilder）：实现Builder接口，完成复杂产品的各个部件的具体创建方法。在构造方法完成后，提供产品的实例
- 产品类（Product）：要创建的复杂对象
- 指挥者类（Director）：调用具体的创造者来创建复杂对象的各个部分，在指导者中不涉及具体产品的信息，只负责保证对象各部分完整创建或按某种顺序创建

# 3 案例：生产自行车

```java
public class Bike {

    private String frame;
    private String seat;
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getFrame() {
        return frame;
    }

    public void setFrame(String frame) {
        this.frame = frame;
    }

    public String getSeat() {
        return seat;
    }

    public void setSeat(String seat) {
        this.seat = seat;
    }

    @Override
    public String toString() {
        return "这是一辆使用了" + this.frame + "和" + this.seat + "的" + this.name;
    }
}
```

```java
public abstract class BikeBuilder {

    protected final Bike bike = new Bike();

    /**
     * 建造车架
     */
    public abstract BikeBuilder buildFrame();

    /**
     * 建造车座
     */
    public abstract BikeBuilder buildSeat();

    /**
     * 构建单车
     * @return 单车对象
     */
    public abstract Bike build();
}
```

```java
public class HelloBikeBuilder extends BikeBuilder{

    @Override
    public BikeBuilder buildFrame() {
        bike.setFrame("普通车架");
        return this;
    }

    @Override
    public BikeBuilder buildSeat() {
        bike.setSeat("橡胶车座");
        return this;
    }

    @Override
    public Bike build() {
        bike.setName("哈啰单车");
        return bike;
    }
}
```

```java
public class MobileBikeBuilder extends BikeBuilder{

    @Override
    public BikeBuilder buildFrame() {
        bike.setFrame("碳纤维车架");
        return this;
    }

    @Override
    public BikeBuilder buildSeat() {
        bike.setSeat("真皮车座");
        return this;
    }

    @Override
    public Bike build() {
        bike.setName("摩拜单车");
        return bike;
    }
}
```

```java
public class BikeDirector {

    private final BikeBuilder builder;

    public BikeDirector(BikeBuilder builder) {
        this.builder = builder;
    }

    public Bike construct() {
        return builder.buildFrame()
                .buildSeat()
                .build();
    }
}
```

在客户端调用方中无须关注单车的生产细节

```java
public class Client {

    public static void main(String[] args) {
        BikeDirector director1 = new BikeDirector(new MobileBikeBuilder());
        Bike bike1 = director1.construct();
        BikeDirector director2 = new BikeDirector(new HelloBikeBuilder());
        Bike bike2 = director2.construct();
        System.out.println(bike1);
        System.out.println(bike2);
    }
}
```

![](./images/建造者模式-案例运行结果.png)

