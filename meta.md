# meta

MDN: https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meta

> HTML `<meta>` 元素表示那些不能由其它HTML元相关元素 (`<base>`, `<link>`, `<script>`, `<style>` 或 `<title>`) 之一表示的任何元数据信息.

用 `<meta>` 可以轻松解决一些仅用javascript实现很复杂的任务，如**禁用浏览器自动缩放**。

> iOS Safari会把所有font-size < 16px的input在focus时进行自动放大的操作，导致为mobile设计的响应式布局变得很丑。通过修改meta中的viewport项`maximum-scale=1, user-scalable=yes`可以轻松禁用浏览器自动缩放。

当遇到浏览器实现差异等问题时，可以考虑查一查是否有相应的meta项可以解决这类问题或缩小其带来的影响。
