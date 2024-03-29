## **分页实现**

前端基于Bootstrap的轻量级表格插件 `BootstrapTable`  
后端分页组件使用Mybatis分页插件 `PageHelper`

> 分页实现流程  

1、前端调用封装好的方法$.table.init，传入后台url。
```javascript
var options = {
	url: prefix + "/list",
	columns: [{
		field: 'id',
		title: '主键'
	},
	{
		field: 'name',
		title: '名称'
	}]
};
$.table.init(options);
```

2、自定义查询条件参数（特殊情况提前设置查询条件下使用）
```javascript
var options = {
	url: prefix + "/list",
	queryParams: queryParams,
	columns: [{
		field: 'id',
		title: '主键'
	},
	{
		field: 'name',
		title: '名称'
	}]
};
$.table.init(options);

function queryParams(params) {
	var search = $.table.queryParams(params);
	search.userName = $("#userName").val();
	return search;
}
```

3、后台实现查询逻辑，调用startPage()方法即可自动完成服务端分页。
```java
@PostMapping("/list")
@ResponseBody
public TableDataInfo list(User user)
{
    startPage();  // 此方法配合前端完成自动分页
    List<User> list = userService.selectUserList(user);
    return getDataTable(list);
}
```

常见坑点1：selectPostById莫名其妙的分页。例如下面这段代码
```java
startPage();
List<User> list;
if(user != null){
    list = userService.selectUserList(user);
} else {
    list = new ArrayList<User>();
}
Post post = postService.selectPostById(1L);
return getDataTable(list);
```

原因分析：这种情况下由于user存在null的情况，就会导致`PageHelper`生产了一个分页参数，但是没有被消费，这个参数就会一直保留在这个线程上。
当这个线程再次被使用时，就可能导致不该分页的方法去消费这个分页参数，这就产生了莫名其妙的分页。  
上面这个代码，应该写成下面这个样子才能保证安全。
```java
List<User> list;
if(user != null){
	startPage();
	list = userService.selectUserList(user);
} else {
	list = new ArrayList<User>();
}
Post post = postService.selectPostById(1L);
return getDataTable(list);
```

常见坑点2：添加了startPage方法。也没有正常分页。例如下面这段代码
```java
startPage();
Post post = postService.selectPostById(1L);
List<User> list = userService.selectUserList(user);
return getDataTable(list);
```

原因分析：只对该语句以后的第一个查询（Select）语句得到的数据进行分页。  
上面这个代码，应该写成下面这个样子才能正常分页。
```java
Post post = postService.selectPostById(1L);
startPage();
List<User> list = userService.selectUserList(user);
return getDataTable(list);
```

注意：如果改为其他数据库需修改配置`application.yml` `helperDialect=你的数据库`


## **导入导出**

在实际开发中经常需要使用导入导出功能来加快数据的操作。在项目中可以使用注解来完成此项功能。
在需要被导入导出的实体类属性添加`@Excel`注解，目前支持参数如下：

| 参数                           | 类型                                  | 默认值                       | 描述                                                       |
| ------------------------------ | ------------------------------------- | ---------------------------- | ---------------------------------------------------------- |
| name                           | String                                | 空                           | 导出到Excel中的名字                                        |
| dateFormat                     | String                                | 空                           | 日期格式, 如: yyyy-MM-dd                                   |
| readConverterExp               | String                                | 空                           | 读取内容转表达式 (如: 0=男,1=女,2=未知)                    |
| height                         | String                                | 14                           | 导出时在excel中每个列的高度 单位为字符                     |
| width                          | String                                | 16                           | 导出时在excel中每个列的宽 单位为字符                       |
| suffix                         | String                                | 空                           | 文字后缀,如% 90 变成90%                                    |
| defaultValue                   | String                                | 空                           | 当值为空时,字段的默认值                                    |
| prompt                         | String                                | 空                           | 提示信息                                                   |
| combo                          | String                                | Null                         | 设置只能选择不能输入的列内容                               |
| targetAttr                     | String                                | 空                           | 另一个类中的属性名称,支持多级获取,以小数点隔开             |
| type                           | Enum                                  | Type.ALL                     | 字段类型（0：导出导入；1：仅导出；2：仅导入）              |


> 导出实现流程  

1、前端调用封装好的方法$.table.init，传入后台`exportUrl`
```javascript
var options = {
	exportUrl: prefix + "/export",
	columns: [{
		field: 'id',
		title: '主键'
	},
	{
		field: 'name',
		title: '名称'
	}]
};
$.table.init(options);
```

2、添加导出按钮事件
```html
<a class="btn btn-warning" onclick="$.table.exportExcel()">
	<i class="fa fa-download"></i> 导出
</a>
```

3、在实体变量上添加@Excel注解
```java
@Excel(name = "用户序号", prompt = "用户编号")
private Long userId;

@Excel(name = "用户名称")
private String userName;
	
@Excel(name = "用户性别", readConverterExp = "0=男,1=女,2=未知")
private String sex;

@Excel(name = "最后登陆时间", width = 30, dateFormat = "yyyy-MM-dd HH:mm:ss")
private Date loginDate;
```

4、在Controller添加导出方法
```java
@PostMapping("/export")
@ResponseBody
public AjaxResult export(User user)
{
	List<User> list = userService.selectUserList(user);
	ExcelUtil<User> util = new ExcelUtil<User>(User.class);
	return util.exportExcel(list, "用户数据");
}
```

> 导入实现流程  

1、前端调用封装好的方法$.table.init，传入后台importUrl。
```javascript
var options = {
	importUrl: prefix + "/importData",
	columns: [{
		field: 'id',
		title: '主键'
	},
	{
		field: 'name',
		title: '名称'
	}]
};
$.table.init(options);
```

2、添加导入按钮事件
```html
<a class="btn btn-info" onclick="$.table.importExcel()">
	<i class="fa fa-upload"></i> 导入
</a>
```

3、添加导入前端代码，form默认id为importForm，也可指定importExcel(id)
```html
<form id="importForm" enctype="multipart/form-data" class="mt20 mb10" style="display: none;">
	<div class="col-xs-offset-1">
		<input type="file" id="file" name="file"/>
		<div class="mt10 pt5">
			<input type="checkbox" id="updateSupport" name="updateSupport" title="如果登录账户已经存在，更新这条数据。"> 是否更新已经存在的用户数据
			 &nbsp;	<a onclick="$.table.importTemplate()" class="btn btn-default btn-xs"><i class="fa fa-file-excel-o"></i> 下载模板</a>
		</div>
		<font color="red" class="pull-left mt10">
			提示：仅允许导入“xls”或“xlsx”格式文件！
		</font>
	</div>
</form>
```

4、在实体变量上添加@Excel注解，默认为导出导入，也可以单独设置仅导入Type.IMPORT
```java
@Excel(name = "用户序号")
private Long id;

@Excel(name = "部门编号", type = Type.IMPORT)
private Long deptId;

@Excel(name = "用户名称")
private String userName;

/** 导出部门多个对象 */
@Excels({
	@Excel(name = "部门名称", targetAttr = "deptName", type = Type.EXPORT),
	@Excel(name = "部门负责人", targetAttr = "leader", type = Type.EXPORT)
})
private SysDept dept;

/** 导出部门单个对象 */
@Excel(name = "部门名称", targetAttr = "deptName", type = Type.EXPORT)
private SysDept dept;
```

5、在Controller添加导入方法，updateSupport属性为是否存在则覆盖（可选）
```java
@PostMapping("/importData")
@ResponseBody
public AjaxResult importData(MultipartFile file, boolean updateSupport) throws Exception
{
	ExcelUtil<SysUser> util = new ExcelUtil<SysUser>(SysUser.class);
	List<SysUser> userList = util.importExcel(file.getInputStream());
	String operName = ShiroUtils.getSysUser().getLoginName();
	String message = userService.importUser(userList, updateSupport, operName);
	return AjaxResult.success(message);
}
```


## **上传下载**

首先创建一张上传文件的表，例如：
```sql
drop table if exists sys_file_info;
create table sys_file_info (
  file_id           int(11)          not null auto_increment       comment '文件id',
  file_name         varchar(50)      default ''                    comment '文件名称',
  file_path         varchar(255)     default ''                    comment '文件路径',
  primary key (file_id)
) engine=innodb auto_increment=1 default charset=utf8 comment = '文件信息表';
```

> 上传实现流程

1、代码生成sys_file_info表相关代码并覆盖到对应目录。

2、参考示例修改代码。
```html
<input id="filePath" name="filePath" class="form-control" type="file">
```

```javascript
function submitHandler() {
	if ($.validate.form()) {
		uploadFile();
	}
}

function uploadFile() {
	var formData = new FormData();
	if ($('#filePath')[0].files[0] == null) {
		$.modal.alertWarning("请先选择文件路径");
		return false;
	}
	formData.append('fileName', $("#fileName").val());
	formData.append('file', $('#filePath')[0].files[0]);
	$.ajax({
		url: prefix + "/add",
		type: 'post',
		cache: false,
		data: formData,
		processData: false,
		contentType: false,
		dataType: "json",
		success: function(result) {
			$.operate.successCallback(result);
		}
	});
}
```

3、在FileInfoController添加对应上传方法
```java
	
@PostMapping("/add")
@ResponseBody
public AjaxResult addSave(@RequestParam("file") MultipartFile file, FileInfo fileInfo) throws IOException
{
	// 上传文件路径
	String filePath = RuoYiConfig.getUploadPath();
	// 上传并返回新文件名称
	String fileName = FileUploadUtils.upload(filePath, file);
	fileInfo.setFilePath(fileName);
	return toAjax(fileInfoService.insertFileInfo(fileInfo));
}
```

4、上传成功后需要预览可以对该属性格式化处理
```javascript
{
	field : 'filePath', 
	title: '文件预览',
	formatter: function(value, row, index) {
		return '<a href="javascript:downloadFile(' + row.fileId + ')"><img style="width:30;height:30px;"  src="/profile/upload/' + row.filePath + '"/></a>';
	}
},
```

如需对文件格式控制，设置`application.yml`中的`multipart`属性
```yml
# 文件上传
servlet:
   multipart:
     # 单个文件大小
     max-file-size:  10MB
     # 设置总上传的文件大小
     max-request-size:  20MB
```

注意：如果只是单纯的上传一张图片没有其他参数可以使用通用方法 `/common/upload`  
请求处理方法 `com.ruoyi.web.controller.common.CommonController`


> 下载实现流程

1、参考示例代码。
```javascript
function downloadFile(fileId){
	window.location.href = ctx + "system/fileInfo/downloadFile/" + fileId;
}
```

2、在Controller添加对应上传方法
```java
@GetMapping("/downloadFile/{fileId}")
public void downloadFile(@PathVariable("fileId") Integer fileId, HttpServletResponse response,
		HttpServletRequest request) throws Exception
{
	FileInfo sysFile = fileInfoService.selectFileInfoById(fileId);
	String filePath = sysFile.getFilePath();
	String realFileName = sysFile.getFileName() + filePath.substring(filePath.indexOf("."));
	String path = RuoYiConfig.getUploadPath() + sysFile.getFilePath();
	response.setCharacterEncoding("utf-8");
	response.setContentType("multipart/form-data");
	response.setHeader("Content-Disposition",
			"attachment;fileName=" + FileUtils.setFileDownloadHeader(request, realFileName));
	FileUtils.writeBytes(path, response.getOutputStream());
}
```


## **事务管理**

新建的Spring Boot项目中，一般都会引用spring-boot-starter或者spring-boot-starter-web，而这两个起步依赖中都已经包含了对于spring-boot-starter-jdbc或spring-boot-starter-data-jpa的依赖。
当我们使用了这两个依赖的时候，框架会自动默认分别注入DataSourceTransactionManager或JpaTransactionManager。
所以我们不需要任何额外配置就可以用`@Transactional`注解进行事务的使用。

> @Transactional注解只能应用到public可见度的方法上，可以被应用于接口定义和接口方法，方法会覆盖类上面声明的事务。

例如用户新增需要插入用户表、用户与岗位关联表、用户与角色关联表，如果插入成功，那么一起成功，如果中间有一条出现异常，那么回滚之前的所有操作，
这样可以防止出现脏数据，就可以使用事务让它实现回退。  
做法非常简单，我们只需要在方法或类添加`@Transactional`注解即可。  
 
```java
@Transactional
public int insertUser(User user)
{
	// 新增用户信息
	int rows = userMapper.insertUser(user);
	// 新增用户岗位关联
	insertUserPost(user);
	// 新增用户与角色管理
	insertUserRole(user);
	return rows;
}
```

常见坑点1：遇到非检测异常时，事务开启，也无法回滚。
例如下面这段代码，用户依旧增加成功，并没有因为后面遇到检测异常而回滚！！
```java
@Transactional
public int insertUser(User user) throws Exception
{
	// 新增用户信息
	int rows = userMapper.insertUser(user);
	// 新增用户岗位关联
	insertUserPost(user);
	// 新增用户与角色管理
	insertUserRole(user);
	// 模拟抛出SQLException异常
	boolean flag = true;
	if (flag)
	{
		throw new SQLException("发生异常了..");
	}
	return rows;
}
```
原因分析：因为Spring的默认的事务规则是遇到运行异常（RuntimeException）和程序错误（Error）才会回滚。如果想针对非检测异常进行事务回滚，可以在@Transactional 注解里使用
rollbackFor 属性明确指定异常。  
例如下面这样，就可以正常回滚：
```java
@Transactional(rollbackFor = Exception.class)
public int insertUser(User user) throws Exception
{
	// 新增用户信息
	int rows = userMapper.insertUser(user);
	// 新增用户岗位关联
	insertUserPost(user);
	// 新增用户与角色管理
	insertUserRole(user);
	// 模拟抛出SQLException异常
	boolean flag = true;
	if (flag)
	{
		throw new SQLException("发生异常了..");
	}
	return rows;
}
```

常见坑点2： 在业务层捕捉异常后，发现事务不生效。
这是许多新手都会犯的一个错误，在业务层手工捕捉并处理了异常，你都把异常“吃”掉了，Spring自然不知道这里有错，更不会主动去回滚数据。  
例如：下面这段代码直接导致用户新增的事务回滚没有生效。
```java
@Transactional
public int insertUser(User user) throws Exception
{
	// 新增用户信息
	int rows = userMapper.insertUser(user);
	// 新增用户岗位关联
	insertUserPost(user);
	// 新增用户与角色管理
	insertUserRole(user);
	// 模拟抛出SQLException异常
	boolean flag = true;
	if (flag)
	{
		try
		{
			// 谨慎：尽量不要在业务层捕捉异常并处理
			throw new SQLException("发生异常了..");
		}
		catch (Exception e)
		{
			e.printStackTrace();
		}
	}
	return rows;
}
```

推荐做法：在业务层统一抛出异常，然后在控制层统一处理。
```java
@Transactional
public int insertUser(User user) throws Exception
{
	// 新增用户信息
	int rows = userMapper.insertUser(user);
	// 新增用户岗位关联
	insertUserPost(user);
	// 新增用户与角色管理
	insertUserRole(user);
	// 模拟抛出SQLException异常
	boolean flag = true;
	if (flag)
	{
		throw new RuntimeException("发生异常了..");
	}
	return rows;
}
```

Transactional注解的常用属性表：

| 属性                           | 说明                                  | 
| ------------------------------ | ------------------------------------- |
| propagation                    | 事务的传播行为，默认值为 REQUIRED。   | 
| isolation                      | 事务的隔离度，默认值采用 DEFAULT      | 
| timeout                        | 事务的超时时间，默认值为-1，不超时。如果设置了超时时间(单位秒)，那么如果超过该时间限制了但事务还没有完成，则自动回滚事务。       | 
| read-only                      | 指定事务是否为只读事务，默认值为 false；为了忽略那些不需要事务的方法，比如读取数据，可以设置 read-only 为 true。                 | 
| rollbackFor                    | 用于指定能够触发事务回滚的异常类型，如果有多个异常类型需要指定，各类型之间可以通过逗号分隔。{xxx1.class, xxx2.class,……}          |
| noRollbackFor                  | 抛出 no-rollback-for 指定的异常类型，不回滚事务。{xxx1.class, xxx2.class,……}                                                     | 
|.... |

> 事务的传播机制是指如果在开始当前事务之前，一个事务上下文已经存在，此时有若干选项可以指定一个事务性方法的执行行为。
即:在执行一个@Transactinal注解标注的方法时，开启了事务；当该方法还在执行中时，另一个人也触发了该方法；那么此时怎么算事务呢，这时就可以通过事务的传播机制来指定处理方式。

TransactionDefinition传播行为的常量：

| 常量                                               | 含义                                                                                  |
| -------------------------------------------------- | ------------------------------------------------------------------------------------- |
| TransactionDefinition.PROPAGATION_REQUIRED         | 如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。这是默认值。    |
| TransactionDefinition.PROPAGATION_REQUIRES_NEW     | 创建一个新的事务，如果当前存在事务，则把当前事务挂起。                                |
| TransactionDefinition.PROPAGATION_SUPPORTS         | 如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务的方式继续运行。          |
| TransactionDefinition.PROPAGATION_NOT_SUPPORTED    | 以非事务方式运行，如果当前存在事务，则把当前事务挂起。                                |
| TransactionDefinition.PROPAGATION_NEVER            | 以非事务方式运行，如果当前存在事务，则抛出异常。                                      |
| TransactionDefinition.PROPAGATION_MANDATORY        | 如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常。                        |
| TransactionDefinition.PROPAGATION_NESTED           | 如果当前存在事务，则创建一个事务作为当前事务的嵌套事务来运行；如果当前没有事务，则该取值等价于TransactionDefinition.PROPAGATION_REQUIRED。 |


## **异常处理**

通常一个web框架中，有大量需要处理的异常。比如业务异常，权限不足等等。前端通过弹出提示信息的方式告诉用户出了什么错误。
通常情况下我们用try.....catch.... 对异常进行捕捉处理，但是在实际项目中对业务模块进行异常捕捉，会造成代码重复和繁杂，
我们希望代码中只有业务相关的操作，所有的异常我们单独设立一个类来处理它。全局异常就是对框架所有异常进行统一管理。
我们在可能发生异常的方法里throw抛给控制器。然后由全局异常处理器对异常进行统一处理。
如此，我们的`Controller`中的方法就可以很简洁了。  

所谓全局异常处理器就是使用`@ControllerAdvice`注解。示例如下： 

1、统一返回实体定义
```java
package com.ruoyi.common.core.domain;

import java.util.HashMap;

/**
 * 操作消息提醒
 * 
 * @author ruoyi
 */
public class AjaxResult extends HashMap<String, Object>
{
    private static final long serialVersionUID = 1L;

    /**
     * 返回错误消息
     * 
     * @param code 错误码
     * @param msg 内容
     * @return 错误消息
     */
    public static AjaxResult error(String msg)
    {
        AjaxResult json = new AjaxResult();
        json.put("msg", msg);
        json.put("code", 500);
        return json;
    }

    /**
     * 返回成功消息
     * 
     * @param msg 内容
     * @return 成功消息
     */
    public static AjaxResult success(String msg)
    {
        AjaxResult json = new AjaxResult();
        json.put("msg", msg);
        json.put("code", 0);
        return json;
    }
}
```

2、定义登录异常定义
```java
package com.ruoyi.common.exception;

/**
 * 登录异常
 * 
 * @author ruoyi
 */
public class LoginException extends RuntimeException
{
    private static final long serialVersionUID = 1L;

    protected final String message;

    public LoginException(String message)
    {
        this.message = message;
    }

    @Override
    public String getMessage()
    {
        return message;
    }
}

```

3、基于@ControllerAdvice注解的Controller层的全局异常统一处理  
```java
package com.ruoyi.framework.web.exception;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;
import com.ruoyi.common.core.domain.AjaxResult;
import com.ruoyi.common.exception.LoginException;

/**
 * 全局异常处理器
 * 
 * @author ruoyi
 */
@RestControllerAdvice
public class GlobalExceptionHandler
{
    private static final Logger log = LoggerFactory.getLogger(GlobalExceptionHandler.class);
	
	/**
     * 登录异常
     */
    @ExceptionHandler(LoginException.class)
    public AjaxResult loginException(LoginException e)
    {
        log.error(e.getMessage(), e);
        return AjaxResult.error(e.getMessage());
    }
}
```

4、测试访问请求
```java
@Controller
public class SysIndexController 
{
    /**
     * 首页方法
     */
    @GetMapping("/index")
    public String index(ModelMap mmap)
    {
        /**
         * 模拟用户未登录，抛出业务逻辑异常
         */
        SysUser user = ShiroUtils.getSysUser();
        if (StringUtils.isNull(user))
		{
            throw new LoginException("用户未登录，无法访问请求。");
        }
		mmap.put("user", user);
        return "index";
    }
}
```

根据上面代码含义，当我们未登录访问/index时就会发生LoginException业务逻辑异常，按照我们之前的全局异常配置以及统一返回实体实例化，访问后会出现AjaxResult格式JSON数据，
下面我们运行项目访问查看效果。  
界面输出内容如下所示：
```json
{
    "msg": "用户未登录，无法访问请求。",
    "code": 500
}
```

`对于一些特殊情况，如接口需要返回json，页面请求返回html可以使用如下方法`：  
```java
@ExceptionHandler(LoginException.class)
public Object loginException(HttpServletRequest request, LoginException e)
{
	log.error(e.getMessage(), e);

	if (ServletUtils.isAjaxRequest(request))
	{
		return AjaxResult.error(e.getMessage());
	}
	else
	{
		return new ModelAndView("/error/500");
	}
}
```

若依系统的全局异常处理器`GlobalExceptionHandler`  
注意：如果全部异常处理返回`json`，那么可以使用`@RestControllerAdvice`代替`@ControllerAdvice`，这样在方法上就可以不需要添加`@ResponseBody`。


## **系统日志**

在实际开发中，对于某些关键业务，我们通常需要记录该操作的内容，一个操作调一次记录方法，每次还得去收集参数等等，会造成大量代码重复。
我们希望代码中只有业务相关的操作，在项目中使用注解来完成此项功能。  

在需要被记录日志的`controller`方法上添加@Log注解，使用方法如下：  
```java
@Log(title = "用户管理", businessType = BusinessType.INSERT) 
```

支持参数如下：

| 参数                           | 类型                                  | 默认值                         | 描述                                                     |
| ------------------------------ | ------------------------------------- | ------------------------------ | -------------------------------------------------------- |
| title                          | String                                | 空                             | 操作模块                                                 |
| businessType                   | BusinessType                          | OTHER                          | 操作功能（OTHER其他 INSERT新增 UPDATE修改 DELETE删除 GRANT授权 EXPORT导出 IMPORT导入 FORCE强退 GENCODE生成代码 CLEAN清空数据）  |
| operatorType                   | OperatorType                          | MANAGE                         | 操作人类别（OTHER其他 MANAGE后台用户 MOBILE手机端用户）  |
| isSaveRequestData              | boolean                               | true                           | 是否保存请求的参数                                       |


> 关于自定义操作功能使用流程

1、在`BusinessType`中新增业务操作类型如:  
```java
/**
 * 测试
 */
TEST,
```

2、在`sys_dict_data`字典数据表中初始化操作业务类型
```sql
insert into sys_dict_data values(25, 10, '测试',     '10', 'sys_oper_type',       '',   'primary', 'N', '0', 'admin', '2018-03-16 11-33-00', 'ry', '2018-03-16 11-33-00', '测试操作');
```

3、在`Controller`中使用注解
```java
@Log(title = "测试标题", businessType = BusinessType.TEST)
```

逻辑实现代码 `com.ruoyi.framework.aspectj.LogAspect`   
查询操作详细记录可以登录系统（系统管理-操作日志）  


## **数据权限**

在实际开发中，需要设置用户只能查看哪些部门的数据，这种情况一般称为数据权限。  
例如对于销售，财务的数据，它们是非常敏感的，因此要求对数据权限进行控制，
对于基于集团性的应用系统而言，就更多需要控制好各自公司的数据了。如设置只能看本公司、或者本部门的数据，对于特殊的领导，可能需要跨部门的数据，
因此程序不能硬编码那个领导该访问哪些数据，需要进行后台的权限和数据权限的控制。


> 默认系统管理员`admin`拥有所有数据权限`（userId=1）`，默认角色拥有所有数据权限（如不需要数据权限不用设置数据权限操作）

> 关于数据权限使用流程

支持参数如下：

| 参数                           | 类型                                  | 默认值                         | 描述                                                     |
| ------------------------------ | ------------------------------------- | ------------------------------ | -------------------------------------------------------- |
| tableAlias                     | String                                | 空                             | 表的别名                                                 |

1、在（系统管理-角色管理）设置需要数据权限的角色

2、在需要数据权限控制方法上添加`@DataScope`注解，其中`u`用来表示表的别名  
```java
@DataScope(tableAlias = "u")
```

3、在`mybatis`查询底部标签添加数据范围过滤
```xml
<!-- 数据范围过滤 -->
${params.dataScope}
```

用户管理（未过滤数据权限的情况）：
```sql
select u.user_id, u.dept_id, u.login_name, u.user_name, u.email
	, u.phonenumber, u.password, u.sex, u.avatar, u.salt
	, u.status, u.del_flag, u.login_ip, u.login_date, u.create_by
	, u.create_time, u.remark, d.dept_name
from sys_user u
	left join sys_dept d on u.dept_id = d.dept_id
where u.del_flag = '0'
```

用户管理（已过滤数据权限的情况）：
```sql
select u.user_id, u.dept_id, u.login_name, u.user_name, u.email
	, u.phonenumber, u.password, u.sex, u.avatar, u.salt
	, u.status, u.del_flag, u.login_ip, u.login_date, u.create_by
	, u.create_time, u.remark, d.dept_name
from sys_user u
	left join sys_dept d on u.dept_id = d.dept_id
where u.del_flag = '0'
	and u.dept_id in (
		select dept_id
		from sys_role_dept
		where role_id = 2
	)
```

结果很明显，我们多了如下语句。通过角色部门表（sys_role_dept）完成了数据权限过滤
```sql
and u.dept_id in (
	select dept_id
	from sys_role_dept
	where role_id = 2
)
```

逻辑实现代码 `com.ruoyi.framework.aspectj.DataScopeAspect`  


## **多数据源**

在实际开发中，经常可能遇到在一个应用中可能需要访问多个数据库的情况  
在需要切换数据源`Service`或`Mapper`方法上添加`@DataSource`注解  
@DataSource(value = DataSourceType.MASTER)，其中`value`用来表示数据源名称  

> 关于多数据源使用流程（如果有多个，可以参考slave添加）

支持参数如下：

| 参数                           | 类型                                  | 默认值                         | 描述                                                     |
| ------------------------------ | ------------------------------------- | ------------------------------ | -------------------------------------------------------- |
| value                          | DataSourceType                        | DataSourceType.MASTER          | 主库                                                     |

1、在`application-druid.yml`配置从库数据源
```java 
# 从库数据源
slave:
	# 从数据源开关/默认关闭
	enabled: true
	url: jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf8&zeroDateTimeBehavior=convertToNull&useSSL=true&serverTimezone=GMT%2B8
	username: root
	password: password
```

2、在`DataSourceType`类添加数据源枚举
```java
/**
 * 从库
 */
SLAVE
```

3、在`DruidConfig`配置读取数据源
```java 
@Bean
@ConfigurationProperties("spring.datasource.druid.slave")
@ConditionalOnProperty(prefix = "spring.datasource.druid.slave", name = "enabled", havingValue = "true")
public DataSource slaveDataSource(DruidProperties druidProperties)
{
	DruidDataSource dataSource = DruidDataSourceBuilder.create().build();
	return druidProperties.dataSource(dataSource);
}
```

4、在`DruidConfig`类`dataSource`方法添加数据源
```java 
targetDataSources.put(DataSourceType.SLAVE.name(), slaveDataSource);
```

5、在需要使用多数据源方法上添加`@DataSource`注解，其中`value`用来表示数据源
```java 
@DataSource(value = DataSourceType.SLAVE)
public List<SysUser> selectUserList(SysUser user)
{
	return userMapper.selectUserList(user);
}
```

对于特殊情况可以通过`DynamicDataSourceContextHolder`手动实现数据源切换
```java 
public List<SysUser> selectUserList(SysUser user)
{
	DynamicDataSourceContextHolder.setDataSourceType(DataSourceType.SLAVE.name());
	List<SysUser> userList = userMapper.selectUserList(user);
	DynamicDataSourceContextHolder.clearDataSourceType();
	return userList;
}
```

逻辑实现代码 com.ruoyi.framework.aspectj.DataSourceAspect  

`注意：目前配置了一个从库，默认关闭状态。如果不需要多数据源不用做任何配置。
另外可新增多个从库。支持不同数据源（Mysql、Oracle、SQLServer）`


## **代码生成**

大部分项目里其实有很多代码都是重复的，几乎每个基础模块的代码都有增删改查的功能，而这些功能都是大同小异，
如果这些功能都要自己去写，将会大大浪费我们的精力降低效率。所以这种重复性的代码可以使用代码生成。 

> 关于代码生成使用流程

1、修改代码生成配置  
   单应用编辑`resources目录下的application.yml`  
   多模块编辑`ruoyi-generator`中的`resources`目录下的`generator.yml`  
   `author`:          # 开发者姓名，生成到类注释上   
   `packageName`:     # 默认生成包路径  
   `autoRemovePre`:   # 是否自动去除表前缀  
   `tablePrefix`:     # 表前缀  
   
2、新建数据库表结构（需要表注释）  
```sql
drop table if exists sys_student;
create table sys_student (
  student_id           int(11)         auto_increment    comment '学生编号',
  student_name         varchar(30)     default ''        comment '学生名称',
  student_age          int(3)          default null      comment '学生年龄',
  primary key (student_id)
) engine=innodb auto_increment=1 comment = '学生信息表';
```

3、登录系统（系统工具 -> 代码生成）
   找到`sys_student`表，点击生成代码会得到一个ruoyi.zip   
   执行`sql`文件，覆盖文件到对应目录即可
   
多模块所有代码生成的相关业务逻辑代码在`ruoyi-generator`模块，可以自行调整或剔除

## **定时任务**  
 
在实际项目开发中Web应用有一类不可缺少的，那就是定时任务。
定时任务的场景可以说非常广泛，比如某些视频网站，购买会员后，每天会给会员送成长值，每月会给会员送一些电影券；
比如在保证最终一致性的场景中，往往利用定时任务调度进行一些比对工作；比如一些定时需要生成的报表、邮件；比如一些需要定时清理数据的任务等。
所以我们提供方便友好的web界面，实现动态管理任务，可以达到动态控制定时任务启动、暂停、重启、删除、添加、修改等操作，极大地方便了开发过程。

> 关于定时任务使用流程

1、后台添加定时任务处理类，并且添加对应Bean注解（@Component或@Service）。如下：
```java
/**
 * 定时任务调度测试
 * 
 * @author ruoyi
 */
@Component("ryTask")
public class RyTask
{
    public void ryParams(String params)
    {
        System.out.println("执行有参方法：" + params);
    }

    public void ryNoParams()
    {
        System.out.println("执行无参方法");
    }
}
```

2、前端新建定时任务信息（系统监控 -> 定时任务）  
    任务名称：对应后台bean注解value名称  
	任务组名：对应的定时任务组名  
	方法名称：对应后台任务方法名称
	方法参数：对应后台任务方法名称值，没有可不填
	执行表达式：可查询官方cron表达式介绍  
	执行策略：定时任务自定义执行策略  
	并发执行：是否需要多个任务间同时执行  
	状态：是否启动定时任务  
	备注：定时任务描述信息  
	
3、点击执行一次，测试定时任务是否正常及调度日志是否正确记录，如正常执行表示任务配置成功。

执行策略详解：  
`立即执行`（所有misfire的任务会马上执行）打个比方，如果9点misfire了，在10：15系统恢复之后，9点，10点的misfire会马上执行  
`执行一次`（会合并部分的misfire，正常执行下一个周期的任务）假设9，10的任务都misfire了，系统在10：15分起来了。只会执行一次misfire，下次正点执行。    
`放弃执行`(所有的misfire不管，执行下一个周期的任务)

cron表达式语法:  
[秒] [分] [小时] [日] [月] [周] [年]

| 说明  | 必填  | 允许填写的值       |	允许的通配符     |
| ----- | ----- | ------------------ | ----------------- |
| 秒    | 是    | 0-59               | , - * /           |
| 分    | 是    | 0-59               | , - * /           |
| 时    | 是    | 0-23               | , - * /           |
| 日    | 是    | 1-31               | , - * /           |
| 月    | 是    | 1-12 / JAN-DEC     | , - * ? / L W     |
| 周    | 是    | 1-7 or SUN-SAT     | , - * ? / L #     |
| 年    | 是    | 1970-2099          | , - * /           |

通配符说明:  
`*` 表示所有值。 例如:在分的字段上设置 *,表示每一分钟都会触发  
`?` 表示不指定值。使用的场景为不需要关心当前设置这个字段的值。例如:要在每月的10号触发一个操作，但不关心是周几，所以需要周位置的那个字段设置为”?” 具体设置为 0 0 0 10 * ?  
`-` 表示区间。例如 在小时上设置 “10-12”,表示 10,11,12点都会触发  
`,` 表示指定多个值，例如在周字段上设置 “MON,WED,FRI” 表示周一，周三和周五触发  
`/` 用于递增触发。如在秒上面设置”5/15” 表示从5秒开始，每增15秒触发(5,20,35,50)。 在月字段上设置’1/3’所示每月1号开始，每隔三天触发一次  
`L` 表示最后的意思。在日字段设置上，表示当月的最后一天(依据当前月份，如果是二月还会依据是否是润年[leap]), 在周字段上表示星期六，相当于”7”或”SAT”。如果在”L”前加上数字，则表示该数据的最后一个。例如在周字段上设置”6L”这样的格式,则表示“本月最后一个星期五”  
`W` 表示离指定日期的最近那个工作日(周一至周五). 例如在日字段上置”15W”，表示离每月15号最近的那个工作日触发。如果15号正好是周六，则找最近的周五(14号)触发, 如果15号是周未，则找最近的下周一(16号)触发.如果15号正好在工作日(周一至周五)，则就在该天触发。如果指定格式为 “1W”,它则表示每月1号往后最近的工作日触发。如果1号正是周六，则将在3号下周一触发。(注，”W”前只能设置具体的数字,不允许区间”-“)  
`#` 序号(表示每月的第几个周几)，例如在周字段上设置”6#3”表示在每月的第三个周六.注意如果指定”#5”,正好第五周没有周六，则不会触发该配置(用在母亲节和父亲节再合适不过了) ；小提示：’L’和 ‘W’可以一组合使用。如果在日字段上设置”LW”,则表示在本月的最后一个工作日触发；周字段的设置，若使用英文字母是不区分大小写的，即MON与mon相同  

常用表达式例子:

| 表达式                         | 说明                                                        |
| ------------------------------ | ----------------------------------------------------------- |
| 0 0 2 1 * ? *                  | 表示在每月的1日的凌晨2点调整任务                            |
| 0 15 10 ? * MON-FRI            | 表示周一到周五每天上午10:15执行作业                         |
| 0 15 10 ? 6L 2002-2006         | 表示2002-2006年的每个月的最后一个星期五上午10:15执行作      |
| 0 0 10,14,16 * * ?             | 每天上午10点，下午2点，4点                                  |
| 0 0/30 9-17 * * ?              | 朝九晚五工作时间内每半小时                                  |
| 0 0 12 ? * WED                 | 表示每个星期三中午12点                                      |
| 0 0 12 * * ?                   | 每天中午12点触发                                            |
| 0 15 10 ? * *                  | 每天上午10:15触发                                           |
| 0 15 10 * * ?                  | 每天上午10:15触发                                           |
| 0 15 10 * * ? *                | 每天上午10:15触发                                           |
| 0 15 10 * * ? 2005             | 2005年的每天上午10:15触发                                   |
| 0 * 14 * * ?                   | 在每天下午2点到下午2:59期间的每1分钟触发                    |
| 0 0/5 14 * * ?                 | 在每天下午2点到下午2:55期间的每5分钟触发                    |
| 0 0/5 14,18 * * ?              | 在每天下午2点到2:55期间和下午6点到6:55期间的每5分钟触发     |
| 0 0-5 14 * * ?                 | 在每天下午2点到下午2:05期间的每1分钟触发                    |
| 0 10,44 14 ? 3 WED             | 每年三月的星期三的下午2:10和2:44触发                        |
| 0 15 10 ? * MON-FRI            | 周一至周五的上午10:15触发                                   |
| 0 15 10 15 * ?                 | 每月15日上午10:15触发                                       |
| 0 15 10 L * ?                  | 每月最后一日的上午10:15触发                                 |
| 0 15 10 ? * 6L                 | 每月的最后一个星期五上午10:15触发                           |
| 0 15 10 ? * 6L 2002-2005       | 2002年至2005年的每月的最后一个星期五上午10:15触发           |
| 0 15 10 ? * 6#3                | 每月的第三个星期五上午10:15触发                             |

多模块所有定时任务的相关业务逻辑代码在`ruoyi-quartz`模块，可以自行调整或剔除

`注意：不同数据源定时任务都有对应脚本，Oracle、Mysql已经有了，其他的可自行下载执行`

## **系统接口**

在现在的开发过程中还有很大一部分公司都是以口口相传的方式来进行前后端的联调，而接口文档很大一部分都只停留在了说说而已的地步，或者写了代码再写文档。
还有一点就是文档的修改，定义好的接口并不是一成不变的，可能在开发过程中文档修改不止一次的变化，这个时候就会很难受了。
只要不是强制性要求，没人会愿意写这东西，而且在写的过程中，一个字母的错误就会导致联调时候的很大麻烦，但是通过Swagger，我们可以省略了这一步，而且文档出错率近乎于零，
只要你在写代码的时候，稍加几个注解，文档自动生成。

1、在控制层`Controller`中添加注解来描述接口信息如:  
```java
@Api("参数配置")
@Controller
@RequestMapping("/system/config")
public class ConfigController
```

2、在方法中配置接口的标题信息
```sql
@ApiOperation("查询参数列表")
@ResponseBody
public TableDataInfo list(Config config)
{
	startPage();
	List<Config> list = configService.selectConfigList(config);
	return getDataTable(list);
}
```

3、在`系统工具-系统接口`测试相关接口

`注意：SwaggerConfig可以指定根据注解或者包名扫描具体的API`

API详细说明

| 作用范围                | API                                  |   使用位置                          |
| ----------------------- | ------------------------------------ | ----------------------------------- |
| 协议集描述              | @Api                                 |   用于controller类上                |
| 对象属性                | @ApiModelProperty                    |   用在出入参数对象的字段上          |
| 协议描述                | @ApiOperation                        |   用在controller的方法上            |
| Response集              | @ApiResponses                        |   用在controller的方法上            |
| Response                | @ApiResponse                         |   用在 @ApiResponses里边            |
| 非对象参数集            | @ApiImplicitParams                   |   用在controller的方法上            |
| 非对象参数描述          | @ApiImplicitParam                    |   用在@ApiImplicitParams的方法里边  |
| 描述返回对象的意义      | @ApiModel                            |   用在返回对象类上                  |

 
`api`标记，用在类上，说明该类的作用。可以标记一个Controller类做为swagger 文档资源，使用方式：
```java
@Api(value = "/user", description = "用户管理")
```

与Controller注解并列使用。 属性配置：

| 属性名称              | 备注                                             |
| --------------------- | ------------------------------------------------ |
| value                 | url的路径值                                      |
| tags                  | 如果设置这个值、value的值会被覆盖                |
| description           | 对api资源的描述                                  |
| basePath              | 基本路径可以不配置                               |
| position              | 如果配置多个Api 想改变显示的顺序位置             |
| produces              | For example, "application/json, application/xml" |
| consumes              | For example, "application/json, application/xml" |
| protocols             | Possible values: http, https, ws, wss.           |
| authorizations        | 高级特性认证时配置                               |
| hidden                | 配置为true 将在文档中隐藏                        |

`ApiOperation`标记，用在方法上，说明方法的作用，每一个url资源的定义,使用方式：
```java
@ApiOperation("获取用户信息")
```

与Controller中的方法并列使用，属性配置：

| 属性名称              | 备注                                             |
| --------------------- | ------------------------------------------------ |
| value                 | url的路径值                                      |
| tags                  | 如果设置这个值、value的值会被覆盖                |
| description           | 对api资源的描述                                  |
| basePath              | 基本路径可以不配置                               |
| position              | 如果配置多个Api 想改变显示的顺序位置             |
| produces              | For example, "application/json, application/xml" |
| consumes              | For example, "application/json, application/xml" |
| protocols             | Possible values: http, https, ws, wss.           |
| authorizations        | 高级特性认证时配置                               |
| hidden                | 配置为true将在文档中隐藏                         |
| response              | 返回的对象                                       |
| responseContainer     | 这些对象是有效的 "List", "Set" or "Map".，其他无效            |
| httpMethod            | "GET", "HEAD", "POST", "PUT", "DELETE", "OPTIONS" and "PATCH" |
| code                  | http的状态码 默认 200                                         |
| extensions            | 扩展属性                                                      |

`ApiParam`标记，请求属性，使用方式：
```java
public TableDataInfo list(@ApiParam(value = "查询用户列表", required = true)User user)
```

与Controller中的方法并列使用，属性配置：

| 属性名称              | 备注                                             |
| --------------------- | ------------------------------------------------ |
| name                  | 属性名称                                         |
| value                 | 属性值                                           |
| defaultValue          | 默认属性值                                       |
| allowableValues       | 可以不配置                                       |
| required              | 是否属性必填                                     |
| access                | 不过多描述                                       |
| allowMultiple         | 默认为false                                      |
| hidden                | 隐藏该属性                                       |
| example               | 举例子                                           |

`ApiResponse`标记，响应配置，使用方式：
```java
@ApiResponse(code = 400, message = "查询用户失败")
```

与Controller中的方法并列使用，属性配置：

| 属性名称              | 备注                                             |
| --------------------- | ------------------------------------------------ |
| code                  | http的状态码                                     |
| message               | 描述                                             |
| response              | 默认响应类 Void                                  |
| reference             | 参考ApiOperation中配置                           |
| responseHeaders       | 参考 ResponseHeader 属性配置说明                 |
| responseContainer     | 参考ApiOperation中配置                           |

`ApiResponses`标记，响应集配置，使用方式:
```java
@ApiResponses({ @ApiResponse(code = 400, message = "无效的用户") })
```

与Controller中的方法并列使用，属性配置：

| 属性名称              | 备注                                             |
| --------------------- | ------------------------------------------------ |
| value                 | 多个ApiResponse配置                              |

`ResponseHeader`标记，响应头设置，使用方法
```java
@ResponseHeader(name="head",description="响应头设计")
```

与Controller中的方法并列使用，属性配置：

| 属性名称              | 备注                                             |
| --------------------- | ------------------------------------------------ |
| name                  | 响应头名称                                       |
| description           | 描述                                             |
| response              | 默认响应类 void                                  |
| responseContainer     | 参考ApiOperation中配置                           |


## **国际化支持**

在我们开发WEB项目的时候，项目可能涉及到在国外部署或者应用，也有可能会有国外的用户对项目进行访问，那么在这种项目中，
为客户展现的页面或者操作的信息就需要使用不同的语言，这就是我们所说的项目国际化。
目前项目已经支持多语言国际化，接下来我们介绍如何使用。

> 关于国际化使用流程

1、修改`I18nConfig`设置默认语言，如默认`中文`：  
```java
// 默认语言，英文可以设置Locale.US
slr.setDefaultLocale(Locale.SIMPLIFIED_CHINESE);
```
   
2、修改配置`application.yml`中的`basename`国际化文件，默认是i18n路径下messages文件  
（比如现在国际化文件是xx_zh_CN.properties、xx_en_US.properties，那么`basename`配置应为是`i18n/xx`
```yml
spring:
  # 资源信息
  messages:
    # 国际化资源文件路径
    basename: static/i18n/messages
```

3、`i18n`目录文件下定义资源文件  
美式英语 messages_en_US.properties
```properties
user.login.username=User name
user.login.password=Password
user.login.code=Security code
user.login.remember=Remember me
user.login.submit=Sign In
```
中文简体 messages_zh_CN.properties
```properties
user.login.username=用户名
user.login.password=密码
user.login.code=验证码
user.login.remember=记住我
user.login.submit=登录
```

4、html使用国际化#{资源文件key}
```html
<form id="signupForm">
	<h4 class="no-margins">登录：</h4>
	<p class="m-t-md">你若不离不弃，我必生死相依</p>
	<input type="text"     name="username" class="form-control uname"  th:placeholder="#{user.login.username}"   />
	<input type="password" name="password" class="form-control pword"  th:placeholder="#{user.login.password}"   />
	<div class="row m-t" th:if="${captchaEnabled==true}">
		<div class="col-xs-6">
			<input type="text" name="validateCode" class="form-control code" th:placeholder="#{user.login.code}" maxlength="5" autocomplete="off">
		</div>
		<div class="col-xs-6">
			<a href="javascript:void(0);" title="点击更换验证码">
				<img th:src="@{captcha/captchaImage(type=${captchaType})}" class="imgcode" width="85%"/>
			</a>
		</div>
	</div>
	<div class="checkbox-custom" th:classappend="${captchaEnabled==false} ? 'm-t'">
		<input type="checkbox" id="rememberme" name="rememberme"> <label for="rememberme" th:text="#{user.login.remember}">记住我</label>
	</div>
	<button class="btn btn-success btn-block" id="btnSubmit" data-loading="正在验证登录，请稍后..." th:text="#{user.login.submit}">登录</button>
</form>
```

5、java代码使用MessageUtils获取国际化
```java
MessageUtils.message("user.login.username")
MessageUtils.message("user.login.password")
MessageUtils.message("user.login.code")
MessageUtils.message("user.login.remember")
MessageUtils.message("user.login.submit")
```

6、js使用国际化
首先在文件引入jquery-i18n-properties依赖，然后在初始化后即可通过JS函数获取对应国际化文件的内容。
```javascript
<!--jQuery国际化插件-->
<script src="../static/js/jquery.i18n.properties.min.js" th:src="@{/js/jquery.i18n.properties.min.js}"></script>

<script th:inline="javascript">
	//获取应用路径
	var ROOT = [[${#servletContext.contextPath}]];

	//获取默认语言
	var LANG_COUNTRY = [[${#locale.language+'_'+#locale.country}]];

	//初始化i18n插件
	$.i18n.properties({
		path: ROOT + '/i18n/',//这里表示访问路径
		name: 'messages',//文件名开头
		language: LANG_COUNTRY,//文件名语言 例如en_US
		mode: 'map'//默认值
	});

	//初始化i18n函数
	function i18n(msgKey) {
		try {
			return $.i18n.prop(msgKey);
		} catch (e) {
			return msgKey;
		}
	}

	//获取国际化翻译值
	console.log(i18n('user.login.username'));
	console.log(i18n('user.login.password'));
	console.log(i18n('user.login.code'));
	console.log(i18n('user.login.remember'));
	console.log(i18n('user.login.submit'));
</script>
```

7、界面定义切换语言
```html
<a href="?lang=en_US"> 英语 </a>  
<a href="?lang=zh_CN"> 中文 </a>  
```