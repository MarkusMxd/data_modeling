## 动态建模页面

**模型信息**：模型分类(modelCategory不能修改)、模型名称(menuName不能修改)、表类型(tableType主表、附表)，模型类型(type默认普通、树、引用树)、表名(tableName校验一下字母数字下划线，不能修改，不能为空)、表描述(tableComment)、表单风格(formStyle默认动态，一列、两列、三列、四列)、

**注**：

1.如果选择附表类型，模型类型只能为普通，同时选择附表类型后，选择关联主表类型（ref_menu_name）

2.如果选择引用树，需要选择引用树类型（tree_ref_menu_name），引用树关联字段（ref_field）默认为CAT_SMALL_ID



**字段信息**：

​	**数据库属性**：字段名称(columnName)、字段备注(columnComment)、字段长度(columnSize)、小数点(columnPointLength)、默认值(columnDefault)、字段类型(jdbcType)、允许空值(columnIsNull)

​	**页面属性**：字段名称(columnName)、字段备注(columnComment)、表单显示(fromShow)、列表显示(listShow)、列表宽度(listWidth)、是否排序(sortTable)、组件类型(fieldType)、定义枚举转换器(listEnumConverter)

​	**校验字段**：字段名称(columnName)、字段备注(columnComment)、验证规则(verifyType)、校验必填(required)、校验提示(promptMsg)

​	**索引**：索引名称(indexName)、索引栏位(indexFields)、索引类型(indexType)

​	**查询配置**：字段名称(columnName)、字段备注(columnComment)、是否查询(query)、控件类型(queryFieldType)(文本框、下拉框、时间控件)、关联类型(自定义枚举转换器、字典表、代码库)字段，默认值(queryDefault)、查询枚举转换器(queryEnumConverter)



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

默认是将普通模型的正式表字段填写到字段信息中（columnType=0，formalFlag=0）



同步数据库后模型分类、模型名称、表名不能修改



考虑附表字段怎么处理，太多了



### 数据库属性：

#### 1.获取内置字段

新增时调用获取内置字段接口，不用传type,获取的是普通模型的所有内置字段（正式表、pre表、dis表所有字段），其中formalFlag=1表示正式表的字段，需要默认填写到字段信息中，formalFlag=3表示是pre表或dis表特有的字段，不需要展示，只用来校验，用户新增字段的时候需要校验，防止填写的字段名和内置字段名重复，内置的字段名、字段备注、字段长度、小数点、默认值、字段类型不能改变

除了formalFlag之外 还有columnType这个字段，columnType=0表示内置的 columnType=1表示手动新增的

手动新增字段时formalFlag=0，columnType=1是需要传的

获取内置字段时type值的含义(1 普通 2树 3引用树)



#### 2.内置类型设置

字段类型： 只有varchar和numeric类型可以设置长度,只有numeric类型可以设置小数位数

```json
{
    "String":"varchar",
    "Long":"bigint",
    "Integer":"integer",
    "Date":"date",
    "Timestamp":"timestamp",
    "Text":"text",
    "Float":"float4",
    "Double":"float8",
    "BigDecimal":"numeric"
}
```

内置字段的字段类型不能修改，新增的可以修改，主键特殊一点，每个tab页里面的数据都不能编辑

integer 默认字段长度10  小数点0  默认不能修改大小及小数点

long 19,0  默认不能修改大小及小数点

double 10,0  double和float先这样弄吧，他两其实不能限制大小 不管了

float 10 ,0

bigDecimal 10 , 0   这个可以编辑大小和小数点

String 128,0 大小可以修改，默认不能修改小数点

Text 0,0 默认不能修改大小及小数点

Date 0,0 默认不能修改大小及小数点

Timestamp 0,0 默认不能修改大小及小数点

####  3.校验字段中验证规则

```java
ID_NUMBER("1", "身份证号"),
SOCIAL_CREDIT_CODE("2", "社会信用代码"),
UNIQUENESS_CHECK("3", "唯一性校验"),
REGULAR_EXPRESSION("4", "正则表达式"),
MAIL("5", "邮箱"),
PHONE_NUMBER("6", "手机号"),
DATABASE_VIEW("7", "自定义数据库视图"),
CUSTOMIZED_UNIQUE_CHECK("8", "自定义唯一校验"),
WEBSITE("9", "网址"),
LETTER("10", "字母"),
NUMBER("11", "数字"),
INTEGER("12", "整数"),
NOT_EMPTY("13", "非空"),
POSTAL_CODE("14", "邮政编码"),
AMOUNT("15", "金额");
```

原来的系统中在模型维护中只有前六个，第7个也有，但是没有写相关逻辑，我们这里也不做展示，但是位置留着，除了7之外的这些校验规则都作为下拉选择项



### 页面属性：

字段名称和字段备注都不能修改，包括手动新增的，要修改只能去数据库属性中修改，定义枚举转换器主要是用来前端列表中枚举值的展示，比如1是启用，我们根据定义的一种格式去转换对应的值，用json存储：

大概是这样存储，或者有其他办法也行，主要是前端使用

```json
{
    "枚举值":"显示名称"   或者  "显示名称":"枚举值"
}
```

```json
[
  {
    "name": "asdf",
    "value": "vfasd"
  },
  {
    "name": "asdv",
    "value": "dsaf"
  }
]
```



### 校验字段：

字段名称和字段备注都不能修改，包括手动新增的，要修改只能去数据库属性中修改，验证规则逻辑和模型维护中一致，但是我发现模型维护中还有部分问题没有完善



### 索引：

校验索引名称重复、索引名称、索引栏位、索引类型都是必填项

```json
{
    "comments": "主表测试1",
    "menuName": "主表测试1",
    "type": 1,
    "tableName": "STD_DY_ZHU",
    "tableType": 0,
    "formStyle": 0,
    "modelCategory": 0,
    "extend": {},
    "indexInfo":{
      "id": "新增不用传，修改的时候有的话就要传",
      "indexName": "",
      "indexFields": "",
      "indexType": 0
      },
    "generateColumns": []
}
```

### 查询配置：

字段名称和字段备注都不能修改，包括手动新增的，要修改只能去数据库属性中修改，查询配置里的组件类型选项和主数据查询配置中的一样：文本框、下拉框、时间控件   选择下拉框的时候可以选择关联类型：自定义枚举转换器(0)、字典表(1)、代码库(2) 

```java
queryCodeRef 0 自定义枚举转换器 1 字典表 2 代码库
queryCodeRefType 关联类型名称
queryCodeRefTypes 关联类型编码串
```





### 操作按钮：

1. 编辑：isInitialized=1时，模型分类、模型名称、表名、表类型、模型类型不能修改

2. 同步数据库：（syncStatus=0的时候显示，反之不显示） 

   > 弹框提示：参考jeecgBoot中的，一模一样    syncType =0普通同步   syncType =1强制同步

3. 下载代码 

4. 在线编译 

5. 初始化业务数据：（isInitialized=0的时候显示，反之不显示）

6. 同步配置项

7. 复制模型：参考jeecgBoot中的，点击复制模型后弹框，取出当前行中的模型名称、表名、表注释，在每个值的后面跟个后缀，例子：STD_DY_ZHU 变成STD_DY_ZHU_COPY    测试主表 变为 测试主表COPY

8. 移除模型：弹框提示标题：确认移除该模型吗？  提示信息：只删除配置信息，数据库物理表保留 

9. 删除模型：弹框提示标题：确认彻底删除该模型吗？  提示信息：删除配置信息的同时将删除数据库物理表 

10. 批量删除：参考jeecgBoot中的，一模一样，不理解cell我
