## 动态建模页面

**模型信息**：模型分类(modelCategory不能修改)、模型名称(menuName不能修改)、表类型(tableType主表、附表)，模型类型(type默认普通、树、引用树)、表名(tableName校验一下字母数字下划线，不能修改，不能为空)、表描述(tableComment)、表单风格(formStyle默认动态，一列、两列、三列、四列)、

**注**：

1.如果选择附表类型，模型类型只能为普通，同时选择附表类型后，选择关联主表类型（ref_menu_name）

2.如果选择引用树，需要选择引用树类型（tree_ref_menu_name），引用树关联字段（ref_field）默认为CAT_SMALL_ID



**字段信息**：

​	**数据库属性**：字段名称(columnName)、字段备注(columnComment)、字段长度(columnSize)、小数点(columnPointLength)、默认值(columnDefault)、字段类型(jdbcType)、允许空值(columnIsNull)

​	**页面属性**：字段名称(columnName)、字段备注(columnComment)、表单显示(fromShow)、列表显示(listShow)、列表宽度(listWidth)、是否排序(sortTable)、组件类型(fieldType)、定义枚举转换器(listEnumConverter)

​	**校验字段**：字段名称(columnName)、字段备注(columnComment)、验证规则(verifyType)、校验必填(required)、校验提示(promptMsg)

​	**索引**：索引名称(idxName)、索引栏位(idxColumn)、索引类型(idxType)

​	**查询配置**：字段名称(columnName)、字段备注(columnComment)、是否查询(query)、控件类型(queryFieldType)(文本框、下拉框、时间控件)、关联类型(字典表、代码库)字段，默认值(queryDefault)、查询枚举转换器(queryEnumConverter)



**功能**：下载代码、在线编译、初始化业务数据、同步数据库

**添加逻辑：**模型初始化后也可以修改字段(同步物理表)、复制模型、移除模型(不删除物理表)、删除模型(删除模型相关数据和物理表)

考虑添加外键逻辑

**注**：修改模型后，同步字段处理逻辑考虑

考虑主表附表，附表修改后需要同步模型维护字段

表名、字段名自动大写

索引名称不能重复，自动小写

附表类型不需要初始化业务数据





列表展示字段：模型分类、模型名称、表类型、模型类型、表名、表描述、同步状态（syncStatus）、创建、更新时间、操作（后面具体说有哪些操作）



formalFlag是否是正式表字段(0否 1是 3 pre表和dis表必填字段)  columnType列类型(0默认列，1手工列)

formalFlag=3的这些字段仅仅是用来防止用户填写的字段名与内置字段重名，只做校验，不做展示

点击新增的时候调用**查询内置字段**接口，这个接口会将内置字段返回，包括正式表，pre表，dis表的字段，还有树模型、引用树模型特有的字段

默认是将普通模型的正式表字段填写到字段信息中（columnType=0，formalFlag=0）、



同步数据库后模型分类、模型名称、表名不能修改



考虑附表字段怎么处理，太多了





新增时调用获取内置字段接口，不用传type,获取的是普通模型的所有内置字段（正式表、pre表、dis表所有字段），其中formalFlag=1表示正式表的字段，需要默认填写到字段信息中，formalFlag=3表示是pre表或dis表特有的字段，不需要展示，只用来校验，用户新增字段的时候需要校验，防止填写的字段名和内置字段名重复，内置的字段名、字段备注、字段长度、小数点、默认值、字段类型不能改变

除了formalFlag之外 还有columnType这个字段，columnType=0表示内置的 columnType=1表示手动新增的

手动新增字段时formalFlag=0，columnType=1是需要传的

获取内置字段时type值的含义(1 普通 2树 3引用树)



字段类型：

```json
{
    "String":"varchar",
    ""
}
```

