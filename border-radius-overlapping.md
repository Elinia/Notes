### CSS豆知识(1): border-radius overlapping

#### w3
> https://www.w3.org/TR/css-backgrounds-3/#corners
#### example
> https://codesandbox.io/s/border-radius-zwm7t?file=/src/styles.css

长懒看：
1. 一个div的border-radius由8个值构成，每个角两个，分别是椭圆弧(圆弧)横向轴和纵向轴的长度。
2. 当一条边对应的两个轴的长度和超过边本身的长度时，会对所有这8个值做等比缩小以满足四条边每条边对应的两个轴长和不大于边长。
