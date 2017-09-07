---
layout: post
title:  "[自荐]-跟我一起学Spring Shell（学习札记）"
date:   2017-09-06 16:00:00 +0800
categories: springshell
comments: true
---
有机会在工作中使用Shell相关，参考了SpringShell，中途记录一则。   

#   内容更新进度
2017.09.06  基本描述，完成shell的基本使用；   
2017.09.07  补充Interacting with the Shell，内置命令系列；   
---

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
3.  2.x只需要引入spring-shell-starter即可；
4.  2.x支持tab获取命令，当让tab不仅限于命令，同样适用于参数；
5.  2.x减少了很多用处不大的内置命令，使2.x更轻盈，功能明确，结构清晰。

---

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
##     运行代码
![启动shell](../../../../sources/images/posts/shell1.png)  

![shell示例](../../../../sources/images/posts/shell3.png)  

![shell示例](../../../../sources/images/posts/shell2.png)  

##     问题
在使用中注意到@ShellOption中的optOut添加上后，代码执行就会自动结束，不会在控制台出现“shell>”符号。

---

##     Interacting with the Shell
Spring Shell project 基于 JLine library, 给我们带来了非常好的交互体验。     
1.  tab键, 本人觉得是2.x做的最优秀的改进，如前所述，不仅限于获取命令，同样适用于参数；
2.  ctrl+r，reverse search；
3.  ctrl+a, move to beginning of line respectively(光标定位到命令开始位置)；
4.  ctrl+e, move to end of line respectively(光标定位到命令结束位置)；    
5.  Esc f, move forward one word at a time(一次前进一个字)；
6.  Esc b, move backward one word at a time(一次回退移动一个字)；
7.  \, 使用\ Enter，开启新的一行继续输入；
8.  ", 使用"并开未结束，并Enter，会继续一条命令的输入，知道输入关闭的"；

---

##     内置命令
clear: Clear the shell screen.   
exit, quit: Exit the shell.   
help: Display help about available commands.   
script: Read and execute commands from a file.   
stacktrace: Display the full stacktrace of the last error.   

####    内置命令可以通过排除spring-shell-standard-commands使其不可用   
```xml
<dependency>
    <groupId>org.springframework.shell</groupId>
    <artifactId>{starter-artifactId}</artifactId>
    <version>{project-version}</version>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.shell</groupId>
            <artifactId>spring-shell-standard-commands</artifactId>
        </exclusion>
    </exclusion>
</dependency>
```
####    禁用指定的内置命令
通过设置spring.shell.command.<command>.enabled为false在应用环境。
```java
public static void main(String[] args) throws Exception {
    String[] disabledCommands = {"--spring.shell.command.help.enabled=false"}; 
    String[] fullArgs = StringUtils.concatenateStringArrays(args, disabledCommands);
    SpringApplication.run(MyApp.class, fullArgs);
}
```
####    override指定的内置命令
1.  像上面所说的禁用指定的内置命令；    
2.  自定义类实现<Command>.Command，例如重新定义clear命令：  
```java
public class MyClear implements Clear.Command {
    @ShellCommand("Clear the screen, only better.")
    public void clear() {
        // ...
    }
}
```

---

##     你可以参与的
如果您在阅读中发现文中描述有不正确的地方，欢迎留言指正，留言交流或者在[关于我](https://beanstt.github.io/about/index.html)加我微信。
##     我还可以做的
此文作为学习入门文档，如果后续有新的见解或更有帮助的内容，会持续更新。   

#      参考声明
[Spring Shell 2.0.0.BUILD-SNAPSHOT Reference](https://docs.spring.io/spring-shell/docs/2.0.0.BUILD-SNAPSHOT/reference/htmlsingle/)