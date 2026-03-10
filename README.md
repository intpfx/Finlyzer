# Finlyzer

Finlyzer 是一个本地优先的 Electron 桌面应用，用于整理个人或小型业务场景下的支付宝、微信和已支持银行账单导出数据。整个工作流围绕一张主交易表展开：导入、分类、检查镜像关系、复核承诺结转，并完成图表分析，全程不依赖远端服务器。

当前生产版本：`1.0.0`

## 当前能力

- 支持导入支付宝、微信 CSV 账单，以及当前已适配的银行账单工作簿。
- 导入时自动识别数据来源格式。
- 通过 Dexie 将交易、分类树、导入任务和 UI 元信息存储在本地 IndexedDB。
- 支持直接在主表内做方向感知的分类选择。
- 支持手动建立和解除镜像分组，避免重复现金流水在分析中被重复计入。
- 支持手动录入未来应付或应得的承诺记录，后续再与真实现金流水进行人工结转确认。
- 支持从金额单元格进入逐条金额均摊分析。
- 内置趋势图、分类分布图、统一日历热力图和分类 x 时间矩阵图。
- 支持 JSON 备份导出和完整恢复。

## 主要工作流

1. 导入一个或多个账单文件。
2. 优先处理主表顶部的未分类流水。
3. 在需要时检查镜像关系和承诺结转候选。
4. 通过图表工作台按日期、分类或账期聚焦主表。
5. 在本地数据稳定后导出备份。

## 存储模型

Finlyzer 采用本地优先设计。

- 数据保存在当前机器上。
- 应用通过 Dexie 管理 IndexedDB 表。
- 备份导出会写出当前数据模型的 JSON 快照。
- 备份恢复只接受当前 schema 版本导出的备份文件。

当前本地存储表包括：

- `transactions`
- `importJobs`
- `categoryTrees`
- `appMeta`

## 技术栈

- Electron
- React 19
- TypeScript
- Vite
- Tailwind CSS v4
- Dexie
- ECharts

## 开发

安装依赖：

```bash
pnpm install
```

启动渲染端和 Electron 壳：

```bash
pnpm dev
```

以开放远程调试端口的方式启动，便于 DevTools 或 MCP 调试：

```bash
pnpm dev:debug
```

执行 lint：

```bash
pnpm lint
```

构建渲染端：

```bash
pnpm build
```

基于最新本地构建运行桌面应用：

```bash
pnpm start
```

## 打包发布

生成 Windows portable 发布包：

```bash
pnpm dist
```

等价的显式命令：

```bash
pnpm dist:win
```

如果打包时需要 Electron 镜像源：

```bash
pnpm dist:win:mirror
```

打包成功后，最终产物会保留在 `release/` 目录中。

当前产物命名示例：

```text
release/Finlyzer-1.0.0-windows-portable.exe
```

## 仓库说明

- `.mcp-debug-shell/` 是临时本地调试壳输出目录，不应提交。
- `dist/` 构建产物和 `release/` 打包产物都应忽略。
- 仓库内已不再保留本地 `mcp-debug-shell` skill 副本，应使用个人全局 skill。

## 当前限制

- 目前不支持云同步和协作。
- 备份恢复严格依赖当前备份格式。
- 当数据量继续增大时，更可能先遇到查询和分页性能问题，而不是存储容量问题。
- 银行导入当前仅支持本仓库中已经验证过的工作簿布局，不支持任意银行格式。
