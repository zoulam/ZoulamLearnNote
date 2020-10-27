---
description: 响应式、diff
---

# \[vue\]原理

## 胡子语法

```javascript
<body>
    <div id="app">{{obj.key.car.color}} <span style="color: red;">{{obj.name}}</span> <strong>{{obj.key.name}}</strong></div>
    <script>
        let obj = {
            val: 15,
            name: 'zoulam',
            key: {
                name: 'lala',
                car: {
                    color: 'red'
                }
            }
        }
        let oApp = document.getElementById('app')
        let val = oApp.innerHTML

        function dep(arr) {
            let [, ...newArr] = arr
            return newArr.reduce((prev, cur) => {
                return prev[cur]
            }, obj)
        }

        function mustache(val) {
            let reg = /\{\{(.+?)\}\}/
            let ans = val.split(reg)
            let target = ans.filter(item => item.startsWith('obj.'))
            let node = target.map(element =>
                dep(element.split('.'))
            );
            let index = -1
            let res = val.replace(reg, (match, lastIndex, oldStr) => {
                index++
                return node[index]
            })
            return res
        }

        oApp.innerHTML = mustache(val)
    </script>
</body>
```

