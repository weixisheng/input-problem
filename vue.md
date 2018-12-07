### Vue使用总结

- 全局注册组件
```javascript
  import upperFirst from 'lodash/upperFirst'
  import camelCase from 'lodash/camelCase'
  // require.context 第二个参数是否查询其子目录
  const requireCom = require.context('./components', false, /\w+\.(vue)$/)
  requireCom.keys().forEach(fileName => {
    const comConfig = requireCom(fileName)
    const comName = upperFirst(
      camelCase(
        // 剥去文件名开头的 `./` 和结尾的扩展名
        fileName.replace(/^\.\/(.*)\.\w+$/, '$1')
      )
    )
    Vue.component(comName, comConfig.default || comConfig)
  })
```
- 生成iconfont样式类名 
```javascript
  // node generateCSS.js
  let fs = require('fs')
  let path = require('path')
  let rootPath = path.resolve(__dirname, './src/assets/iconfont/iconfont.css')

  fs.readFile(`${rootPath}`, 'utf-8', (err, data) => {
    if (err) throw err
    let d = data.match(/(icon-[^.]+)(?=:\w+)/gm)
    let res = JSON.stringify({'icon': d}, null, 4)
    fs.writeFile(
      path.resolve(__dirname, './src/assets/iconfont/iconfont.json'),
      res,
      err => {
        if (err) throw err
        console.log('成功')
      }
    )
  })
```
