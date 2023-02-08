# Vite下使用CDN实例
> ### vite+vue3 开发环境
***
- ## 安装插件
```bash
npm install vite-plugin-cdn-import
```
***
- ## vite.config.js
```js
import { defineConfig } from 'vite'
...
import { Plugin as importToCDN } from 'vite-plugin-cdn-import'

export default defineConfig({
  ...
  plugins: [
    ...
    // 只有打包CDN才生效,开发环境需本地安装
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