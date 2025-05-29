##### 对操作结果的轻提示反馈语做单词断行
#### 示例
```tsx
SysToast({
	content: msg,
	duration: 3000,
});


Toast.show({
	icon: 'fail',
	content: (
		<SysToastContent>
			{translateMessage({
			id: 'ComponentPlatform:SysToolBar.PleaseSelectCompanyOrProject',
			description: '请选择公司或项目',
			})}
		</SysToastContent>
	),
});
```

