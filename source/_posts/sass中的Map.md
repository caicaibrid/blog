---
title: sass中的Map
date: 2017-07-01 17:54:17
tags:
    - Sass
categories: Sass
---
### Map

> * map 常常被称为数据地图，也有人称其为数组，因为他总是以 key:value 成对的出现

```css
// 以下为一个map
$map:(
    $key:value,
    ...
)
$theme-color: (
    default: (
        bgcolor: #fff,
        text-color: #444,
        link-color: #39f
    ),
    primary:(
        bgcolor: #000,
        text-color:#fff,
        link-color: #93f
    ),
    negative: (
        bgcolor: #f36,
        text-color: #fefefe,
        link-color: #d4e
    )
);
```

> * map-get($map,$key) 根据 $key 参数，返回 $key 在 $map 中对应的 value 值。如果 $key 不存在 $map中，将返回 null 值。

```css
$social-colors: (
    dribble: #ea4c89,
    facebook: #3b5998,
    github: #171515,
    google: #db4437,
    twitter: #55acee
);
.btn-dribble{
  color: map-get($social-colors,facebook);
}
//编译结果
.btn-dribble {
  color: #3b5998; }
```

> * map-has-key($map,$key) 函数将返回一个布尔值。当 $map 中有这个 $key，则函数返回 true，否则返回 false

```css
$social-colors: (
    dribble: #ea4c89,
    facebook: #3b5998,
    github: #171515,
    google: #db4437,
    twitter: #55acee
);
@function colors($color){
    @if not map-has-key($social-colors,$color){
        @warn "No color found for `#{$color}` in $social-colors map. Property omitted.";
    }
    @return map-get($social-colors,$color);
}
.btn-dribble {
    color: colors(dribble);
}
.btn-facebook {
    color: colors(facebook);
}
.btn-github {
    color: colors(github);
}
.btn-google {
    color: colors(google);
}
.btn-twitter {
    color: colors(twitter);
}
.btn-weibo {
    color: colors(weibo);
}
//编译结果
WARNING: No color found for `weibo` in $social-colors map. Property omitted.
         on line 10 of ./8457/5JPS/index.scss
.btn-dribble {
  color: #ea4c89; }
.btn-facebook {
  color: #3b5998; }
.btn-github {
  color: #171515; }
.btn-google {
  color: #db4437; }
.btn-twitter {
  color: #55acee; }
```

> * map-keys($map) 函数将会返回 $map 中的所有 key。这些值赋予给一个变量，那他就是一个列表

```css
$social-colors: (
    dribble: #ea4c89,
    facebook: #3b5998,
    github: #171515,
    google: #db4437,
    twitter: #55acee
);
map-keys($social-colors);
= > "dribble","facebook","github","google","twitter"
```

> * map-values($map) 类似于 map-keys($map) 功能，不同的是 map-values($map )获取的是 $map 的所有 value 值

```css
$social-colors: (
    dribble: #ea4c89,
    facebook: #3b5998,
    github: #171515,
    google: #db4437,
    twitter: #55acee
);
map-values($social-colors);
= > #ea4c89,#3b5998,#171515,#db4437,#55acee
```

> * map-merge($map1,$map2) 函数是将 $map1 和 $map2 合并，然后得到一个新的 $map

```css
$color: (
    text: #f36,
    link: #f63,
    border: #ddd,
    backround: #fff
);
$typo:(
    font-size: 12px,
    line-height: 1.6
);
$newmap: map-merge($color,$typo);
=> $newmap:(
       text: #f36,
       link: #f63,
       border: #ddd,
       background: #fff,
       font-size: 12px,
       line-height: 1.6
   );
```

> * map-remove($map,$key) 

1. map-remove($map,$key) 函数是用来删除当前 $map 中的某一个 $key，从而得到一个新的 map。其返回的值还是一个 map。他并不能直接从一个 map 中删除另一个 map，仅能通过删除 map 中的某个 key 得到新 map。
2. 如果删除的 key 并不存在于 $map 中，那么 map-remove() 函数返回的新 map 和以前的 map 一样。 

```css
$social-colors: (
    dribble: #ea4c89,
    facebook: #3b5998,
    github: #171515,
    google: #db4437,
    twitter: #55acee
);
$map:map-remove($social-colors,dribble);
=> $map:(
       facebook: #3b5998,
       github: #171515,
       google: #db4437,
       twitter: #55acee
   );
```
