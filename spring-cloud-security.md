#说明
下面将由几个例子来说明spring-cloud-security的使用.例子的顺序由浅入深.

#快速入门
##1.基本的认证
1.引入依赖
````
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-oauth2</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-security</artifactId>
        </dependency>

````

2.基本代码
````
@RestController
public class HelloController {


    @RequestMapping("/hello")
    public String hello() {
        return "hell0";
    }
}
````
3.测试访问/hello,这时浏览器会弹出basic 认证,用户名是user,密码在项目启动的控制台上面可以看到.

>注意:我们也可以自定义密码,只需要在配置文件中添加 *security.user.password=1234*
##2.自定义登录界面
