# 和侄子今天做什么？

这是一个有趣的小应用，用来随机选择和侄子一起进行的活动。使用 Vue 3 + Vite 开发，界面简洁美观。

## 预览

在线预览：[和侄子今天做什么？](https://uncle.example.com)

> 提示：请将图片放在 `src/assets/images` 目录下，支持 JPG 和 PNG 格式。图片会按照文件名自动排序并生成活动名称。

预览效果：

![预览图](./preview.png)

## 功能特点

- 优雅的随机选择动画
- 渐进式减速效果
- 响应式设计，适配移动端
- 使用 LXGW WenKai 字体，优雅美观

## 技术栈

- Vue 3
- Vite
- CSS3 动画
- LXGW WenKai 字体

## 开发

```bash
# 安装依赖
npm install

# 启动开发服务器
npm run dev

# 构建生产版本
npm run build

# 预览生产版本
npm run preview
```

## 使用说明

1. 点击"开始选择"按钮，系统会随机选择一个活动
2. 动画会从快到慢，最终停在随机选中的活动上
3. 可以重复点击按钮进行新的选择

## 自定义

1. 添加新活动：将新的图片（JPG 或 PNG）放入 `src/assets/images` 目录
2. 修改背景：替换 `src/assets/nanke.jpg` 文件
3. 调整动画：可以在 `ImageSelector.vue` 中修改动画时长和效果
