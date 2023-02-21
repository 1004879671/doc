# 实例化bean方式

![](D:\mutianyu\doc\images\2022-10-13-00-12-37-image.png)

![](D:\mutianyu\doc\images\2022-10-13-00-16-18-image.png)

![](D:\mutianyu\doc\images\2022-10-13-00-14-46-image.png)

# bean的生命周期

![](D:\mutianyu\doc\images\2022-10-13-00-28-04-image.png)

![](D:\mutianyu\doc\images\2022-10-13-00-28-28-image.png)

![](D:\mutianyu\doc\images\2022-10-13-00-28-46-image.png)

![](D:\mutianyu\doc\images\2022-10-13-00-29-13-image.png)

# 依赖注入

## 1、setter注入

![](D:\mutianyu\doc\images\2022-10-13-00-33-31-image.png)

![](D:\mutianyu\doc\images\2022-10-13-00-37-07-image.png)

## 2、构造器注入

![](D:\mutianyu\doc\images\2022-10-13-00-40-45-image.png)

![](D:\mutianyu\doc\images\2022-10-13-00-41-34-image.png)

![](D:\mutianyu\doc\images\2022-10-13-00-44-10-image.png)

![](D:\mutianyu\doc\images\2022-10-13-00-44-29-image.png)

![](D:\mutianyu\doc\images\2022-10-13-00-47-08-image.png)

## 3、自动装配

![](D:\mutianyu\doc\images\2022-10-13-00-51-42-image.png)

![](D:\mutianyu\doc\images\2022-10-13-00-51-59-image.png)

![](D:\mutianyu\doc\images\2022-10-13-00-54-09-image.png)

![](D:\mutianyu\doc\images\2022-10-13-00-52-22-image.png)

![](D:\mutianyu\doc\images\2022-10-13-00-56-30-image.png)

## 4、集合注入

![](D:\mutianyu\doc\images\2022-10-13-00-59-20-image.png)

![](D:\mutianyu\doc\images\2022-10-13-01-01-56-image.png)

![](D:\mutianyu\doc\images\2022-10-13-01-00-03-image.png)

![](D:\mutianyu\doc\images\2022-10-13-01-00-44-image.png)

# 总结

![](D:\mutianyu\doc\images\2022-10-14-00-00-18-image.png)

# AOP

```java
@Component
@Aspect
public class ProjectAdvice {
    //配置业务层的所有方法
    @Pointcut("execution(* com.itheima.service.*Service.*(..))")
    private void servicePt(){}
    //@Around("ProjectAdvice.servicePt()") 可以简写为下面的方式
    @Around("servicePt()")
    public void runSpeed(ProceedingJoinPoint pjp){
        //获取执行签名信息
        Signature signature = pjp.getSignature();
        //通过签名获取执行操作名称(接口名)
        String className = signature.getDeclaringTypeName();
        //通过签名获取执行操作名称(方法名)
        String methodName = signature.getName();

        long start = System.currentTimeMillis();
        for (int i = 0; i < 10000; i++) {
           pjp.proceed();
        }
        long end = System.currentTimeMillis();
        System.out.println("万次执行："+ className+"."+methodName+"---->" +(end-start) + "ms");
    } 
}
```

# 事务

![](D:\mutianyu\doc\images\2022-10-15-17-11-01-image.png)
