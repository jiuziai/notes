# Vite下使用CDN实例
> ### vite+vue3+view ui plus开发环境
***
- ## package.json 

```
{
  "name": "vite",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "devDependencies": {
    "@vitejs/plugin-legacy": "^3.0.1",
    "@vitejs/plugin-vue": "^4.0.0",
    "terser": "^5.16.1",
    "view-ui-plus": "^1.3.1",
    "vite": "^4.0.0",
    "vite-plugin-cdn-import": "^0.3.5",
    "vue": "^3.2.45"
  }
}
```
***
- ## vite.config.js
```
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