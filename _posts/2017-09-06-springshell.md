---
layout: post
title:  "Spring Shell 学习札记"
date:   2017-09-06 16:00:00 +0800
categories: spring, shell
comments: true
---
有机会在工作中使用Shell相关，参考了SpringShell，中途记录一则。   
#   What is Spring Shell?
略。
#   Using Spring Shell
##     版本
目前主要有1.2.0.RELEASE、1.2.1.BUILD-SNAPSHOT、2.0.0.BUILD-SNAPSHOT
##     建议使用
2.0.0.BUILD-SNAPSHOT
##     2.x PK 1.x
1.  2.x更加规范，更加标准，对接口使用了standard API；
2.  2.x使用@ShellComponent、@ShellMethod、@ShellOption代替了1.x的@Component、@CliCommand、@CliOption；
3.  2.x只需要引入spring-shell-starter即可。

##     主要代码
实例是一个springboot项目:    

```java
import org.springframework.shell.standard.ShellComponent;
import org.springframework.shell.standard.ShellMethod;
import org.springframework.shell.standard.ShellOption;

/**
 * shell中的case命令入口
 *
 * Created by yangjt on 2017-09-04 13:39.
 */
@ShellComponent
public class CaseCommands
{
    @ShellMethod(key = "b-simple", value = "Print a simple hello world message")
    public String simple(
            @ShellOption(value = { "message" }, help = "The hello world message") final String message,
            @ShellOption(value = { "location" }, help = "Where you are saying hello", defaultValue="At work") final String location) 
    {
        return "Message = [" + message + "] Location = [" + location + "]";
    }
}
```


##     问题
在使用中注意到@ShellOption中的optOut添加上后，代码执行就会自动结束，不会在控制台出现“shell>”符号。
##     你可以参与的
如果您在阅读中发现文中描述有不正确的地方，欢迎留言指正，留言交流或者在[关于我](https://beanstt.github.io/about/index.html)加我微信。
##     我还可以做的
此文作为学习入门文档，如果后续有新的见解或更有帮助的内容，会持续更新。
