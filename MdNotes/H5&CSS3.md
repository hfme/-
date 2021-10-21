

# CSS3





## 选择器



## 盒模型



## 背景和边框

### 边框：

- border-radius: 设置圆角（可以设置边框圆角、背景圆角、背景图片圆角）
- box-shadow
- border-image: 设置边框图片
  - border-image: urlsource图片地址 slice图像边界向内偏移 width宽度 outset repeat/stretch/round

### 背景：

- background-image
- background-size
- background-origin 背景图像的位置区域
- background-clip 背景剪裁属性，从指定位置开始绘制背景
  - content-box
  - padding-box
  - border-box

### 渐变

线性渐变

linear gradients 向下/向上/向左/向右/对角方向

径向渐变

radial gradients 由它们的中心定义



语法

创建一个线性渐变，你必须至少定义两种颜色节点。颜色节点即你想要呈现平稳过渡的颜色。同时，你也可以设置一个起点和一个方向（或一个角度）

```css
background-image: linear-gradient(direction默认从上到下, color-stop1, color-stop2, ...);
```

```css
background-image: linear-gradient(to bottom right对角线方向, red, yellow); 
```

使用角度

```css
background-image: linear-gradient(angle, color-stop1, color-stop2);
```

<img src="/Users/neofei/Fei/Frontend/笔记/image-20210507091411951.png" alt="image-20210507091411951" style="zoom:50%;" />



## 文字特效

text-shadow

box-shadow

text-overflow

word-wrap

word-break



## 字体

### CSS3 @font-face 规则







## 2D/3D转换



## 动画



## 多列布局



## 用户界面





## 选择器



