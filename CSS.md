# css盒子模型

## 基本概念： 基本模型+IE模型

### 标准模型和IE模型的区别

- 两者的区别在于content的不同，IE盒模型的content包括border、padding
- IE模型：really height  = border+ padding + content

​	box-sizing: border-box 

- 标准模型：really height = content

​	box-sizing: content-box(默认)

### js如何获取盒模型对应的宽和高

- dom.style.width/height

  只能取内联样式的宽高（head style/link 不能用）

- Dom.currentStyle.width/height

  取渲染后的宽高（仅IE支持）

- Windows.getComputedStyle(dom).width/height

  取渲染后的宽高 兼容性更好

- Dom.getBoundingClientReact().widrh/height

  计算元素绝对位置 

## BFC--块级格式化上下文

​