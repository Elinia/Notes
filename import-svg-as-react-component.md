# React中如何使svg继承父组件颜色

1. 将svg文件内想要继承颜色的部分设置 `fill: currentColor` 
```html
<svg {...} xmlns="http://www.w3.org/2000/svg">
  <path {...} fill="currentColor"/>
</svg>
```
2. 将svg文件以React Component形式导入。参见：https://create-react-app.dev/docs/adding-images-fonts-and-files/#adding-svgs
```javascript
import { ReactComponent as Arrow } from 'client/images/arrow.svg'
```
3. 按需求设置父组件颜色
```html
<div style={{ color: getColor() }}>
  <Arrow />
</div>
```
