#### 何时使用 
系统上很多地方只有值，但是不知道它表达什么字段。因此提供一个```文字提示控件SysToolTip```，它有两种格式：

#### 代码演示
##### 1、悬浮的文案是普通多语言
![](Pasted%20image%2020250313114756.png)
传递locale属性作为多语言的key
```tsx
<SysTooltip locale="Contract:LeaseContract.ContractTerm">
	<span style={{ marginRight: '4px' }}>
	{translateMessage({
	  id: 'Common:To',
	  values: {
		from: formatDate('2025-03-13'),
		to: formatDate('2025-04-13'),
	  },
	  description: '{from} to {to},{from} 至 {to}',
	})}
	</span>
</SysTooltip>
```
 
 ##### 2、悬浮的文案是需要带参数的多语言
 ![](Pasted%20image%2020250313135653.png)
 传递locale属性作为多语言的key，同时传递localeValue作为多语言的value的值，但是需要注意：在json文件中定义的多语言key变量名必须是{0}
```tsx
<SysTooltip locale="Contract:LeaseContract.WhenExpire" localeValue="2">
  <span>7</span>
</SysTooltip>
```

#### API
