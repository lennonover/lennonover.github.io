---
title: 用Gulp优化项目
date: 2016-07-11 20:49:47
tags:
	- Gulp
---

# 需求 
公司的项目性能很差,由于刚来实习老大让我尝试优化下就当给我练练手

# 原因

发现这个项目是JSP写的动态网页并没有模块化预加载什么的单纯的引入JS/CSS，每次访问请求用太多时间

# 解决

想到就先合并请求，并压缩代码之类的 工具是基于流的Gulp
//然并没用过

## 首先安装Gulp

```plain
npm install -g gulp
```

## 安装相应的插件

为了解决插件和模块依赖被重复的引入进来我用了ulp-load-plugins
为了以后项目的升级我也进行了参数配置全局化在gulp.config.js
```plain
npm install gulp-minify-css gulp-uglify gulp-concat gulp-rename ulp-load-plugins through2 vinyl-source-stream gulp-marked --save-dev gulp
```

## 由于是jsp写的路径useref方法无法识别 

用到filesystem模块先读到index.jsp然后用正则替换路径
```plain
gulp.task('readBuffer', function() {
    return gulp.src('app/index.jsp')
    .pipe(modify(swapStuff))
    .pipe(modify(swapStuff_))
    .pipe(gulp.dest(config.build));

})
function modify(modifier) {
    return through2.obj(function(file, encoding, done) {
        var content = modifier(String(file.contents));
        file.contents = new Buffer(content);
        this.push(file);
        done();
    });
}
//正则
function swapStuff(data) {
    return data.replace(/\$\{pageContext\.request\.contextPath\s*\}/g, '..');
}
function swapStuff_(data) {
    return data.replace(/window\.ContextPath \= \"\.\.\/\"/, 'window.ContextPath = "${pageContext.request.contextPath}/"')
               .replace(/base\shref\=\"\.\.\/\"/,'base href="dist/"');
}
```

## 然后用useref方法读到所有内容，进行压缩合并等步骤

这里需要在index.jsp根据你的需求页面配置
```plain
<!-- build:css css/lib.css -->
<!-- endbuild -->
<!-- build:css css/app.css -->
<!-- endbuild -->
<!-- build:js js/lib.js -->
<!-- endbuild -->
<!-- build:js js/app.js -->
<!-- endbuild -->
```

```plain
gulp.task('optimize',['readBuffer'],function() {
   return gulp
            .src('dist/index.jsp')
            .pipe($.useref())  // 解析jsp中build，将里面引用到的文件合并传过来
            .pipe($.if('js/app.js', $.ngAnnotate()))
            .pipe($.if('js/*.js', $.uglify()))
            .pipe($.if('css/*.css', $.minifyCss()))
            .pipe($.if('!index.jsp', $.rev()))
            .pipe($.revReplace())
            .pipe(gulp.dest(config.build))
            .pipe($.rev.manifest())
            .pipe(gulp.dest('dist/rev/'));
});
```

## 给资源文件加时间戳

```plain
gulp.task('rev', ['optimize'],function() {
    return gulp.src(['dist/rev/*.json', 'dist/index.jsp'])  
        .pipe($.revCollector())                                   
        .pipe(gulp.dest(config.build));                     
});
```

## 由于所有图片都合并到一起、路径就会发生变化这里对路径做个统一

```plain
gulp.task('images', function() {
  return gulp
      .src(config.images)
      .pipe($.rename(function(path) {
        path.dirname = path.dirname.replace(/.*\\images(.*)/, 'images$1');
        path.dirname = path.dirname.replace(/.*\\img(.*)/, 'img$1');
      }))
    .pipe(gulp.dest(config.build + 'images/'));
});
```

## 最后调用任务

```plain
gulp.task('default', ['rev','images']);
```

## 结束

刚接触gulp目前只是实现了更能，代码还有很多不足和优化地方欢迎指正O(∩_∩)O哈哈~
[源码](https://github.com/lennonover/Gulp) 

### 注：任务依赖要处理好

