---
title: sass中的@规则
date: 2017-07-01 17:54:55
tags:
    - Sass
categories: Sass
---
### scss @规则

> * @import


1. Sass 扩展了 CSS 的 @import 规则，让它能够引入 SCSS 和 Sass 文件。 所有引入的 SCSS 和 Sass 文件都会被合并并输出一个单一的 CSS 文件。
2. 引入多个文件 @import "rounded-corners", "text-shadow";
3. 嵌套引入

```css
//假设要引入的样式文件`example.scss`文件中包含这样的代码：
.example {
  color: red;
}
//然后这样引用：

#main {
  @import "example";
}
//编译出来的 CSS：
#main .example {
  color: red;
}
```

> * @media 指令和 CSS 的使用规则一样的简单，但它有另外一个功能，可以嵌套在 CSS 规则中。

```css
.sidebar {
  width: 300px;
  @media screen and (orientation: landscape) {
    width: 500px;
  }
}
//编译出来：
.sidebar {
  width: 300px; 
}
@media screen and (orientation: landscape) {
    .sidebar {
      width: 500px; 
    } 
}
//@media 也可以嵌套 @media：
@media screen {
  .sidebar {
    @media (orientation: landscape) {
      width: 500px;
    }
  }
}
//此时编译出来：
@media screen and (orientation: landscape) {
  .sidebar {
    width: 500px; } }
//在使用 @media 时，还可以使用插件#{}:
$media: screen;
$feature: -webkit-min-device-pixel-ratio;
$value: 1.5;
@media #{$media} and ($feature: $value) {
  .sidebar {
    width: 500px;
  }
}
//编译出来的 CSS：
@media screen and (-webkit-min-device-pixel-ratio: 1.5) {
  .sidebar {
    width: 500px; 
  } 
}
```

> * @extend 是用来扩展选择器或占位符

```css
.error {
  border: 1px #f00;
  background-color: #fdd;
}
.error.intrusion {
  background-image: url("/image/hacked.png");
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
//编译结果
.error, .seriousError {
  border: 1px #f00;
  background-color: #fdd; }
.error.intrusion, .seriousError.intrusion {
  background-image: url("/image/hacked.png"); 
}
.seriousError {
  border-width: 3px; 
}
```

> * @at-root 从字面上解释就是跳出根元素

```css
.a {
  color: red;
  .b {
    color: orange;
    .c {
      color: yellow;
      @at-root .d {
        color: green;
      }
    }
  }  
}
//编译结果
.a {
  color: red;
}
.a .b {
  color: orange;
}
.a .b .c {
  color: yellow;
}
.d {
  color: green;
}
```

> * @debug 在 Sass 中是用来调试的，当你的在 Sass 的源码中使用了 @debug 指令之后，Sass 代码在编译出错时，在命令终端会输出你设置的提示 Bug

```css
@debug 10em + 12em;
会输出：
Line 1 DEBUG: 22em
```

> * @warn 和 @debug 功能类似，用来帮助我们更好的调试 Sass。

> * @error 和 @warn、@debug 功能是如出一辙。

```css
@mixin error($x){
  @if $x < 10 {
    width: $x * 10px;
  } @else if $x == 10 {
    width: $x;
  } @else {
    @error "你需要将#{$x}值设置在10以内的数";
  }

}
.test {
  @include error(15);
}
//编译结果
=> 你需要将15值设置在10以内的数 on line 7 at column 5
```