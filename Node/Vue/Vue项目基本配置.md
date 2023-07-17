# Vue项目基本配置
>Vue3 + Vite + Naive-UI 项目基本配置信息，包含自动CDN转换功能
 

---
## package.json  

因为使用CDN加速,所以全部包均可以放在devDependencies,build时无需打包  
  

```json
{
    "name": "vue-project",
    "private": true,
    "version": "0.0.0",
    "type": "module",
    "scripts": {
        "dev": "vite",
        "build": "vite build",
        "preview": "vite preview"
    },
    "devDependencies": {
        "@vitejs/plugin-legacy": "^4.0.1",
        "@vitejs/plugin-vue": "^4.0.0",
        "autoprefixer": "^10.4.13",
        "naive-ui": "^2.34.3",
        "path": "^0.12.7",
        "vite": "^4.1.0",
        "vite-plugin-cdn-import": "^0.3.5",
        "vue": "^3.2.45",
        "vue-router": "^4.1.6"
    }
}
```   

---
## vite.config.js配置  
  
  
```js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import legacy from '@vitejs/plugin-legacy'
import autoprefixer from 'autoprefixer'
import { resolve } from 'path'
import { Plugin as importToCDN } from 'vite-plugin-cdn-import'

export default defineConfig({
    plugins: [
        vue(),
        legacy({
            targets: ['defaults', 'not IE 11'],
        }),
        // 构建时，import自动转换为CDN加载
        importToCDN({
            modules: [
                {
                    name: "vue",
                    var: "Vue",
                    path: "https://unpkg.com/vue@3.2.45"
                },
                {
                    name: "vue-router",
                    var: "VueRouter",
                    path: "https://unpkg.com/vue-route@1.5.1"
                },
                {
                    name: "naive-ui",
                    var: "naive",
                    path: "https://unpkg.com/naive-ui@2.34.3"
                }
            ]
        })
    ],
    css: {
        postcss: {
            plugins: [
                autoprefixer
            ],
        }
    },
    resolve: {
        // 别名替换
        alias: [
            { find: /^@\//, replacement: `${resolve(__dirname, 'src')}/` },
            { find: /^~/, replacement: '' }
        ],
        extensions: ['.js', '.vue', '.json', '.css']
    },
    build: {
        // 输入文件
        rollupOptions: {
            input: {
                index: 'src/index.html',
            },
        },
        // 输出目录
        outDir: 'public',
    },
})
```  

---
## 初始化项目
```bash
mkdir vue-project
cd vue-project
mkdir src components assets router
touch package.json vite.config.js src/index.html
# 将上方的内容分别写入 package.json 和 vite.config.js
npm i
```