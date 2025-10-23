1、SysProFormInputNumber：一个页面出现2个数字输入框，失焦才显示数字
Fix：后端返回的字段是string类型，要转成Number类型
2、SysModal内套SysProList：出现2个滚动条
Fix：业务代码要写{visible && <SysModal />}
