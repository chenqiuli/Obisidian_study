- /business-frontend-codegen @docs/租户评估-租户评估-列表页面.html                                     新建路由，新建页面，给我实现这个前端页面，使用SysProList组件。原型图跟组件标准不一样，按照组件标准来做。页面上搜索是配置searchConfig，导出是配置exportConfig，排序是sortConfig，等级（API接口：getApiElevateTenantEvalConfigGradeDefGetList）、楼层（数据源是B1、L1）是filterConfig，全部那一条页签是业态数据（API接口：getApiGeneralBusinessFormatGetList），也是放在filterConfig。列表services是getApiElevateTenantEvalResultGetListByBatchBatchid。
- TODO：在测算菜单下面新建一个租户评估的菜单，挂上去。

- Git提交：chenqiuli分支【不要提交到dev】

- http://localhost:8001/#/forecast/tenantEval/1



##### 2026.04.23

给 AI 相关文档和原型，文档和原型放到一个固定的位置，告诉 AI 去读
让 AI 理解原型，其中原型中有些文字是原型说明，不用开发到正式的页面，给说明举例
先分析，找出它不理解的
让它来理解并给出思路
AI 结合文档和原型审查思路，人审查思路
从大框架到小来生成
前端多语言翻译，甄别用户输入的内容及真正的多语言

前端不局限于我们现在的风格，主要是字体大小的规范，主色调的规范。

附件自己对接，

design - 原型说明的Tag不需要展示在前端
React Docs-后端反馈给前端的文档，包含：总文档、每个模块的文档

AI

不用组件的两层意思：
1、UI不好看、不好用
2、没人能反抗杨总，打破这个，前端页面慢慢换掉



顺序：
配置中心 

田总AI使用经验：
先总体思考一次，给出开发计划，每次完成一个开发计划，单开一个会话，就对这个任务进行复盘，复盘后生成到md文档放到audit文件夹下（大概应该是有已完成、未完成的功能清单），这里md文档太多，最后让它再整理一下变成一个总的文档




代码：High
上下文： 1M

响应式开发
头部使用SysBaseHeader组件
表格的按钮参照6.2三个点的样式

1、形成一个开发计划到md文件
2



**我的使用步骤**：
- 我现在要做一个



- CLAUDE.md：每次 Claude犯错，就加一条规则