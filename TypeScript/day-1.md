## 搭建开发环境

1. 全局安装 TypeScript

```bash
yarn global add typescript
```

2. 初始化项目

生成 `tsconfig.json`

```bash
tsc --init
```

初始化 npm 管理，在工作区安装 `typescript`

```bash
npm --init

yarn add typescript -D
```

3. 选择 `TypeScript` 版本

    * 打开 `VSCode`
    * 点击 `查看`
    * 选择 `命令面板`
    * 创建 `.ts` 文件
    * 输入 `typescript`
    * 选择 `选择 TypeScript 版本`
    * 选择 `使用工作区版本`

`VSCode` 默认使用自身内置的 TypeScript 语言服务版本，改为使用工作区的 typescript 版本，防止出现因为开发和构建环境版本不同，检测结果不一致的问题。
