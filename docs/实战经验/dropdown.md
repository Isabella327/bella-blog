---
sidebar_position: 1
---

# 实现dropdown组件

最近在做 **tdesign** 的dropdown组件重构，原来的实现方式是获取当前活跃节点所有上级，全部push进col里，设计给的图纸是父子元素分开，我的方案是使用递归嵌套元素，利用css的position去做跟随。

- position问题 overflow  z-index display 
- 递归渲染树形结构
- 高阶函数HOC
-  ```React.cloneElement e:React.MouseEvent<HTMLDivElement, MouseEvent> useRipple(ref || dropdownItemRef);Pick<DropdownOption > omit(popupProps, 'onVisibleChange') React.FC [文档](https://juejin.cn/post/6968739546997456926)```
- rc-trigger
- 扁平化柯里化