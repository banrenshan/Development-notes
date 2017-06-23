#1.spring @Conditional注解的使用
此注解使得只有在特定条件满足时才启用一些配置
````
public class MyCondition implements Condition  
{  
    /** 
     * 这里写自己的逻辑，只有返回true，才会启用配置 
     */  
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata)  
    {  
        return true;  
    }  
}  
````

````
@Configuration  
@Conditional(MyCondition.class)  
public class Config  
{  
    @Bean  
    public Serializable createSerializable()  
    {  
        System.out.println("======000");  
        return "";  
    }  
}  

````

除了自己自定义Condition之外，Spring还提供了很多Condition给我们用
@ConditionalOnBean（仅仅在当前上下文中存在某个对象时，才会实例化一个Bean）
@ConditionalOnClass（某个class位于类路径上，才会实例化一个Bean）
@ConditionalOnExpression（当表达式为true的时候，才会实例化一个Bean）
@ConditionalOnMissingBean（仅仅在当前上下文中不存在某个对象时，才会实例化一个Bean）
@ConditionalOnMissingClass（某个class类路径上不存在的时候，才会实例化一个Bean）
@ConditionalOnNotWebApplication（不是web应用）

#2.@Qualifier注解
@Autowired是根据类型进行自动装配的。如果当spring上下文中存在不止一个类型的bean时，就会抛出BeanCreationException异常;如果Spring上下文中不存在某类型的bean，也会抛出BeanCreationException异常。我们可以使用@Qualifier配合@Autowired来解决这些问题。如下：
````
@Autowired   
@Qualifier("userServiceImpl")   
public IUserService userService;
````
