# Vite配置CDN加载插件
> vite-plugin-cdn-import 可在build时将import包转换为CDN加载，从而大幅度减小项目体积
***
---
## 安装插
件
```bash
npm install vite-plugin-cdn-import
```
---
## vite.config.js
```js
import { defineConfig } from 'vite'
...
import { Plugin as importToCDN } from 'vite-plugin-cdn-import'

export default defineConfig({
  ...
  plugins: [
    ...
    // 此处配置import和CDN转换的配置信息
    importToCDN({
      modules: [
        {
          name: "vue",
          var: "Vue",
          path: "https://unpkg.com/vue@3.2.45"
        }
        ...
      ]
    })
  ],
  ...
})
```