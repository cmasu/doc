## **简介**

RuoYi是一款基于SpringBoot+Bootstrap的极速后台开发框架。

* RuoYi 官网地址：http://ruoyi.vip
* RuoYi 在线文档：http://doc.ruoyi.vip
* RuoYi 源码下载：https://gitee.com/y_project/RuoYi
* RuoYi 在线提问：https://gitee.com/y_project/RuoYi/issues
* RuoYi 博客：https://www.oschina.net/p/ruoyi
* QQ 群号： 1389287、1679294、1529866、1772718、1366522、1382251

RuoYi 是一个 Java EE 企业级快速开发平台，基于经典技术组合（Spring Boot、Apache Shiro、MyBatis、Thymeleaf、Bootstrap、Hplus），内置模块如：部门管理、角色用户、菜单及按钮授权、数据权限、系统参数、日志管理、通知公告等。在线定时任务配置；支持集群，支持多数据源。

## **主要特性**

* 完全响应式布局（支持电脑、平板、手机等所有主流设备）
* 强大的一键生成功能（包括控制器、模型、视图、菜单等）
* 支持多数据源，简单配置即可实现切换。
* 支持按钮及数据权限，可自定义部门数据权限。
* 对常用js插件进行二次封装，使js代码变得简洁，更加易维护
* 完善的XSS防范及脚本过滤，彻底杜绝XSS攻击
* Maven多项目依赖，模块及插件分项目，尽量松耦合，方便模块升级、增减模块。
* 国际化支持，服务端及客户端支持
* 完善的日志记录体系简单注解即可实现

## **技术选型**

**1、系统环境**

- Java EE 8
- Servlet 3.0
- Apache Maven 3

**2、主框架**

- Spring Boot 2.0
- Spring Framework 5.0
- Apache Shiro 1.4

**3、持久层**

- Apache MyBatis 3.4
- Hibernate Validation 6.0
- Alibaba Druid 1.1

**4、视图层**

- Bootstrap 3.3
- Hplus 4.1
- Thymeleaf 3.0


## **内置功能**

* 用户管理：用户是系统操作者，该功能主要完成系统用户配置。
* 部门管理：配置系统组织机构（公司、部门、小组），树结构展现支持数据权限。
* 岗位管理：配置系统用户所属担任职务。
* 菜单管理：配置系统菜单，操作权限，按钮权限标识等。
* 角色管理：角色菜单权限分配、设置角色按机构进行数据范围权限划分。
* 字典管理：对系统中经常使用的一些较为固定的数据进行维护。
* 参数管理：对系统动态配置常用参数。
* 通知公告：系统通知公告信息发布维护。
* 操作日志：系统正常操作日志记录和查询；系统异常信息日志记录和查询。
* 登录日志：系统登录日志记录查询包含登录异常。
* 在线用户：当前系统中活跃用户状态监控。
* 定时任务：在线（添加、修改、删除)任务调度包含执行结果日志。
* 代码生成：前后端代码的生成（java、html、xml、sql)支持CRUD下载 。
* 系统接口：根据业务代码自动生成相关的api接口文档。
* 服务监控：监视当前系统CPU、内存、磁盘、堆栈等相关信息。
* 在线构建器：拖动表单元素生成相应的HTML代码。
* 连接池监视：监视当期系统数据库连接池状态，可进行分析SQL找出系统性能瓶颈。

## **更新日志**

* v3.4.0 2019-06-03

	* 新增实例演示菜单及demo
	* 新增页签右键操作
	* 菜单管理新增打开方式
	* 新增点击某行触发的事件
	* 新增双击某行触发的事件
	* 新增单击某格触发的事件
	* 新增双击某格触发的事件
	* 新增是否启用显示细节视图
	* 支持上传任意格式文件
	* 修复角色权限注解失效问题
	* 左侧的菜单栏宽度调整
	* 新增响应完成后自定义回调函数
	* 支持前端及其他模块直接获取用户信息
	* 升级swagger到最新版2.9.2
	* 升级jquery.slimscroll到最新版1.3.8
	* 升级select2到最新版4.0.7
	* 新增角色配置本部门数据权限
	* 新增角色配置本部门及以下数据权限
	* 优化底部操作防止跳到页面顶端
	* 修改冻结列选框无效及样式问题
	* 修复部门四层级修改祖级无效问题
	* 更换开关切换按钮样式
	* 新增select2-bootstrap美化下拉框
	* 添加表格内图片预览方法
	* 修复权限校验失败跳转页面路径错误
	* 国际化资源文件调整
	* 通知公告布局调整
	* 删除页签操作功能
	* 表格树新增查询指定列值
	* 更改系统接口扫描方式及完善测试案例
	* 表格列浮动提示及字典回显默认去背景
	* 修复启用翻页记住前面的选择check没选中问题
	* 去除监控页面底部的广告
	* 日期控件功问题修复及data功能增强
	* 新增角色权限可见性（前端直接调用）
	* 新增获取当前登录用户方法（前端及子模块调用）
	* 修复热部署重启导致菜单丢失问题
	* 优化业务校验失败普通请求跳转页面
	* 操作日志新增状态条件查询
	* 操作类型支持多选条件查询
	* 通知公告防止滚动触底回弹优化
	* 其他细节优化

* v3.3.0 2019-04-01

	* 新增线程池统一管理
	* 新增支持左右冻结列
	* 新增表格字符超长浮动提示
	* 升级datepicker拓展并汉化
	* 升级druid到最新版本v1.1.14
	* 修复个人头像为图片服务器跨域问题
	* 修改上传文件按日期存储
	* 新增表格客户端分页选项
	* 新增表格的高度参数
	* 新增表格销毁方法
	* 新增表格下拉按钮切换方法
	* 新增表格分页跳转到指定页码
	* 新增表格启用点击选中行参数
	* 修复表格数据重新加载未触发部分按钮禁用
	* 使用jsonview展示操作日志参数
	* 新增方法（addTab、editTab）
	* 修改用户管理界面为Tab打开方式
	* 表单验证代码优化
	* 修复@Excel注解 prompt 属性使用报错
	* 修复combo属性Excel兼容性问题
	* 新增@Excel导入导出支持父类字段
	* 修复关闭最后选项卡无法激活滚动问题
	* 增加日期控件显示类型及回显格式扩展选项
	* 修复定时任务执行失败后入库状态为成功状态
	* 支持定时任务并发开关控制
	* 优化权限校验失败普通请求跳转页面
	* 捕获线程池执行任务抛出的异常
	* 修复IE浏览器导出功能报错
	* 新增角色管理分配用户功能
	* 新增表格翻页记住前面的选择
	* 调整用户个人中心页面
	* 修复界面存在的一些安全问题
	* 其他细节优化

* v3.2.0 2019-01-16

    * 部门修改时不允许选择最后节点
    * 修复部门菜单排序字段无效
    * 修复光驱磁盘导致服务监控异常
    * 登录界面去除check插件
    * 验证码文本字符间距修正
    * 升级SpringBoot到最新版本2.1.1
    * 升级MYSQL驱动
    * 修正登录必填项位置偏移
    * Session会话检查优化
    * Excel注解支持多级获取
    * 新增序列号生成方法
    * 修复WAR部署tomcat退出线程异常
    * 全屏操作增加默认确认/关闭
    * 修复个人信息可能导致漏洞
	* 字典数据根据下拉选择新增类型
	* 升级Summernote到最新版本v0.8.11
	* 新增用户数据导入
	* 首页主题样式更换
	* layer扩展主题更换
	* 用户管理移动端默认隐藏左侧布局
	* 详细信息弹出层显示在顶层
	* 表格支持切换状态（用户/角色/定时任务）
	* Druid数据源支持配置继承
	* 修正部分iPhone手机端表格适配问题
	* 新增防止重复提交表单方法
	* 新增表格数据统计汇总方法
	* 支持富文本上传图片文件

* v3.1.0 2018-12-03

    * 新增内网不获取IP地址
    * 新增cron表达式有效校验
    * 定时任务新增详细信息
    * 定时任务默认策略修改（不触发立即执行）
    * 定时任务显示下一个执行周期
    * 支持前端任意日期格式处理
    * 上传头像删除多余提交按钮
    * 表格增加行间隔色配置项
    * 表格增加转义HTML字符串配置项
    * 表格增加显示/隐藏指定列
    * 代码生成优化
    * 操作日志参数格式化显示
    * 页签新增新增全屏显示
    * 新增一键打包部署
	* Excel注解新增多个参数
	* 新增提交静默更新表格方法
	* 新增服务监控菜单
	
* v3.0.0 2018-10-08

    * 升级poi到最新版3.17
    * 导出修改临时目录绝对路径
    * 升级laydate升级到最新版5.0.9
    * 升级SpringBoot到最新版本2.0.5
    * 优化开始/结束时间校验限制
    * 重置密码参数表中获取默认值
    * 修复头像修改显示问题
    * 新增数据权限过滤注解
    * 新增表格检索折叠按钮
    * 新增清空（登录、操作、调度）日志
    * 固定按钮位置（提交/关闭）
    * 部门/菜单支持（展开/折叠）
    * 部分细节调整优化
    * 项目采用分模块