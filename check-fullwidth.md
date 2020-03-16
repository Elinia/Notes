> https://www.npmjs.com/package/is-fullwidth-code-point

Install
```bash
$ npm install is-fullwidth-code-point
```

Usage
```javascript
const isFullwidthCodePoint = require('is-fullwidth-code-point');
 
isFullwidthCodePoint('谢'.codePointAt(0));
//=> true
 
isFullwidthCodePoint('a'.codePointAt(0));
//=> false
```
