![[Pasted image 20250125131442.png]]
# spring 框架中的单例bean是线程安全的吗？
>[!info]-
>不是线程安全的.
>一般spring的bean都是注入的无状态的对象，或者方法中的局部变量，都没有线程安全问题。如果bean中定义了可修改的成员变量，需要考虑线程安全问题，可以使用多例或者加锁来解决。

# 什么是AOP？项目中有没有使用到AOP？
>[!info]-
>AOP是一种面向切面编程的思想，它允许开发者在不修改业务代码的前提前，对原有代码添加额外的，公共的逻辑。比如日志记录，事务管理。
>直白一点说，它是在程序运行的过程中，将额外的功能逻辑动态的插入到业务逻辑中。


# Spring中事务失效的场景有哪些？

![[Pasted image 20250125140833.png]]
![[Pasted image 20250125140656.png]]
![[Pasted image 20250125140813.png]]

# Spring中bean的生命周期


# Spring中的循环引用


# Springboot中的自动配置原理


# Spring框架中常见的注解
## Spring中的常用注解
## SpringMvc中常用注解
## springboot中常见注解


## Mybatis的执行流程



