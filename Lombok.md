Lombok项目是一个java库，它可以自动插入到编辑器和构建工具中，增强java的性能。不需要再写getter、setter或equals方法，只要有一个注解，就有一个功能齐全的构建器、自动记录变量等等


## Lombok常用注解
### Data

整合了Getter、Setter、ToString、EqualsAndHashCode、RequiredArgsConstructor注解。

### Getter

快速构建Getter方法。

### Setter

快速构建Setter方法。

### ToString

快速将当前对象转换成[字符串](https://baike.baidu.com/item/%E5%AD%97%E7%AC%A6%E4%B8%B2/1017763?fromModule=lemma_inlink)类型，便于[log](https://baike.baidu.com/item/log/39110?fromModule=lemma_inlink)。

### EqualsAndHashCode

快速进行相等判断。
```java
import lombok.EqualsAndHashCode;  
  
@EqualsAndHashCode  
public class EqualsAndHashCodeExample {  
    private transient int transientVar = 10;  
    private String name;  
    private double score;  
    @EqualsAndHashCode.Exclude private Shape shape = new Square(5, 10);  
    private String[] tags;  
    @EqualsAndHashCode.Exclude private int id;  
  
    public String getName() {  
        return this.name;  
    }  
  
    @EqualsAndHashCode(callSuper=true)  
    public static class Square extends Shape {  
        private final int width, height;  
  
        public Square(int width, int height) {  
            this.width = width;  
            this.height = height;  
        }  
    }  
}
```

### NonNull

判断变量（对象）是否为空。
官方示例
```java
import lombok.NonNull;

public class NonNullExample extends Something {
  private String name;
  
  public NonNullExample(@NonNull Person person) {
    super("Hello");
    this.name = person.getName();
  }
}
```






















老板，我想和你谈一下工资假期的问题。

第1点：有关于我的能力。
1. 我一直想等到证明自己的能力之后再说这个问题，现在微信小程序的后台需求已经做得差不多了，你可以看一下。
另外我给你看一下我谈的之前的工资图片。


第2点：谈到薪资问题。
首先我是被招聘简章上的工资问题吸引过来的，上面写的是13-17K x 14，
即便是最底薪也可以了。现在这个工资落差很大。

但是我们一周6天班，没有节假日（虽然技术部门没有说一定上班，但是不来扣工资）。


就是我作为男生，有车贷房贷，有挣钱娶媳妇的压力。
	不能和刚毕业的