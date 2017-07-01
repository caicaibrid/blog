---
title: sass控制命令
date: 2017-07-01 15:06:31
tags:
    - Sass
categories: Sass
---

## scss控制命令

> * @if

```css
/*scss写法*/
@mixin blockOrHidden($boolean:true) {
  @if $boolean {
      @debug "$boolean is #{$boolean}";
      display: block;
    }
  @else {
      @debug "$boolean is #{$boolean}";
      display: none;
    }
}

.block {
  @include blockOrHidden;
}

.hidden{
  @include blockOrHidden(false);
}
/*编译结果*/
.block {
  display: block;
}

.hidden {
  display: none;
}
```

```css
/*scss写法*/
$num:10;

@mixin size($num){
    @if $num > 5  and $num < 8{
        width:$num * 10px;
    }
    @else if $num < 5{
        width:$num * $num * 10px;
    }
    @else{
        width:#{$num}px
    }
}

body{
    //@include size(7) 
    //@include size(10)
    @include size(3)
}
/*编译结果*/
body{
    //width:70px;
    //width:10px;
    width:90px;
}
```

> * @for 

1.@for $i from <start> through <end>

2.@for $i from <start> to <end>

$i 表示变量 start 表示起始值 end 表示结束值

两个的区别是关键字 through 表示包括 end 这个数，而 to 则不包括 end 这个数

```css
$col:8.33%;
//through
@for $i from  1 through 2 {
    .col-xs-#{$i} {
        width: $i * $col;
    }
} 


//to
@for $i from  1 to 2 {
    .col-md-#{$i} {
        width: $i * $col;
    }
} 
/*编译结果*/
.col-xs-1 {
  width: 8.33%; }
.col-xs-2 {
  width: 16.66%; }
.col-md-1 {
  width: 8.33%; }
```

> * @while 

```css
$types: 4;
$type-width: 20px;

@while $types > 0 {
    .while-#{$types} {
        width: $type-width + $types;
    }
    $types: $types - 1;
}
/*编译结果*/
.while-4 {
  width: 24px;
}
.while-3 {
  width: 23px;
}
.while-2 {
  width: 22px;
}
.while-1 {
  width: 21px;
}
```

> * @each 

@each $var in <list>

```css
$list: adam john wynn mason kuroir;
@mixin author-images {
    @each $author in $list {
        .photo-#{$author} {
            background: url("/images/avatars/#{$author}.png") no-repeat;
        }
    }
}

.author-bio {
    @include author-images;
}
/*编译结果*/
.author-bio .photo-adam {
  background: url("/images/avatars/adam.png") no-repeat; }
.author-bio .photo-john {
  background: url("/images/avatars/john.png") no-repeat; }
.author-bio .photo-wynn {
  background: url("/images/avatars/wynn.png") no-repeat; }
.author-bio .photo-mason {
  background: url("/images/avatars/mason.png") no-repeat; }
.author-bio .photo-kuroir {
  background: url("/images/avatars/kuroir.png") no-repeat; }
```
