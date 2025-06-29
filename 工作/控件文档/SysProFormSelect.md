#### 解决了什么问题？
- 在查看页和编辑页的时候先显示了guid，再变成label
- 在编辑页的时候，该guid已经被禁用或删除，还是直接显示了guid在界面上，并且在保存时，该无效guid需要业务进行额外校验判断是否可以保存进数据库
#### 何时使用
需要弹出一个下拉菜单给用户选择操作
#### 效果
![](Pasted%20image%2020250627140432.png)
![](Pasted%20image%2020250627140454.png)

#### 注意事项
- 在编辑页或查看页回显数据的时候，把从查看接口获取到的数据使用form.setFieldsValue都解构出来，因为displayName需要从中取值
- 标注必填时，只需要传```rules={[{ required: true }]}```，组件内在查看页去除必填标识
#### 代码演示
```tsx
useEffect(() => {
	form.setFieldsValue({
		...detailInfo，
		sourceCode: 'Leads.Prospect.Source.002',
		sourceName: 'Agency',
		expectedLocationCodes: [
			'Leads.Prospect.ExpectedLocation.001',
			'Leads.Prospect.ExpectedLocation.002',
		],
		expectedLocationNames: [
			'Near influencer hotspot', 'Near escalator'
		],		
		brandId: [
			'3a19fef6-52fb-07c2-7cd4-1eb95690eb6b', 
			'3a19b090-b611-fdcb-5752-21f2746ca92f'
		],
		brandName: ['TOMEI Gold & Jewellery', 'Jaya Grocer'],
	})
}, [detailInfo])
```
##### 1、单选模式-基本使用
```tsx
<SysProFormSelect
	label="单选下拉"
	name="sourceCode"
	rules={[{ required: true }]}
	fieldProps={{
		options: sourceCodeList,
		readOnly,
		displayName: 'souceName'
	}}
/>
```
##### 2、多选模式-基本使用
```tsx
<SysProFormSelect
	label="多选下拉"
	name="expectedLocationCodes"
	fieldProps={{
		options: expectedLocationCodesList,
		readOnly,
		mode: 'multiple',
		displayName: 'expectedLocationNames'
	}}
/>
```
##### 3、多选模式-全选
```tsx
<SysProFormSelect
	label="多选下拉"
	name="expectedLocationCodes"
	fieldProps={{
		options: expectedLocationCodesList,
		readOnly,
		mode: 'multiple',
		isCheckAll: true,
		displayName: 'expectedLocationNames'
	}}
/>
```
##### 4、多选模式-新增标签
```tsx
<SysProFormSelect
	label="多选下拉"
	name="brandId"
	fieldProps={{
		options: brandList,
		readOnly,
		mode: 'tags',
		isCheckAll: true,
		displayName: 'brandName'
	}}
/>
```
##### 5、联动模式
```tsx
<SysProFormSelect
	label="品牌"
	name="brandId1"
	fieldProps={{
		options: brandList,
		readOnly,
		displayName: 'brandName1',
		onChange: (value) => {
			if(value) {
				const brandItem = brandList.find(
					(item) => item.id === value,
				) as GeneralAPI.BrandListDto;
				form.setFieldsValue({
					leasingTradeId: brandItem.businessFormatId,
					leasingTradeName: brandItem.businessFormatName,
				});
			}else {
				form.setFieldsValue({
					leasingTradeId: undefined,
					leasingTradeName: undefined,
				});
			}
		},
	}}
/>
<SysProFormSelect
	label="业态"
	name="leasingTradeId"
	fieldProps={{
		options: [],
		readOnly,
		displayName: 'leasingTradeName',
	}}
/>
```
##### 6、自定义下拉选项
```tsx
<SysProFormSelect
	label="计费单位"
	name="priceUnit"
	fieldProps={{
		options: priceUnitModeList.map((item, index) => {
			return {
				...item,
				key: index,
				label: (
					<p>
						{`${item.label}`}
						<span style={{ fontSize: 'var(--adm-font-size-2)' }}>
							({translateMessage({id: PriceUnitModeEnum[item.value as PriceUnitMode]})})
						</span>
					</p>
				),
				desc: `${item.label}${translateMessage({
					id: PriceUnitModeEnum[item.value as PriceUnitMode],
				})}`,
			};
		}),
		readOnly,
		displayName: 'souceName'
	}}
/>
```

#### API
该API属性基于[antd-mobile的FormItem属性](https://ant-design-mobile.antgroup.com/zh/components/form#%E5%B1%9E%E6%80%A7-1)，再添加以下几个属性：

| 属性| 说明|类型|默认值|是否必传|
| -- | -- |--|--| -- | 
| fieldProps | [antd的Select组件的共同API属性](https://ant-design.antgroup.com/components/select-cn#api)，除了options、value和onChange | Omit<SelectProps, 'options' | 'value' | 'onChange'> | - | 是|

在以上fieldProps类型的基础上，再添加以下几个特有属性：

| 属性| 说明|类型|默认值|是否必传|
| -- | -- |--|--| -- | 
| value | 指定当前选中的条目，多选时为一个数组 | CheckListValue | 单选为undefined，多选为[] | 否|
| onChange | 选中的回调 | OnChangeType | - | 否 |
|options | 下拉数据源|SysSelectOptionsType | - | 是|
| readOnly |是否查看 | boolean | false | 否|
| footer | 下拉框自定义渲染底部 | (item: FooterValue) => React.ReactNode | - | 否| 
|footerCalssName | 底部类名 | string | - | 否 | 
| className | 类名 | string  | - | 否| 
| isCheckAll | 多选时是否全选 | boolean| false| 否| 
| dropdownRender  |对下拉菜单进行自由扩展 | (menu: React.ReactNode) => React.ReactElement | - | 否| 
| displayName | 编辑、查看展示的label字段，从查看接口获取而来 | string | - | 是| 
| form | SysForm组件上下文的表单实例|  FormInstance | - | 否| 
| omitDisplayName | 是否忽略displayName进行展示，用options进行查找【用于选项使用ReactNode的】 | boolean | - | 否| 


##### CheckListValue类型：
```ts
type CheckListValue = string | number | Array<string | number>;
```
##### OnChangeType类型：
```ts
type OnChangeType =（value: CheckListValue | undefined） => void;
```
##### SysSelectOptionsType类型
```ts
type SysSelectOptionsType = {
	label: React.ReactNode;
	value: string | number;
	disabled?: boolean;
	[propName: string]: any;
};
```
##### FooterValue类型
```ts
type FooterValue = {
	setVisible: (val: boolean) => void;
};
```


#### 在编辑下：
- 多选模式下，有一个value是有效的，有一个value是无效的，无效的value会直接显示guid，因为没法知道这个无效的guid的label究竟是什么




