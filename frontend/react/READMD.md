## REACT学习记录
### API
+ reactdom.createportal
  + 将子节点渲染到存在于父组件以外的 DOM 节点
  + 可获得上下文context
  + 仅能在组件中使用

### 常用技巧
+ useRef的作用
  + 获得定时器的id
  + 不引起re-render的变量应该用ref保存