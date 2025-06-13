##### 解决之前问题：
- 接口频繁请求，上一次被cancel：
![](Pasted%20image%2020250324140727.png)
- 缓存：搜索记录、滚动位置等
- 原来控件太乱，SysToolBar与SysList两个控件业务逻辑强关联，但被分成2个，业务代码需要额外维护多余变量去强耦在一起

##### 缓存实现机制：
1、列表页 → 详情页：记住搜索参数、列表数据、滚动位置，后退不请求列表接口，直接按照以往方式：history.push
2、列表页 → 编辑页：记住搜索参数，返回列表带上前一次搜索参数去请求列表接口。在编辑history.push前要reload?.()（目的是：假设先进的详情页后退再去编辑页，重刷接口）
3、列表页 → 新增页：记住搜索参数，返回列表带上前一次搜索参数去请求列表接口。组件内统一处理了，直接按照以往方式：history.push

##### 注意事项：
1、正常来说，列表控件只是在列表使用，但是有些业务场景，也把整个列表控件放到SysModal中使用，这个时候，以防列表的缓存信息与弹窗内的缓存信息互串，弹窗也不需要缓存，因此请在查看页内使用的任何SysModal内添加上clearProListCache=false，不然从详情页后退就会重刷接口了。如需要缓存：不传destroyOnClose不会销毁dom，传destroyOnClose=true，每次 （优惠减免页面/contract/billDiscount）（附件类型页面/platform/attachment-type/TenancyManagement）
2、如果你觉得你的编辑页，主列表的信息是不支持修改的，其实在编辑前你也可以不执行reload（附件类型页面/platform/attachment-type/TenancyManagement）
3、高级搜索内的一般都不需要配置filterOption，控件内会自动根据type改成对应的operator
- type = select/multiSelect：单选operator用=、多选operator用in
- type = dateRange：>= 、<=
- type = date：=
- type = text：=

4、部分service接口的params参数无Filter、Sort、Take、RequireTotalCount等参数，这个时候需对service的接口进行二次导出，如：
```tsx
const loadData = async () => {
	const res = await getApiAppAttachmentTypeTree({
		isFilterSystem: true,
		productCode,
		groupName,
	});
	return {
		data: res,
	};
};
service={loadData}
```
5、部分service接口除了传入常见的params[Filter、Sort、Take、RequireTotalCount等参数]，还有必传的params，这时候可以对service的接口进行二次导出，如：
```tsx
service={async (params) => {
	if (!versionNumber) {
		return {
			data: [],
		};
	}
	return await getApiOpenapiOpenApiManage({
		...params,
		versionNumber,
	});
}}
```
5、列表页如果有使用到SysProList组件，return出去的JSX如果不只有SysProList组件，根部请使用<></>或Fragment标签包裹，不然页面会不能滚动

##### Bug：
- 1、从详情页回来，不能加载更多数据（页签一搜索”1“，滚动一点距离，去到详情页，再回退，这个时候接口的skip会从0开始，不希望从0开始）
- 2、删除的时候：没有调用汇总接口 ✅ 
- 3、从详情页回来，下拉刷新，参数没带进去 ✅
- 4、列表接口死循环：① 第一个状态栏搜索一个“1”让总数变少点，点击进去详情页，回来加载更多之后切换状态 ✅
- 5、编辑、新增的时候，回退第一次接口没有拿到params ✅ （原因出在SysList的hasMore，跟第一点冲突）
- 6、新增有判断是否选择项目后再跳转，之前那一步reload要真的跳转才执行 ✅ （判断history真正改变了才执行onClickBubble的reload()）
- 7、filterConfig有initValue时，初始化没有传入接口✅
- 8、filterConfig内的配置不支持在filterOption更改入参配置✅
- 9、filterConfig内的select的value若是字符串拼接，入参接口没有使用in操作符✅
- 10、业务代码配置了toolBarConfig，点击onAction对导致列表接口、汇总接口重刷新 ✅（使用useMemo缓存SysProList，避免父组件渲染导致子组件重渲染）
- 11、beforeChange的参数没有进去fetchSummary接口
- 12、onChange重复执行，死循环✅（定义一个新的ref变量存储filterParams，初始化onChange，后续的每当有改变再调用onChange）引发新问题：页面显示“All Projects”，初始化不调用，因为业务页面没传statusConfig，导致prevFilterParamsRef.current与_filterParams相同了 ✅
- 13、性能优化接口慢：第一次防抖时间久，改变入参的时候就缩短防抖时间 ✅（定义一个新的ref变量为true，初始化设置为1400ms，第一次调用之后把ref改为false，防抖时间是一个变量）
- 14、[婷恩的合同预测](http://localhost:8000/#/forecast/projectBudget/budgeting/leaseContract/3a190214-d95a-b8cc-f9d9-e28557d3043f)，filterConfig的options存在联动，会导致form表单的数据变成undefined ✅ （useEffect判断是否有initValue的问题，去掉useEffect的依赖）
- 15、TODO：客户台账详情页进入各个模块公用列表页，看看能否改掉之前设计，改成类似弹窗那样，设置一个clearProListCache，传过去后，在组件内执行clearDataSourceCache() 【暂时解决】
- 16、出现两个滚动条：在列表页使用，要把outletDiv的overflow改为hidden。✅ 
	在SysModal中使用，要把SysModal的content的overflow改为hidden。（要写成{visible && <SysModal /> }）✅
- 17、状态规则优先级：activeKey > initValue > 所有tabs从左到右优先展示第一个有显示数字的第一个tabs，若都无展示所有tabs从左到右不显示数字的第一个tabs，若都无展示第一个。✅
- 18、无services的StatusFilter组件，设置了initValue后，Tabs不见了 ✅ 
- 19、StatusFilter的tabs数据是前端自定义的静态数据，且 projectConfig: { showEntityType: false}，单独使用SysToolBar2的时候会刷两次 ✅ （设置prevFilterParamsRef.current = _filterParams的判断条件添加 !summaryData）

##### 优化：
- 1、列表页①是SysProList，跳转进去的页面②还使用了SysProList，②初始化不会调用接口，看了是因为filterParamsCache不变。 【预期是能使用，目前方案是②使用SysToolBar2 + SysList，想到的是传一个key属性给SysProList组件，让它在有这个属性的时候就要调用】eg：http://localhost:8000/#/forecast/projectBudget
- 2、分开使用的时候，列表太长，滚动会有问题，我想想怎么优化 eg：http://localhost:8000/#/forecast/projectBudget
- 3、点击查看后退的时候，不能刷业务额外的接口【先用一个页面来玩，然后看别的怎么实现】
- 4、批量选择的数据不是普通列表，而是树形的列表 eg：/Users/cql/work/business-frontend/apps/mobile/src/pages/Patrolling/BasicSetting/PatrolLocations。我在控件内弄了个otherData

##### 特殊 讲：
- http://localhost:8000/#/asset/asset-management/asset-acquisition , IFCA Msc Bhd, Rejected, Re-Submit，statusConfig的activeKey
- http://localhost:8000/#/leads/prospectPool , statusConfig无services
- http://localhost:8000/#/contract/leaseContract ，切换Approved不止入参这个状态，还有recordStatus，汇总接口默认除了状态字段，其它全入参，但这里特殊recordStatus也不要，omitField
- http://localhost:8000/#/contract/leaseContract ，龙威广场，第一个页签没数据，高亮第二个页签


##### 业务需检查、调整：
1、调整在查看页内SysModal 的 clearProListCache要设置成false
2、以前所有的列表页：SysToolBar + SysList + SysFixedIcon，替换成SysProList。SysToolBar会移除
3、在弹窗SysModal内使用SysProList，都要加上enableCache={false}

<hr />

##### 控件文档
###### 一、[效果](http://localhost:8000/#/view/list)
![](Pasted%20image%2020250425112312.png)
###### 二、API
- SysProList属性：

| 属性                       | 说明                                                                            | 类型                                                        | 默认值  |     |
| ------------------------ | ----------------------------------------------------------------------------- | --------------------------------------------------------- | ---- | --- |
| organizationSelectConfig | 设置头部项目选择树                                                                     | OrganizationSelectConfigType                              | -    |     |
| searchConfig             | 设置搜索框                                                                         | SearchConfigType                                          | -    |     |
| filterConfig             | 设置高级搜索                                                                        | FilterConfigType                                          | -    |     |
| toolBarConfig            | 设置工具栏，如：导出等操作                                                                 | ToolBarConfigType                                         | -    |     |
| statusConfig             | 设置状态栏                                                                         | StatusConfigType                                          | -    |     |
| batchSelectConfig | 设置批量全选 | BatchSelectConfigType | - |
| policy                   | 新增按钮的操作授权，跟列表上编辑按钮返回的operations权限保持一致                                         | string                                                    | -    |     |
| onClickBubble            | 新增按钮点击时的回调                                                                    | () => void                                                | -    |     |
| service                  | 列表接口                                                                          | SysProListServiceType                                     | -    |     |
| filterOption             | 配置接口入参的格式，但高级搜索，搜索框，状态栏的字段不要重复配置                                              | FilterOptions                                             | -    |     |
| onLoadCallBack           | 列表请求后，父组件拿到的回调数据，用ref去接收，不使用state                                             | （value: T[]) => void                                      | -    |     |
| leftExtra                | 搜索框左侧区域                                                                       | React.ReactNode                                           | -    |     |
| rightExtra               | 工具栏右侧区域                                                                       | React.ReactNode                                           | -    |     |
| topExtra                 | 搜索框上面区域                                                                       | React.ReactNode                                           | -    |     |
| searchBottomExtra        | 搜索框下面区域                                                                       | React.ReactNode                                           | -    |     |                              
| isShowHeader             | 是否显示橙色背景头，用于在SysModal中使用                                                      | boolean                                                   | true |     |
| entityData               | 在SysModal中使用时，不显示橙色背景头，需要手动传入的项目信息。一般是在```SysOrganizationSelect.useWatch```获取 | EntityType                                                | -    |     |
| initParams               | 传入状态汇总和列表接口的默认参数，不受控                                                          | Record<string, any>                                       | -    |     |
| activeParams             | 传入状态汇总和列表接口的参数，受控                                                             | Record<string, any>                                       | -    |     |
| beforeChange             | 作接口入参前的一些修改，回调函数内拿到所有的入参对象                                                    | (params: Record<string, any>) => Record<string, any>      | -    |     |
| onChangeRaw              | 拿到入参的整个对象。一般用于：services需要自己组装入参，或单独使用SysToolBar控件                             | (params: Record<string, any>) => void                     | -    |     |
| onChange                 | 拿到入参的整个对象，已经是convertToParams之后的格式                                             | (params: CurrentParams) => void                           | -    |     |
| enableCache              | 是否开启缓存，如：搜索框参数、列表数据、高级搜索参数等，在SysModal中需关闭                                     | boolean                                                   | true |     |

- SysProListServiceType类型：
```bash
(
	params: SysListServiceParamsType,
	options?: Record<string, any>,
) => Promise<Partial<{ data?: T[] } & Record<string, any>>>;
```

- OrganizationSelectConfigType 属性：

| 属性 | 说明 | 类型| 默认值| 
| -- | -- | --| -- | 
| entityLevel | 限制组织树从上到下递增显示到哪些级别的数据，比如：Company，那么只显示Group、Company的数据，Project的数据就不显示了 | EntityLevel | - |
| selectEntityLevel | 限制组织树哪些级别是否可选中和勾选 | EntityLevel[] | - |
| entityLevelProperty | 设置传入接口的字段是projectId、companyId或groupId | string | - |
| cache | 组织树是否缓存，即会存入ifca_project_store中 | boolean | true|
| defaultValue | 给组织树默认值，需给id | string | - |

- SearchConfigType 属性：

| 属性 | 说明 | 类型| 默认值| 
| -- | -- | --| -- | 
| field | 需要模糊搜索的字段 | string[] | - |

- FilterConfigType 属性：

| 属性 | 说明 | 类型| 默认值| 
| -- | -- | --| -- | 
| options | 需要高级搜索的选项 | FilterOptionType[] | - | 

- FilterOptionType属性：

| 属性        | 说明                                              | 类型                                               | 默认值   |     |
| --------- | ----------------------------------------------- | ------------------------------------------------ | ----- | --- |
| label     | 标题                                              | React.ReactNode                                  | -     |     |
| value     | 数据源，当type = ‘select’,'multiSelect'才需要，可直接传入数据字典 | (Partial<Popup> & Option)[]                      | -     |     |
| field     | 后端字段                                            | string                                           | -     |     |
| type      | 渲染成什么控件，目前支持：日期区间，日期，单选标签，多选标签，文本框              | 'select','multiSelect','dateRange','text','date' | -     |     |
| initValue | 默认值                                             | InitValueType                                    | -     |     |
| withTime  | type = dateRange 是否携带00:00:00 至 23:59:59        | boolean                                          | false |     |
| onChange  | 内容变化时的回调                                        | OnChangeType                                     | -     |     |

- OnChangeType类型：
```bash
(values: (Partial<Popup> & Option) | (Date | null) | ([Date, Date] | null) | string) => void;
```

- ToolBarConfigType属性：

| 属性 | 说明 | 类型| 默认值| 
| -- | -- | --| -- | 
| actions | 悬浮按钮操作组|ButtonItem[]| - |
| onAction | 点击按钮时的回调 | (val: Action) => void | - |
|isVertical| 工具栏图标三个点是否垂直展示 | boolean | true | 

- StatusConfigType 属性：

| 属性 | 说明 | 类型| 默认值| 
| -- | -- | --| -- | 
| tabs | 数据源 | AboveNumTabsType[] | - | 
| direction | 文本与数字布局 | AboveNumDirectionType | ‘horizontal’|
| field | 后端字段| string | - |
| services | 汇总接口 | ServiceType | - | 
| initValue| 默认值 | string | - |
|activeKey | 受控值 | string| -|
| onChange| 状态变化时的回调 | (values: string) => void | - |
|omitField | 汇总接口请求时的忽略字段，默认是field，但不排除使用还需要忽略其它字段| string[] |- |

- BatchSelectConfigType类型：
```bash
(obj: Record<string, string | string[]>) => boolean
```

###### 三、注意事项
- 重启、删除、移入黑名单等改变列表条数或状态的，调完各自的接口都需要执行reload?.()，才会重刷列表接口
- 编辑前记得要先执行reload?.()
- 查看页内使用的任何SysModal内添加上clearProListCache=false，不然从详情页后退就会重刷接口了，也记不住滚动位置了
- 如有配置到toolBarConfig，toolBarConfig内的actions的onAction事件是条件渲染某个JSX元素的，必须使用useMemo来缓存SysProList，防止因为条件渲染导致业务父组件渲染，导致SysProList子组件也重渲染。重渲染之后导致列表接口、汇总接口都会刷新。
- 若SysProList渲染的children是一个子组件，且子组件内有usePopupList，必须要在父组件上调用再传给子组件，不能在子组件内直接使用usePopupList，会造成死循环。
- 页面如果是显示“All Projects”，路由表必须配置projectConfig : {showEntityType: false}

