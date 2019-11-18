iOS Safari会把所有`font-size < 16px`的input在focus时进行自动放大的操作，导致为mobile设计的响应式布局变得很丑。
没有找到完美的解决方案，以下是几种work around。
#####1. 将 `font-size` 修改到 `16px` 或以上

```css
input, select, textarea {
	font-size: 16px;
}
```
#####2. 修改meta项禁用浏览器自动缩放

```html
<head>
	<meta
		  name="viewport"
		  content="width=device-width, initial-scale=1, shrink-to-fit=no, minimum-scale=1, maximum-scale=1, user-scalable=yes"
	/>
</head>
```
#####3.忍受缩放
