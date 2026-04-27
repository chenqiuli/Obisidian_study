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



前端开发

给 AI 相关文档和原型，文档和原型放到一个固定的位置，告诉 AI 去读
让 AI 理解原型，其中原型中有些文字是原型说明，不用开发到正式的页面，给说明举例。
先分析，找出他不理解的

让他来理解并给出落地思路
AI 结合文档和原型审查，人审查思路
思路确定后，让他来制定开发落地的批次及计划

每一步落地前，先进行落地计划方案的制定以及需要拍板的事项，让我确认后再执行代码编写


进展到一定的程度，做一次已完成功能的复盘，形成清单

当前

从大框架到小来生成
前端多语言翻译，甄别内容及真正的多语言

前端不局限于我们现在的风格，主要是字体大小的规范，主色调的规范。

附件自己对接，


**我的使用步骤（正式版）**：
- AI：/init 更新一下CLAUDE.md文档
- AI：帮我生成租户评估的services接口，swagger地址是https://rentx-dev.ifca.cloud/b/elevate/swagger/TenantEval/swagger.json，projectName是forecastTenantEval
- 人：页面新建菜单，routes下新建路由，pages下新建TenantEval文件夹，在下面新建index.tsx。
- AI：@docs/TenantEval/design @"docs/TenantEval/React Docs/"  我要做一个租户评估，design文件夹是原型，React Docs文件夹包含一份总的前端开发指南以及细分到每个页面的前端开发指南。先别着急写代码，你先理解原型，输出一份需求文档到docs/TenantEval下面。
- AI：@docs/TenantEval/PRD_租户评估前端需求文档.md 文档语义压缩，太多了，看不过来。
- 看AI生成的这个文档，对照原型图，过程中一直在调整这个md文档，一边调整一边对着原型图理解，下面是我调整中提给AI的一些prompt，举个例子：
	- @"docs/TenantEval/React Docs/01_模板库_前端开发指南.md" 这个后端接口板块是根据后端代码生成的，优化：根据现在的前端代码生成的services，加多一个板块为前端接口，方便前端去寻找



**PRD疑问**：
- 3.1： 删除模板（仅 Draft）接口，在原型图找不到删除按钮（已补充到PRD，原型图未体现）✅
- 

**原型图疑问**：
- 规则中心详情页：
	- 命中动作没接口 新增编辑删除
	- 业务建议保存没接口
- 基础配置/战略矩阵配置：
	- 试跑和配置JSON看不懂



- 生成CLAUDE.md：
	- /init
	- 每次 Claude犯错，就加一条规则