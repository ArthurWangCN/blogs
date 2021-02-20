---
title: Gulp 101
date: 2020-04-28 12:40:45
tags: [Node, Gulp]
categories: Node
---



>Gulp —— 基于node平台开发的前端构建工具

+ 将机械化操作缩写成任务，想要执行机械化操作时执行一个命令行命令任务就能自动执行
+ 用机器代替手工，提高开发效率

### Gulp能做什么

+ 项目上线，HTML、CSS、JS文件压缩合并
+ 语法转换（es6、less）
+ 公共文件抽离
+ 修改文件浏览器自动刷新

### Gulp使用

1. 使用 `npm install gulp` 下载 gulp库文件
2. 在项目根目录下建立 `gulpfile.js` 文件
3. 重构项目的文件夹结构，`src` 目录放置源代码文件，`dist` 目录放置构建后文件
4. 在 gulpfile.js 文件中编写任务
5. 在命令行工具中执行 gulp 任务

### Gulp中提供的方法

+ gulp.src()：获取任务要处理的文件
+ gulp.dest()：输出文件
+ gulp.task()：建立gulp任务
+ gulp.watch()：监控文件的变化

```javascript
const gulp = require('gulp')
// gulp.task建立任务
gulp.task('task1', () => {
    // 获取要处理的文件
    gulp.src('./src/css/base.css')
    	// 将处理后的文件输出到dist目录
    	.pipe(gulp.dest('./dist/css'))
})
```

安装 `gulp-cli` : `npm install gulp-cli -g`

命令行中通过 `gulp 任务名` 执行gulp任务 

### Gulp插件

+ gulp-htmlmin：html文件压缩
+ gulp-csso：压缩css
+ gulp-babel：JavaScript语法转化
+ gulp-less：less语法转化
+ gulp-uglify：压缩混淆JavaScript
+ gulp-file-include：公共文件包含
+ browsersync：浏览器实时同步

