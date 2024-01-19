代码提交校验，单独抽离开来，vue 和 react 都适用


配置 typeScript
1. 修改 tsconfig.node.json 文件
{
  "compilerOptions": {
    "target": "ESNext", // 将代码编译为最新版本的 JS
    "module": "ESNext", // 使用 ES Module 格式打包编译后的文件
    "moduleResolution": "node", // 使用 Node 的模块解析策略
    "lib": ["ESNext", "DOM", "DOM.Iterable"], // 引入 ES 最新特性和 DOM 接口的类型定义
    "skipLibCheck": true, // 跳过对 .d.ts 文件的类型检查
    "resolveJsonModule": true, // 允许引入 JSON 文件
    "isolatedModules": true, // 要求所有文件都是 ES Module 模块。
    "noEmit": true, // 不输出文件,即编译后不会生成任何js文件
    "jsx": "preserve", // 保留原始的 JSX 代码，不进行编译
    "strict": true, // 开启所有严格的类型检查
    "noUnusedLocals": true, //报告未使用的局部变量的错误
    "noUnusedParameters": true, //报告函数中未使用参数的错误
    "noFallthroughCasesInSwitch": true, //确保switch语句中的任何非空情况都包含
    "esModuleInterop": true, // 允许使用 import 引入使用 export = 导出的内容
    "allowJs": true, //允许使用js
    "baseUrl": ".", //查询的基础路径
    "paths": { "@/*": ["src/*"], "#/*": ["types/*"] } //路径映射,配合别名使用
  },
  //需要检测的文件
  "include": ["src/**/*.ts", "src/**/*.d.ts", "src/**/*.tsx", "src/**/*.vue"],
  "references": [{ "path": "./tsconfig.node.json" }] //为文件进行不同配置
}


2. 在 src 文件夹下新建 typings.d.ts 文件
//声明window上自定义属性，如事件总线
declare interface Window {
  eventBus: any;
}

//声明.vue文件
declare module "*.vue" {
  import { DefineComponent } from "vue";
  const component: DefineComponent<object, object, any>;
  export default component;
}


3. 配置路径别名
 pnpm install @types/node -D




配置 eslint
1. 安装 eslint
// eslint 安装
pnpm install eslint  -D 
// eslint vue插件安装
pnpm install eslint-plugin-vue -D
// eslint 识别 typescript 语法
pnpm install @typescript-eslint/parser -D
// eslint typescript 默认规则补充
pnpm install @typescript-eslint/eslint-plugin -D

// 批量安装
pnpm install eslint eslint-plugin-vue @typescript-eslint/parser @typescript-eslint/eslint-plugin  -D

2. 新建 .eslintrc 文件
{
  "env": {
    "browser": true,
    "node": true,
    "es2021": true
  },
  "parser": "vue-eslint-parser",
  "extends": [
    "eslint:recommended",
    "plugin:vue/vue3-recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:prettier/recommended",
    "eslint-config-prettier"
  ],
  "parserOptions": {
    "ecmaVersion": "latest",
    "parser": "@typescript-eslint/parser",
    "sourceType": "module",
    "ecmaFeatures": {
      "jsx": true
    }
  },
  "plugins": ["vue", "@typescript-eslint", "prettier"],
  "rules": {
    "vue/multi-word-component-names": "off", // 禁用vue文件强制多个单词命名
    "@typescript-eslint/no-explicit-any": ["off"], //允许使用any
    "@typescript-eslint/no-this-alias": [
      "error",
      {
        "allowedNames": ["that"] // this可用的局部变量名称
      }
    ],
    "@typescript-eslint/ban-ts-comment": "off", //允许使用@ts-ignore
    "@typescript-eslint/no-non-null-assertion": "off", //允许使用非空断言
    "no-console": [
      //提交时不允许有console.log
      "warn",
      {
        "allow": ["warn", "error"]
      }
    ],
    "no-debugger": "warn" //提交时不允许有debugger
  }
}


3. 添加 eslint 忽略文件和目录配置 .eslintignore
# eslint 忽略检查 (根据项目需要自行添加)
node_modules
dist

4. 在 package.json 中增加 eslint 格式化命令
{
  "name": "vite-work",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vue-tsc && vite build",
    "preview": "vite preview",
    "lint": "eslint src --fix --ext .ts,.tsx,.vue,.js,.jsx"
  },
  "dependencies": {
    "vue": "^3.3.11"
  },
  "devDependencies": {
    "@types/node": "^20.10.5",
    "@typescript-eslint/eslint-plugin": "^6.15.0",
    "@typescript-eslint/parser": "^6.15.0",
    "@vitejs/plugin-vue": "^4.5.2",
    "eslint": "^8.56.0",
    "eslint-plugin-vue": "^9.19.2",
    "typescript": "^5.2.2",
    "vite": "^5.0.8",
    "vue-tsc": "^1.8.25"
  }
}

配置 prettier
1. 安装 prettier 依赖
// 安装 prettier 依赖
pnpm install prettier  -D  
//解决ESlint中的样式规范和prettier中样式规范的冲突，以prettier的样式规范为准，使用ESlint中的样式规范自动失效
pnpm install eslint-config-prettier -D  
// 将 prettier 做为 eslint 规范来使用
pnpm install eslint-plugin-prettier -D  
pnpm install vue-eslint-parser -D

// 批量安装
pnpm install prettier eslint-config-prettier eslint-plugin-prettier vue-eslint-parser -D

2. 新建配置 .prettierrc.json 文件
{  
  	"printWidth": 150, // 单行输出（不折行）的（最大）长度 
    "useTabs": false, // 不使用缩进符，而使用空格 
    "tabWidth": 2, // 每个缩进的空格数 
    "tabs": false, // 使用制表符 (tab) 缩进行而不是空格 (space) 
    "semi": false, // 是否在语句末尾打印分号
    "singleQuote": true, // 是否使用单引号
    "quoteProps": "as-needed", // 尽在需要时在对象属性周围添加引号 
    "trailingComma": "none", // 去除对象最末尾元素跟随的逗号
    "arrowParens": "always", // 箭头函数，只有一个参数的时候，也需要括号
    "rangeStart": 0, // 每个文件格式化的范围是文件的全部内容
    "proseWrap": "always", // 当超出print width（上面有这个参数）时就折行
    "endOfLine": "lf", // 换行符使用 lf 
    "bracketSpacing": true, // 是否在对象属性添加空格 
    "jsxBracketSameLine": true, // 将 > 多行 JSX 元素放在最后一行的末尾，而不是单独放在下一行（不适用于自闭元素）,默认false,这里选择>不另起一行 
    "htmlWhitespaceSensitivity": "ignore", // 指定 HTML 文件的全局空白区域敏感度, "ignore" - 空格被认为是不敏感的
    "jsxSingleQuote": false, // jsx 不使用单引号，而使用双引号  "rangeStart": 0, // 每个文件格式化的范围是文件的全部内容
}

3. 在 package.json 中增加 prettier 格式化命令
{
  "name": "vite-work",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vue-tsc && vite build",
    "preview": "vite preview",
    "lint": "eslint src --fix --ext .ts,.tsx,.vue,.js,.jsx",
    "prettier": "prettier --write ."
  },
  "dependencies": {
    "vue": "^3.3.11"
  },
  "devDependencies": {
    "@types/node": "^20.10.5",
    "@typescript-eslint/eslint-plugin": "^6.15.0",
    "@typescript-eslint/parser": "^6.15.0",
    "@vitejs/plugin-vue": "^4.5.2",
    "eslint": "^8.56.0",
    "eslint-config-prettier": "^9.1.0",
    "eslint-plugin-prettier": "^5.1.2",
    "eslint-plugin-vue": "^9.19.2",
    "prettier": "^3.1.1",
    "typescript": "^5.2.2",
    "vite": "^5.0.8",
    "vue-eslint-parser": "^9.3.2",
    "vue-tsc": "^1.8.25"
  }
}

4. 添加 prettier 忽略文件和目录配置 .prettierignore
# 忽略格式化文件 (根据项目需要自行添加)
node_modules
dist

如见代码标红报错执行：
● pnpm lint
● pnpm prettier
vscode 使用插件 volar 校验代码

配置 stylelint
1. 安装依赖
// 安装 stylelint 核心库
pnpm install stylelint -D

// stylelint 标准共享配置
pnpm install stylelint-config-standard -D

// 提供优化样式顺序的配置
pnpm install stylelint-config-recess-order -D

// 共享 HTML (类似 HTML) 配置，捆绑 postcss-html 并对其进行配置
pnpm install stylelint-config-html -D

// 扩展 stylelint-config-recommended 共享配置并为 Vue 配置其规则
pnpm install stylelint-config-recommended-vue -D

// 扩展 stylelint-config-recommended 共享配置并为 SCSS 配置其规则
pnpm install stylelint-config-recommended-scss -D

// postcss 的 scss 解析器
pnpm install postcss -D

// 解析 HTML (类似 HTML) 的 postcss 语法
pnpm install postcss-html -D

// postcss 的 scss 解析器
pnpm install postcss-scss -D

// 安装 scss 依赖
pnpm i sass -D 

// 批量安装
pnpm install scss stylelint stylelint-config-standard stylelint-config-recommended-scss stylelint-config-recommended-vue postcss postcss-html postcss-scss stylelint-config-recess-order stylelint-config-html -D

2. 根目录新建 .stylelintrc.cjs 文件，配置如下：
module.exports = {
  // 继承推荐规范配置
  extends: [
    "stylelint-config-standard",
    "stylelint-config-recommended-scss",
    "stylelint-config-recommended-vue/scss",
    "stylelint-config-html/vue",
    "stylelint-config-recess-order",
  ],
  // 指定不同文件对应的解析器
  overrides: [
    {
      files: ["**/*.{vue,html}"],
      customSyntax: "postcss-html",
    },
    {
      files: ["**/*.{css,scss}"],
      customSyntax: "postcss-scss",
    },
  ],
  // 自定义规则
  rules: {
    "import-notation": "string", // 指定导入CSS文件的方式("string"|"url")
    "selector-class-pattern": null, // 选择器类名命名规则
    "custom-property-pattern": null, // 自定义属性命名规则
    "keyframes-name-pattern": null, // 动画帧节点样式命名规则
    "no-descending-specificity": null, // 允许无降序特异性
    // 允许 global 、export 、deep伪类
    "selector-pseudo-class-no-unknown": [
      true,
      {
        ignorePseudoClasses: ["global", "export", "deep"],
      },
    ],
    // 允许未知属性
    "property-no-unknown": [
      true,
      {
        ignoreProperties: ["menuBg", "menuText", "menuActiveText"],
      },
    ],
  },
};

3. 配置 vite.config.ts
css: {
  // CSS 预处理器
  preprocessorOptions: {
    //define global scss variable
    scss: {
      javascriptEnabled: true,
        additionalData: `@use "@/styles/variables.scss" as *;`
    }
  }
}

配置 husky 
● husky：一个为 git 客户端增加 hook 的工具
● commitlint：让commit信息规范化，检查提交描述是否符合规范要求
● lint-staged：自动修复格式错误，仅对 git 代码暂存区文件进行处理，配合 husky 使用
● pre-commit：git hooks的钩子，在代码提交前检查代码是否符合规范，不符合规范将不可被提交
● commit-msg：git hooks的钩子，在代码提交前检查commit信息是否符合规范

提交校验工作流程分析：
1. 利用 commitizen 提供的 git cz 指令代替 git commit 指令，调出由 cz-customizable 提供的自定义界面供开发者按照规范选择对应的操作，并且输入一些描述
2. 再使用 @commitlint/cli 与 @commitlint/config-conventional 对提交的描述进行检查，如果失败则停止提交
3. 完成 git cz 之后，利用 husky 的钩子调用 lint-staged 配合 eslint 进行代码的检查和修复，最后 git push完成整个 git 操作

1. 安装依赖
// 安装 husky 依赖
pnpm install husky -D
// 安装 commitlint 对提交的描述进行检查
pnpm install @commitlint/config-conventional @commitlint/cli -D
// 安装 lint-staged 依赖
pnpm install lint-staged -D

// 批量安装
pnpm install husky @commitlint/config-conventional @commitlint/cli lint-staged -D

2. 在控制台输入命令，生成 .husky 文件夹
// 创建git仓库
git init

//生成 .husky 的文件夹
npx husky install

// 添加 hooks，会在 .husky 目录下生成一个 pre-commit 脚本文件
npx husky add .husky/pre-commit "npx --no-install lint-staged"

// 添加 commit-msg
npx husky add .husky/commit-msg 'npx --no-install commitlint -e'


3. commit-msg 规范化提交信息，修改 pre-commit 文件校验代码
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

echo "ts 类型检查中..."
npx tsc  
echo "ts 类型检查完成"
npx --no-install lint-staged --quiet

4. 修改 package.json
"lint-staged": {
  "*.{js,ts}": [
    "eslint --fix",
    "prettier --write"
  ],
    "*.{cjs,json}": [
    "prettier --write"
  ],
    "*.{vue,html}": [
    "eslint --fix",
    "prettier --write",
    "stylelint --fix"
  ],
    "*.{scss,css}": [
    "stylelint --fix",
    "prettier --write"
  ],
    "*.md": [
    "prettier --write"
  ]
}


配置 commitizen 
● commitizen：基于Node.js的 git commit 命令行工具，辅助生成标准化规范化的 commit message
● cz-git：一款工程性更强，轻量级，高度自定义，标准输出格式的 commitizen 适配器

1. 安装依赖
// 网上很多文章都需要安装多余的插件，其实 husky 8+ 只需要配合 commitizen 就可以实现
// 安装自定义 commitizen cz-git 的交互界面信息
pnpm install commitizen cz-git -D 

2. 配置 package.json
"config": {
  "commitizen": {
    "path": "node_modules/cz-git"
  }
}

3. 在根目录创建 commitlint.config.cjs
commitlint 对应配置Api：https://cz-git.qbb.sh/zh/config/
module.exports = {
  // 继承的规则
  extends: ['@commitlint/config-conventional'],
  // 自定义规则
  rules: {
    // @see https://commitlint.js.org/#/reference-rules

    // 提交类型枚举，git提交type必须是以下类型
    'type-enum': [
      2,
      'always',
      [
        'feat', // 新增功能
        'fix', // 修复缺陷
        'docs', // 文档变更
        'style', // 代码格式（不影响功能，例如空格、分号等格式修正）
        'refactor', // 代码重构（不包括 bug 修复、功能新增）
        'perf', // 性能优化
        'test', // 添加疏漏测试或已有测试改动
        'build', // 构建流程、外部依赖变更（如升级 npm 包、修改 webpack 配置等）
        'ci', // 修改 CI 配置、脚本
        'revert', // 回滚 commit
        'chore' // 对构建过程或辅助工具和库的更改（不影响源文件、测试用例）
      ]
    ],
    'subject-case': [0] // subject大小写不做校验
  },

  prompt: {
    messages: {
      type: '选择你要提交的类型 :',
      scope: '选择一个提交范围（可选）:',
      customScope: '请输入自定义的提交范围 :',
      subject: '请输入提交内容的变更描述 :',
      body: '填写详细的变更描述',
      breaking: '列举非兼容性重大的变更（可选）。使用 "|" 换行 :\n',
      footerPrefixesSelect: '选择关联issue前缀（可选）:',
      customFooterPrefix: '输入自定义issue前缀 :',
      footer: '列举关联issue (可选) 例如: #31, #I3244 :\n',
      generatingByAI: '正在通过 AI 生成你的提交简短描述...',
      generatedSelectByAI: '选择一个 AI 生成的简短描述:',
      confirmCommit: '是否提交或修改commit ?'
    },
    // prettier-ignore
    types: [
      { value: "feat",     name: "特性: feat 新增功能" },
      { value: "fix",      name: "修复: fix 修复缺陷" },
      { value: "docs",     name: "文档: docs 文档变更" },
      { value: "style",    name: "格式: style 代码格式" },
      { value: "refactor", name: "重构: refactor 代码重构" },
      { value: "perf",     name: "性能: perf 性能优化" },
      { value: "test",     name: "测试: test 添加疏漏测试或已有测试改动" },
      { value: "build",    name: "构建: build 构建流程、外部依赖变更" },
      { value: "ci",       name: "集成: ci 修改 CI 配置、脚本" },
      { value: "revert",   name: "回退: revert 回滚 commit" },
      { value: "chore",    name: "其他: chore 对构建过程或辅助工具和库的更改" },
    ],
    useEmoji: true,
    emojiAlign: 'center',
    useAI: false,
    aiNumber: 1,
    themeColorCode: '',
    scopes: [],
    allowCustomScopes: true,
    allowEmptyScopes: true,
    customScopesAlign: 'bottom',
    customScopesAlias: 'custom',
    emptyScopesAlias: 'empty',
    upperCaseSubject: false,
    markBreakingChangeMode: false,
    allowBreakingChanges: ['feat', 'fix'],
    breaklineNumber: 100,
    breaklineChar: '|',
    skipQuestions: ['body', 'breaking', 'footer'],
    issuePrefixes: [{ value: 'closed', name: 'closed:   ISSUES has been processed' }],
    customIssuePrefixAlign: 'top',
    emptyIssuePrefixAlias: 'skip',
    customIssuePrefixAlias: 'custom',
    allowCustomIssuePrefix: false,
    allowEmptyIssuePrefix: false,
    confirmColorize: true,
    maxHeaderLength: Infinity,
    maxSubjectLength: Infinity,
    minSubjectLength: 0,
    scopeOverrides: undefined,
    defaultBody: '',
    defaultIssues: '',
    defaultScope: '',
    defaultSubject: ''
  }
}


