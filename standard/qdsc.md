## **前端组件**

若依封装了一些常用的JS组件方法。

| 名称         | 代码           | 介绍                 |
| ------------ |--------------- | -------------------- |
| 表格         | $.table        | 表格封装处理         |
| 表格树       | $.treeTable    | 表格树封装处理       |
| 表单         | $.form         | 表单封装处理         |
| 弹出层       | $.modal        | 弹出层封装处理       |
| 操作         | $.operate      | 操作封装处理         |
| 校验         | $.validate     | 校验封装处理         |
| 树插件       | $.tree         | 树插件封装处理       |
| 通用方法     | $.common       | 通用方法封装处理     |

## **通用方法**


支持属性

| 方法                            | 参数                                            | 介绍                               |
| ------------------------------- | ----------------------------------------------- | ---------------------------------- |
| $.table.init();                 | options（选项参数）                             | 初始化表格参数                     |
| $.table.search();               | formId（表单ID）                                | 搜索-默认第一个form                |
| $.table.exportExcel();          | formId（表单ID）                                | 导出-默认第一个form                |
| $.table.importExcel();          | formId（表单ID）                                | 导入-默认importForm                |
| $.table.importTemplate();       | formId（表单ID）                                | 摸版下载                           |
| $.table.refresh();              | 无                                              | 刷新表格                           |
| $.table.selectColumns();        | column（查询列值）                              | 查询表格指定列值                   |
| $.table.selectFirstColumns();   | 无                                              | 查询表格首列值                     |
| $.table.destroy();              | tableId（表格ID）                               | 销毁表格-默认options.id            |
| $.table.serialNumber();         | index（序号）                                   | 序列号生成                         |
| $.table.dropdownToggle();       | value（内容）                                   | 下拉按钮切换                       |
| $.table.imageView();            | value（内容）, path（路径）, target（打开方式） | 图片预览                           |
| $.table.showColumn();           | column（列值）                                  | 显示表格特定的列                   |
| $.table.hideColumn();           | column（列值）                                  | 隐藏表格特定的列                   |
| $.table.tooltip();              | value（内容）, length（截取长度）               | 超出指定长度浮动提示               |
| $.table.selectDictLabel();      | datas（字典列表）, value（当前值）              | 回显数据字典                       |
| $.treeTable.init();             | options（选项参数）                             | 初始化表格树参数                   |
| $.treeTable.search();           | formId（表单ID）                                | 搜索-默认第一个form                |
| $.treeTable.refresh();          | 无                                              | 刷新表格树                         |
| $.form.reset();                 | formId（表单ID）                                | 表单重置                           |
| $.form.selectCheckeds();        | name（name名称）                                | 获取选中复选框项                   |
| $.form.selectSelects();         | name（id名称）                                  | 获取选中下拉框项                   |
| $.modal.icon                    | type（图标类型）                                | 显示图标                           |
| $.modal.msg                     | content（内容）, type（图标类型）               | 消息提示                           |
| $.modal.msgError();             | content（内容）                                 | 错误消息                           |
| $.modal.msgSuccess();           | content（内容）                                 | 成功消息                           |
| $.modal.msgWarning();           | content（内容）                                 | 警告消息                           |
| $.modal.alert                   | content（内容）, type（图标类型）               | 消息提示                           |
| $.modal.msgReload               | msg（消息）, type（图标类型）                   | 消息提示并刷新父窗体               |
| $.modal.alertError();           | content（内容）                                 | 错误提示                           |
| $.modal.alertSuccess();         | content（内容）                                 | 成功提示                           |
| $.modal.alertWarning();         | content（内容）                                 | 警告提示                           |
| $.modal.close();                | 无                                              | 关闭窗体                           |
| $.modal.closeAll                | 无                                              | 关闭全部窗体                       |
| $.modal.confirm();              | content（内容）, callBack（回调函数）           | 确认窗体                           |
| $.modal.open();                 | title, url, width, height, callBack（回调函数） | 弹出层指定宽度                     |
| $.modal.openOptions();          | options（选项参数）                             | 弹出层指定参数选项                 |
| $.modal.openFull();             | title, url, width, height                       | 弹出层全屏                         |
| $.modal.openTab                 | title（标题）, url（地址）                      | 选卡页方式打开                     |
| $.modal.disable                 | 无                                              | 禁用按钮                           |
| $.modal.enable                  | 无                                              | 启用按钮                           |
| $.modal.loading();              | message（提示消息）                             | 打开遮罩层                         |
| $.modal.closeLoading();         | 无                                              | 关闭遮罩层                         |
| $.modal.reload();               | 无                                              | 重新加载                           |
| $.operate.submit();             | url, type, dataType, data, callback（回调函数） | 提交数据                           |
| $.operate.post();               | url（地址）, data（数据）, callback（回调函数） | post方式请求提交数据               |
| $.operate.get();                | url（地址）, callback（回调函数）               | get请求传输数据                    |
| $.operate.detail();             | id（数据ID）                                    | 详细信息                           |
| $.operate.remove();             | id（数据ID）                                    | 删除信息                           |
| $.operate.removeAll();          | 无                                              | 批量删除信息                       |
| $.operate.clean();              | 无                                              | 清空信息                           |
| $.operate.add();                | id（数据ID）                                    | 添加信息                           |
| $.operate.addTab();             | id（数据ID）                                    | 添加信息（选项卡方式）             |
| $.operate.addFull();            | id（数据ID）                                    | 添加信息 全屏                      |
| $.operate.addUrl();             | id（数据ID）                                    | 添加访问地址                       |
| $.operate.edit();               | id（数据ID）                                    | 修改信息                           |
| $.operate.editTab();            | id（数据ID）                                    | 修改信息（选项卡方式）             |
| $.operate.editFull();           | id（数据ID）                                    | 修改信息 全屏                      |
| $.operate.editUrl();            | id（数据ID）                                    | 修改访问地址                       |
| $.operate.save();               | url（地址）, data（数据）, callback（回调函数） | 保存信息                           |
| $.operate.saveModal();          | url（地址）, data（数据）, callback（回调函数） | 保存信息 弹出提示框                |
| $.operate.saveTab();            | url（地址）, data（数据）, callback（回调函数） | 保存选项卡信息                     |
| $.operate.ajaxSuccess();        | result（返回结果）                              | 保存结果弹出msg刷新table表格       |
| $.operate.saveSuccess();        | result（返回结果）                              | 保存结果提示msg                    |
| $.operate.successCallback();    | result（返回结果）                              | 成功回调执行事件（静默更新）       |
| $.operate.successTabCallback(); | result（返回结果）                              | 选项卡成功回调执行事件（静默更新） |
| $.validate.unique();            | value（返回标识）                               | 判断返回标识是否唯一               |
| $.validate.form();              | formId（表单ID）                                | 表单验证-默认第一个form            |
| $.tree.init();                  | options（选项参数）                             | 初始化树结构                       |
| $.tree.searchNode();            | 无                                              | 搜索节点                           |
| $.tree.selectByIdName();        | treeId, treeName, node                          | 根据Id和Name选中指定节点           |
| $.tree.showAllNode();           | nodes（全部节点数据）                           | 显示所有节点                       |
| $.tree.hideAllNode();           | nodes（全部节点数据）                           | 隐藏所有节点                       |
| $.tree.showParent();            | treeNode（节点数据）                            | 显示所有父节点                     |
| $.tree.showChildren();          | treeNode（节点数据）                            | 显示所有孩子节点                   |
| $.tree.updateNodes();           | nodeList（全部节点数据）                        | 更新节点状态                       |
| $.tree.getCheckedNodes();       | column（列值）                                  | 获取当前被勾选集合                 |
| $.tree.notAllowParents();       | _tree（树对象）                                 | 不允许根父节点选择                 |
| $.tree.toggleSearch();          | 无                                              | 隐藏/显示搜索栏                    |
| $.tree.collapse();              | 无                                              | 树折叠                             |
| $.tree.expand();                | 无                                              | 树展开                             |
| $.common.isEmpty();             | value（值）                                     | 判断字符串是否为空                 |
| $.common.isNotEmpty();          | value（值）                                     | 判断一个字符串是否为非空串         |
| $.common.nullToStr();           | value（值）                                     | 空对象转字符串                     |
| $.common.visible();             | value（值）                                     | 是否显示数据 为空默认为显示        |
| $.common.trim();                | value（值）                                     | 空格截取                           |
| $.common.random();              | min（最小）, max（最大）                        | 指定随机数返回                     |
| $.common.startWith();           | value（值）, start（开始值）                    | 判断字符串是否是以start开头        |
| $.common.endWith();             | value（值）, end（结束值）                      | 判断字符串是否是以end结尾          |


## **表格使用**

表格组件基于`bootstrap table`组件进行封装，轻松实现数据表格。  

* 表格初始化 $.table.init 

表的各项(Table options )

| 参数                           | 类型                                  | 默认值                                      | 描述                                                     |
| ------------------------------ | ------------------------------------- | ------------------------------------------- | -------------------------------------------------------- |
| url                            | String                                | Null                                        | 请求后台的URL                                            |
| uniqueId                       | String                                | Null                                        | 指定唯一列属性  配合删除/修改使用 未指定则使用表格行首列 |
| createUrl                      | String                                | Null                                        | 新增URL 配合使用 $.operate.add()，$.operate.addTab()     |
| updateUrl                      | String                                | Null                                        | 修改URL 配合使用 $.operate.edit()，$.operate.editTab()   |
| removeUrl                      | String                                | Null                                        | 删除URL 配合使用 $.operate.remove()                      |
| exportUrl                      | String                                | Null                                        | 导出URL 配合使用 $.table.exportExcel()                   |
| importUrl                      | String                                | Null                                        | 导入URL 配合使用 $.table.importExcel()                   |
| detailUrl                      | String                                | Null                                        | 详细URL 配合使用 $.operate.detail()                      |
| cleanUrl                       | String                                | Null                                        | 清空URL 配合使用 $.operate.clean()                       |
| importTemplateUrl              | String                                | Null                                        | 摸版URL 配合使用 $.table.importTemplate()                |
| height                         | String                                | undefined                                   | 表格的高度                                               |
| striped                        | String                                | false                                       | 是否显示行间隔色                                         |
| sortName                       | String                                | Null                                        | 排序列名称                                               |
| sortOrder                      | String                                | Null                                        | 排序方式 asc 或者 desc                                   |
| pagination                     | Boolean                               | true                                        | 默认为true表格的底部工具栏会显示分页条，设为false不显示  |
| pageSize                       | int                                   | 10                                          | 每页的记录行数（*）                                      |
| pageList                       | Array                                 | [10, 25, 50]                                | 可供选择的每页的行数                                     |
| id                             | String                                | bootstrap-table                             | 表格ID属性                                               |
| toolbar                        | String                                | toolbar                                     | 表格工具栏ID属性                                         |
| escape                         | Boolean                               | false                                       | 是否转义HTML字符串                                       |
| showFooter                     | Boolean                               | false                                       | 默认为false隐藏表尾，设为true显示                        |
| sidePagination                 | String                                | server                                      | server启用服务端分页client客户端分页                     |
| search                         | Boolean                               | true                                        | 默认为true显示搜索框功能，设为false隐藏                  |
| searchText                     | String                                | ''                                          | 搜索框初始显示的内容，需要启用search设为true     |
| showSearch                     | Boolean                               | true                                        | 默认为true显示检索信息，设为false隐藏                    |
| showPageGo                     | Boolean                               | false                                       | 默认为false不显示跳转页，设为true显示                    |
| showRefresh                    | Boolean                               | true                                        | 默认为true显示刷新按钮，设为false隐藏                    |
| showColumns                    | Boolean                               | true                                        | 默认为true显示某列下拉菜单，设为false隐藏                |
| showToggle                     | Boolean                               | true                                        | 默认为true显示视图切换按钮，设为false隐藏                |
| showExport                     | Boolean                               | true                                        | 默认为true显示导出文件按钮，设为false隐藏                |
| clickToSelect                  | Boolean                               | false                                       | 默认为false不启用点击选中行，设为true启用                |
| detailView                     | Boolean                               | false                                       | 是否启用显示细节视图                                     |
| onClickRow                     | Function                              | onClickRow(row, $element)                   | 点击某行触发的事件                                       |
| onDblClickRow                  | Function                              | onDblClickRow(row, $element)                | 双击某行触发的事件                                       |
| onClickCell                    | Function                              | onClickCell(field, value, row, $element)    | 单击某格触发的事件                                       |
| onDblClickCell                 | Boolean                               | onDblClickCell(field, value, row, $element) | 双击某格触发的事件                                       |
| rememberSelected               | Boolean                               | false                                       | 默认为false不启用翻页记住前面的选择，设为true启用        |
| fixedColumns                   | Boolean                               | false                                       | 默认为false禁用冻结列，设为true启用冻结列（左侧）        |
| fixedNumber                    | int                                   | 0                                           | 冻结列的个数，当fixedColumns设为true有效（左侧）         |
| rightFixedColumns              | Boolean                               | false                                       | 默认为false禁用冻结列，设为true启用冻结列（右侧）        |
| rightFixedNumber               | int                                   | 0                                           | 冻结列的个数，当fixedColumns设为true有效（右侧）         |
| onReorderRow                   | Function                              | onReorderRow: function (data)               | 当拖拽结束后处理函数                                     |
| rowStyle                       | Function                              | rowStyle(row, index)                        | 改变某行的格式，需要两个参数：row行的数据index行的索引   |
| params                         | Array                                 | Null                                        | 当请求数据时，你可以通过修改queryParams向服务器发送参数  |
| columns                        | Array                                 | Null                                        | 默认空数组，在JS里面定义 参考列的各项(Column options )   |
| responseHandler                | object                                | responseHandler(res)                        | 在加载服务器发送来的数据之前，处理数据的格式             |
| onLoadSuccess                  | object                                | onLoadSuccess(data)                         | 当所有数据被加载时触发处理函数                           |
| exportOptions                  | Array                                 | [0]                                         | 前端导出忽略列索引如[0,5,10]                             |
| detailFormatter                | Function                              | (index, row, element)                       | detailView设为true，启用了显示detail view。用于格式化细节视图 |

列的各项(Column options )  

| 参数                           | 类型                                  | 默认值                         | 描述                           |
| ------------------------------ | ------------------------------------- | ------------------------------ | ------------------------------ |
| radio                          | Boolean                               | false                          | 默认false不显示radio（单选按钮），设为true则显示，radio宽度是固定的                                       |
| checkbox                       | Boolean                               | false                          | 默认false不显示checkbox（复选框），设为true则显示，checkbox的每列宽度已固定                               |
| field                          | String                                | Null                           | 是每列的字段名，不是表头所显示的名字，通过这个字段名可以给其赋值，相当于key，表内唯一                     |
| title                          | String                                | Null                           | 这个是表头所显示的名字，不唯一，如果你喜欢，可以把所有表头都设为相同的名字                                |
| titleTooltip                   | String                                | true                           | 当悬浮在某控件上，出现提示 - 参考 Bootstrap 提示工具（Tooltip）插件                                       |
| class                          | boolean                               | false                          | 表格列样式                                                                                                |
| rowspan                        | Number                                | true                           | 每格所占的行数                                                                                            |
| colspan                        | Number                                | true                           | 每格所占的列数                                                                                            |
| align                          | String                                | true                           | 每格内数据的对齐方式，有：left（靠左）、right（靠右）、center（居中）                                     |
| halign                         | String                                | true                           | table header（表头）的对齐方式，有：left（靠左）、right（靠右）、center（居中）                           |
| falign                         | String                                | true                           | table footer（表脚，的对齐方式，有：left（靠左）、right（靠右）、center（居中）                           |
| valign                         | String                                | true                           | 每格数据的对齐方式，有：top（靠上）、middle（居中）、bottom（靠下）                                       |
| width                          | Number                                | Null                           | 每列的宽度。如果没有自定义宽度自适应                                                                      |
| sortable                       | Boolean                               | false                          | 默认false就默认显示，设为true则会被排序                                                                   |
| order                          | String                                | asc                            | 默认的排序方式为"asc（升序）"，也可以设为"desc（降序）"。                                                 |
| visible                        | Boolean                               | true                           | 默认为true显示该列，设为false则隐藏该列                                                                   |
| cardVisible                    | Boolean                               | true                           | 默认为true显示该列，设为false则隐藏。                                                                     |
| switchable                     | Boolean                               | true                           | 默认为true显示该列，设为false则禁用列项目的选项卡。                                                       |
| clickToSelect                  | Boolean                               | true                           | 默认true不响应，设为false则当点击此行的某处时，不会自动选中此行的checkbox（复选框）或radiobox（单选按钮） |
| formatter                      | Function                              | Null                           | 某格的数据转换函数，需要三个参数： -value： field（字段名） -row：行的数据  -index：行的（索引）index     |
| footerFormatter                | Function                              | Null                           | 某格的数据转换函数，需要一个参数： -data： 所有行数据的数组 函数需要返回（return）footer某格内所要显示的字符串的格式 |
| events                         | Object                                | Null                           | 当某格使用formatter函数时，事件监听会响应，需要四个参数： -event：-value：字段名 -row：行数据 -index：此行的index    |
| sorter                         | Function                              | Null                           | 自定义的排序函数，实现本地排序，需要两个参数： - a：第一个字段名  - b：第二个字段名                       |
| sortName                       | String                                | Null                           | 排序列名称                                                                                                |
| cellStyle                      | Function                              | Null                           | 对某列中显示样式改变                                                                                      |
| searchable                     | Boolean                               | true                           | 默认true，表示此列数据可被查询                                                                            |
| searchFormatter                | Boolean                               | true                           | 默认true，可使用格式化的数据查询                                                                          |
| escape                         | Boolean                               | false                          | 是否转义HTML字符串                                                                                        |

* 表单搜索 $.table.search  
```html
<a onclick="$.table.search();">搜索</a>
```

* 表格数据导出 $.table.exportExcel
```html
<a onclick="$.table.exportExcel();">导出</a>
```

* 数据模板下载 $.table.importTemplate
```html
<a onclick="$.table.importTemplate();">下载模板</a>
```

* 表格数据导入 $.table.importExcel
```html
<a onclick="$.table.importExcel();">导入</a>
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

* 表格销毁 $.table.destroy
```html
<a onclick="$.table.destroy();">销毁</a>
```

* 表格数据刷新 $.table.refresh
```html
<a onclick="$.table.refresh();">刷新</a>
```

* 选择表格行具体列 $.table.selectColumns
```javascript
var loginName = $.table.selectColumns("loginName");
```

* 选择表格行首列 $.table.selectFirstColumns
```javascript
var firstColumn = $.table.selectFirstColumns();
```

* 显示表格特定的列 $.table.showColumn
```javascript
$.table.showColumn("userName");
```

* 隐藏表格特定的列 $.table.hideColumn
```javascript
$.table.hideColumn("userName");
```

* 序列号生成 $.table.serialNumber    
```javascript
{
	title: "序号",
	formatter: function (value, row, index) {
		return $.table.serialNumber(index);
	}
},
```

* 超出指定长度浮动提示 $.table.tooltip    
```javascript
{
	field: 'remark',
	title: '备注',
	align: 'center',
	formatter: function(value, row, index) {
		return $.table.tooltip(value);
	}
},
```

* 回显数据字典 $.table.selectDictLabel    
```javascript
var datas = [[${@dict.getType('sys_common_status')}]];
{
	field: 'status',
	title: '用户状态',
	align: 'center',
	formatter: function(value, row, index) {
		return $.table.selectDictLabel(datas, value);
	}
},
```

* 下拉按钮切换 $.table.dropdownToggle    
```javascript
formatter: function(value, row, index) {
	var actions = [];
	actions.push('<a class="' + editFlag + '" href="#" onclick="$.operate.edit(\'' + row.deptId + '\')"><i class="fa fa-edit"></i>编辑</a>');
	actions.push('<a class="' + removeFlag + '" href="#" onclick="$.operate.remove(\'' + row.deptId + '\')"><i class="fa fa-trash"></i>删除</a>');
	actions.push('<a class="' + addFlag + '" href="#" onclick="$.operate.add(\'' + row.deptId + '\')"><i class="fa fa-plus"></i>添加下级部门</a>');
	return $.table.dropdownToggle(actions.join(''));
}
```

* 图片预览 $.table.imageView    
```javascript
{
	field: 'avatar',
	title: '用户头像',
	formatter: function(value, row, index) {
		return $.table.imageView(value, '/profile/avatar');
	}
},
```


## **弹层使用**

弹层组件目前基于`layer`组件进行封装,提供了弹出、消息、提示、确认、遮罩处理等功能。  

* 提供成功、警告和错误等反馈信息
```javascript
$.modal.msg("默认反馈");
$.modal.msgError("错误反馈");
$.modal.msgSuccess("成功反馈");
$.modal.msgWarning("警告反馈");
```

* 提供成功、警告和错误等提示信息
```javascript
$.modal.alert("默认提示");
$.modal.alertError("错误提示");
$.modal.alertSuccess("成功提示");
$.modal.alertWarning("警告提示");
$.modal.confirm("确认信息", function() {});
```

* 提供弹出层信息
```javascript
// title 标题 url 请求链接 width 宽度 height 高度 options 选项
$.modal.open(title, url, width, height);
$.modal.openTab(title, url);
$.modal.openOptions(options);
$.modal.openFull(title, url, width, height);
$.modal.close();
$.modal.closeAll();
$.modal.reload();
```

* 提供遮罩层信息
```javascript
$.modal.loading("正在导出数据，请稍后...");
$.modal.closeLoading();
```


## **权限使用**

使用thymeleaf模板整合了shiro标签 - 界面可以直接使用。（此处简单介绍两个，更多请参考官方文档）  
```html
<a href="#" shiro:hasPermission="system:user:add">包含权限字符串才能看到</a>
<a href="#" shiro:hasRole="admin">管理员才能看到</a>
```

如果需要在JS中使用权限，使用封装方法
```html
var addFlag = [[${@permission.hasPermi('system:user:add')}]];
var removeFlag = [[${@permission.hasRole('admin')}]];
<a class="btn btn-success btn-xs ' + editFlag + '">包含权限字符串才能看到</a>
<a class="btn btn-danger btn-xs ' + removeFlag + '">管理员才能看到</a>
```


## **字典使用**

配置好相关的数据字典信息即可正常使用（系统管理-字典管理）

```html
<select name="status" th:with="type=${@dict.getType('sys_normal_disable')}">
  <option value="">所有</option>
  <option th:each="dict : ${type}" th:text="${dict.dictLabel}" th:value="${dict.dictValue}">
  </option>
</select>
```

如果在想Table表格数据使用字典，使用formatter格式化
```javascript
// 获取数据字典数据
var datas = [[${@dict.getType('sys_normal_disable')}]];

// 格式化数据字典
formatter: function(value, row, index) {
	return $.table.selectDictLabel(datas, value);
}
```


## **参数使用**

配置好相关的参数信息即可正常使用（系统管理-参数管理）

```html
<body th:classappend="${@config.getKey('sys.index.skinName')}">
```

如果需要在JS中使用参数，使用封装方法
```javascript
var skinName = [[${@config.getKey('sys.index.skinName')}]];
$("#id").val(skinName);
```
