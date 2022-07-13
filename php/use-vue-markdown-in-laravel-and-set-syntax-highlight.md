## 在 laravel 中使用 vue-markdown 并设置语法高亮

> 可以作为 laravel 使用 vue 的教程, 本文使用 `vue-markdown` 和 `highlight.js`

### 准备工作

- 安装 `yarn` 或 `npm/cnpm`, 本文使用 `yarn`

- 一个 laravel 项目, 本文使用 `laravel 5.8`

- [vue-markdown 官方文档](https://github.com/miaolz123/vue-markdown)

- [highlight.js 官方文档](https://highlightjs.org/usage/)

### 安装

```bash
# 安装 vue-markdown
yarn add vue-markdown --save

# 安装 highlight.js
yarn add highlight.js --save

```

### 使用

- 在 `resources/js/app.js`, 这行代码以下 `window.Vue = require('vue');` 添加

```js
// 引入 vue-markdown 组件
Vue.component('vue-markdown', require('vue-markdown').default);

// 引入 highlight.js 组件及 GitHub 样式
import hljs from 'highlight.js'
import 'highlight.js/styles/github.css'
Vue.directive('highlight', (el) => {
  let blocks = el.querySelectorAll('pre code')
  blocks.forEach((block) => {
    hljs.highlightBlock(block)
  })
})
```

### 生成可用的 js 和 css 

- 在项目根目录执行

```bash
npm run prod
```

- 你可能会遇到以下错误, 解决方案: 进入 `./node_modules/vue-markdown/` 目录, 执行 `yarn`

```
ERROR in ./node_modules/vue-markdown/dist/vue-markdown.common.js
Module not found: Error: Can't resolve 'babel-runtime/core-js/get-iterator' in '/mnt/e/donews/project/laravel58/node_modules/vue-markdown/dist'
 @ ./node_modules/vue-markdown/dist/vue-markdown.common.js 9:27-72
 @ ./resources/js/app.js
 @ multi ./resources/js/app.js ./resources/sass/app.scss

ERROR in ./node_modules/vue-markdown/dist/vue-markdown.common.js
Module not found: Error: Can't resolve 'babel-runtime/core-js/object/keys' in '/mnt/e/donews/project/laravel58/node_modules/vue-markdown/dist'
 @ ./node_modules/vue-markdown/dist/vue-markdown.common.js 9:74-118
 @ ./resources/js/app.js
 @ multi ./resources/js/app.js ./resources/sass/app.scss
```

### 在页面中使用

```html
<!-- resources/views/home.blade.php -->

<!-- 其他内容 -->

@section('content')
<div class="container">
    <div class="row justify-content-center">
        <div class="col-md-12">
            <div class="card">
                <div class="card-header">Dashboard</div>
                <div class="card-body">
					<vue-markdown v-highlight>i am a ~~tast~~ **test**.</vue-markdown>
				</div>
            </div>
        </div>
    </div>
</div>
@endsection

<!-- 其他内容 -->
```