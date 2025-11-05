 SysToast 对Toast.show进行了封装，主要是想让展示的内容不截断展示
#### 何时使用
需要对用户的操作给予友好性的反馈时
#### 示例
```tsx
SysToast({
	content: msg,
	duration: 3000,
});
```

#### API
该API属性基于[antd-mobile的Toast组件的Toast.show属性](https://ant-design-mobile.antgroup.com/zh/components/toast#toastshow)]