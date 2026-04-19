- /business-frontend-codegen @docs/租户评估-租户评估-列表页面.html                                     新建路由，新建页面，给我实现这个前端页面，使用SysProList组件。原型图跟组件标准不一样，按照组件标准来做。页面上搜索是配置searchConfig，导出是配置exportConfig，排序是sortConfig，等级（API接口：getApiElevateTenantEvalConfigGradeDefGetList）、楼层（数据源是B1、L1）是filterConfig，全部那一条页签是业态数据（API接口：getApiGeneralBusinessFormatGetList），也是放在filterConfig。列表services是getApiElevateTenantEvalResultGetListByBatchBatchid。
- TODO：在测算菜单下面新建一个租户评估的菜单，挂上去。

- Git提交：chenqiuli分支【不要提交到dev】

- http://localhost:8001/#/forecast/tenantEval/1
