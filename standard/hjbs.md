## **准备工作**

~~~
JDK >= 1.8 (推荐1.8版本)
Mysql >= 5.5.0 (推荐5.7版本)
Maven >= 3.0
~~~


## **运行系统**

1、前往Gitee下载页面([https://gitee.com/y_project/RuoYi](https://gitee.com/y_project/RuoYi))下载解压到工作目录  
2、导入到Eclipse，菜单 File -> Import，然后选择 Maven -> Existing Maven Projects，点击 Next> 按钮，选择工作目录，然后点击 Finish 按钮，即可成功导入  
Eclipse会自动加载Maven依赖包，初次加载会比较慢（根据自身网络情况而定）  
3、创建数据库ry并导入数据脚本ry_20190215.sql，quartz.sql  
4、打开运行`com.ruoyi.RuoYiApplication.java`  
5、打开浏览器，输入：http://localhost:80 （默认账户 `admin/admin123`）  
若能正确展示登录页面，并能成功登录，菜单及页面展示正常，则表明环境搭建成功  

建议使用Git克隆，因为克隆的方式可以和RuoYi随时保持更新同步。使用Git命令克隆  
git clone https://gitee.com/y_project/RuoYi.git


## **必要配置**

1. 修改数据库连接  
   `编辑resources目录下的application-druid.yml`  
   `url: 服务器地址`  
   `username: 账号`  
   `password: 密码`
   
2. 开发环境配置  
   `编辑resources目录下的application.yml`  
   `port: 端口`  
   `context-path: 部署路径`


## **部署系统**
`bin/package.bat` 在项目的目录下执行  
然后会在项目下生成 `target`文件夹包含 `war` 或`jar` （多模块生成在ruoyi-admin）
  
1、jar部署方式  
   使用命令行执行：`java –jar RuoYi.jar` 或者执行脚本：`bin/run.bat`  

2、war部署方式  
   pom.xml packaging修改为`war` 放入tomcat服务器webapps

> SpringBoot去除内嵌tomcat  

```xml
<!-- 多模块排除内置tomcat -->
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
	<exclusions>
		<exclusion>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-tomcat</artifactId>
		</exclusion>
	</exclusions>
</dependency>
		
<!-- 单应用排除内置tomcat -->		
<exclusions>
	<exclusion>
		<artifactId>spring-boot-starter-tomcat</artifactId>
		<groupId>org.springframework.boot</groupId>
	</exclusion>
</exclusions>
```


## **常见问题**
1. 如果使用`Mac` 需要修改`application.yml`文件路径`profile`
2. 如果使用`Linux` 提示表不存在，设置大小写敏感配置在/etc/my.cnf 添加lower_case_table_names=1，重启MYSQL服务
3. 如果提示当前权限不足，无法写入文件请检查`profile`是否可读可写，或者无法访问此目录

如遇到问题到[Issues](https://gitee.com/y_project/RuoYi/issues)反馈