## 创建项目

输入 `vue create ts-demo`

接下来会出现

```
? Please pick a preset:(使用上下箭头)
 ◯ default (babel, eslint)        //默认配置
❯◉ Manually select features       //手动选择===>这里我们选择手动选择
```

```
 // 选择好相应的包
? Check the features needed for your project:
 ◉ Babel                                    // javascript转译器
 ◉ TypeScript                               // 使用 TypeScript 书写源码
 ◯ Progressive Web App (PWA) Support        // 渐进式WEB应用
 ◉ Router                                   // 使用vue-router
 ◉ Vuex                                     // 使用vuex
 ◉ CSS Pre-processors                       // 使用css预处理器
❯◉ Linter / Formatter                       // 代码规范标准
 ◯ Unit Testing                             // 单元测试
 ◯ E2E Testing                              // e2e测试

// 是否使用class风格的组件语法： 使用前：home = new Vue()创建vue实例 使用后：class home extends Vue{}
? Use class-style component syntax? (Y/n) Y

// 使用Babel与TypeScript一起用于自动检测的填充
? Use Babel alongside TypeScript (required for modern mode, auto-detected polyfills, transpiling JSX)? (Y/n) Y

// 路由
? Use history mode for router? (Requires proper server setup for index fallback in production) (Y/n) Y

// 预处理器
? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default): (Use arrow keys)
 ◯Sass/SCSS (with dart-sass)    // 保存后编译
 ◯ Sass/SCSS (with node-sass)    // 实时编译 
❯◉ Less
 ◯ Stylus

// 代码格式化检测
? Pick a linter / formatter config: (Use arrow keys)
 ◯ ESLint with error prevention only     // 只进行报错提醒
 ◯ ESLint + Airbnb config                // 不严谨模式
 ◯ ESLint + Standard config              // 正常模式
 ◯ ESLint + Prettier                     // 严格模式
❯◉ TSLint(deprecated)                    // typescript格式验证工具

// 代码检查方式
? Pick additional lint features: (Press <space> to select, <a>
to toggle all, <i> to invert selection)
❯◉ Lint on save             // 保存检查
 ◯ Lint and fix on commit   // commit时fix

// 文件配置
? Where do you prefer placing config for Babel, ESLint, etc.? (
Use arrow keys)
  In dedicated config files // 配置在独立的文件中
❯ In package.json
  
// 保存上述配置，保存后下一次可直接根据上述配置生成项目
? Save this as a preset for future projects? (y/N) N

// 创建成功
🎉  Successfully created project ts-demo.
👉  Get started with the following commands:

 $ cd ts-demo
 $ yarn serve
```

`yarn serve`运行项目之后会报一堆莫名的错误，这都是 `tslint.json` 搞的鬼，配置一下重新运行即可

```json
// tslint.json 改动后

{
  "defaultSeverity": "warning",
  "extends": [
    "tslint:recommended"
  ],
  "linterOptions": {
    "exclude": [
      "node_modules/**"
    ]
  },
  "rules": {
    "indent": [true, "spaces", 2],
    "interface-name": false,
    "no-consecutive-blank-lines": false,
    "object-literal-sort-keys": false,
    "ordered-imports": false,
    "quotemark": [true, "single"],
    "no-console":false, // 不检测concole
    "semicolon": [ // 去除行尾必加';'
      false,
      "always"
    ],
    // 禁止自动检测末尾行必须使用逗号，always总是检测，never从不检测，ignore忽略检测
    "trailing-comma": [true,{ 
      "singleline": "never",
        "multiline": {
            "objects": "ignore",
            "arrays": "ignore",
            "functions": "never",
            "typeLiterals": "ignore"
        }
      }
    ]
  }
}

```

改完后重新运行`yarn serve`,至此，项目正常运行起来了。接下来我们改造目录结构。

```
├── public                          // 静态页面
├── scripts                         // 相关脚本配置
├── src                             // 主目录
    ├── assets                      // 静态资源
    ├── api                         // axios封装
    ├── filters                     // 过滤
    ├── lib                         // 全局插件
    ├── router                      // 路由配置
    ├── store                       // vuex 配置
    ├── styles                      // 样式
    ├── types                       // 全局注入
    ├── utils                       // 工具方法(全局方法等)
    ├── views                       // 页面
    ├── App.vue                     // 页面主入口
    ├── main.ts                     // 脚本主入口
    ├── registerServiceWorker.ts    // PWA 配置
├── tests                           // 测试用例
├── .editorconfig                   // 编辑相关配置
├── .npmrc                          // npm 源配置
├── .postcssrc.js                   // postcss 配置
├── babel.config.js                 // preset 记录
├── cypress.json                    // e2e plugins
├── f2eci.json                      // 部署相关配置
├── package.json                    // 依赖
├── README.md                       // 项目 readme
├── tsconfig.json                   // ts 配置
├── tslint.json                     // tslint 配置
└── vue.config.js                   // webpack 配置
```

主要涉及 `shims-tsx.d.ts` 和 `shims-vue.d.ts` 两个文件

- `shims-tsx.d.ts`，允许你以 `.tsx` 结尾的文件，在 `Vue` 项目中编写 `jsx`代码
- `shims-vue.d.ts` 主要用于 `TypeScript` 识别 `.vue`文件， ts 默认并不支持导入 `.vue`文件，这个文件告诉 ts 导入`.vue` 文件都按 `VueConstructor` 处理。
