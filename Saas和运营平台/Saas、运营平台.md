#### 区别
- Saas：ERP 转 Saas
- 运营平台：专门管理saas里面的租户，给内部用的。
	- 工单：IFCA的客户提工单给IFCA的
- ERP：
	- 工单： 客户提的维修工单
##### 数据库
在ifca-马来pgsql环境下：
- saas：Saas-dev
- 运营平台：Operation-dev
##### 代码仓库
- 前端：http://123.249.96.106:32023/ERPSTD/platform62/saas-group/infra-operation-frontend
	- components文件夹 - 工单Com与OperationCom不能互相引用，避免后续需要抽包；
	- pages文件夹 - 工单、Opertion、Platform不能互相引用
	- <font color="red">📢</font>：ERP 与Saas是同一套代码，改了ERP要同步改Saas，改了Saas要同步改ERP，都要部署

- 后端：http://120.46.205.111:38080/view/ERP%206.2%20%E6%A0%87%E5%87%86%E7%89%88/job/saas62-dev-linux/
	- 写在Operation内
##### 部署
- 前端
- 后端
