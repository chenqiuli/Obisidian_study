普通下拉控件
```tsx
<SysProFormSelect
label="单选下拉"
name="sourceCode"
fieldProps={{
	options: sourceCodeList,
	readOnly,
	displayName: 'souceName'
 }}
/>
```

#### 在编辑下：
- 多选模式下，有一个value是有效的，有一个value是无效的，无效的value会直接显示guid，因为没法知道这个无效的guid的label究竟是什么

#### 在查看下：
##### 单选：
- 不管value有没有在options中，都要传displayName【预期】，不然直接乱码



