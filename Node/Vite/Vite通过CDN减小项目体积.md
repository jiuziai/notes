# Vite下使用CDN实例
> ### vite+vue3+view ui plus开发环境
***
- ## 安装插件
```bash
npm install vite-plugin-cdn-import
```
***
- ## vite.config.js
```js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import legacy from '@vitejs/plugin-legacy'
import { resolve } from 'path'
import { Plugin as importToCDN } from 'vite-plugin-cdn-import'

export default defineConfig({
  plugins: [
    vue(),
    legacy({
      targets: ['defaults', 'not IE 11'],
    }),
    // 只有打包CDN才生效,开发环境需本地安装
    importToCDN({
      modules: [
        {
          name: "vue",
          var: "Vue",
          path: "https://unpkg.com/vue@3.2.45"
        },
        {
          name: "view-ui-plus",
          var: "ViewUIPlus",
          path: "https://unpkg.com/view-ui-plus@1.3.1/dist/viewuiplus.min.js",
          css: "https://unpkg.com/view-ui-plus@1.3.1/dist/styles/viewuiplus.css",
        }
      ]
    })
  ],
  resolve: {
    alias: {
      '@': resolve(__dirname, './src/assets'),
    },
    extensions: ['.js', '.vue', '.json'] 
  },
  build: {
    rollupOptions: {
      input: {
        login: 'src/templates/accounts/login.html',
      },
    },
  },
})
```