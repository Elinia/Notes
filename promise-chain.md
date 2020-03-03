Promise链的处理结果：
```javascript
function then({ status, content }) {
  if (status == reject) return reject(content)
  
  const res = handle_then(content)
  if (res != Promise) return resolve(res)
  return res
}

function catch({ status, content }) {
  if (status == resolve) return resolve(content)
  
  const res = handle_catch(content)
  if (res != Promise) return resolve(res)
  return res
}

function finally(last) {
  const res = handle_finally()
  if (res != Promise || res.status == resolve) return last
  return reject(res.content)
}
```
