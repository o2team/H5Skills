# SASS语法，学得更多一些

> 参考文档： [SASS 官方文档](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#css_extensions)


## _为前缀的文件命名
下划线(`_`)为前缀命名的sass文件是不会被编译为css文件。

注意：不带文件类型后缀，否则使用原生的`@import`实现文件引入。

使用场景：
```scss
@import "_rem";

// 另外一提，@import支持多文件简写法
@import "_function_v2","_rem";
```

## !default
用于设置变量的默认值。

注意：需要**在默认变量前**声明变量。

使用场景：wx/gb/static/_rem.scss
```scss
$rem_grid : 20 !default;
$page_width : 320 !default;
```
## List类型与Map类型变量
SASS的所有变量类型：
- `numbers` (e.g. 1.2, 13, 10px)
- `strings` of text, with and without quotes (e.g. "foo", 'bar', baz)
colors (e.g. blue, #04a3f9, rgba(255, 0, 0, 0.5))
- `booleans` (e.g. true, false)
- `nulls` (e.g. null)
- `lists` of values, separated by spaces or commas (e.g. 1.5em 1em 0 2em, Helvetica, Arial, sans-serif)
- `maps` from one value to another (e.g. (key1: value1, key2: value2))

### List类型变量
#### 两种写法：以空格 或 逗号 隔开
- (1px 2px) (3px 4px) 
- 1px 2px , 3px 4px 

#### 方法
```scss
// nth($list,$n)
nth(10px 20px 30px, 1) => 10px

// $join($list,$list)
join(10px 20px, 30px 40px) => 10px 20px 30px 40px

// $append($list,$val,$separator:auto)
append(10px 20px, 30px) => 10px 20px 30px
append((blue, red), green) => blue, red, green
append((blue, red), green, space) => blue red green

// @each $var in <list or map>
@each $v1,$v2 in (1px #999) {
   border-width: $v1;
   border-color: $v2;
}

```

### Map类型变量
####写法： 
- ($color: 1px, $radius: 2px , $width: 200px)

#### 方法：
```scss
// map-get($map,$key)
// map-merge($map1,$map2)
// @each $var in <list or map>
```

## 使用 @each计算多维List变量
使用场景：
```scss
// 简化重复代码
$animal-list: puma, sea-slug, egret, salamander;
@each $animal in $animal-list {
  .icon_#{$animal} {
    background-image: url('images/#{$animal}.png');
  }
}

// 计算boder-radius的两倍大小
@mixin border($pos,$color:#999,$rad:null) {
	border-#{$pos}: 1px solid $color;
	@if $rad {
		// 两倍大小
		$x2rad: null;
		@each $i in $rad {
			$x2rad: append($x2rad,$i * 2);
		}
		border-radius: $x2rad;
	}
}

.test {
	@include border(bottom,$rad: 7px 0 0);
}
```

## 巧用Map变量实现部分传参
使用场景：1像素边框

```scss
// 以下是简化的例子
@mixin border($pos,$color: #999,$rad:null){
  border-#{$pos}-color: $color;
  border-radius: $rad;
}
@include border(top);
@include border(top,#eee);
// 指定参数对应的变量值
@include border(top,$rad:2px);

```



## @media
在SASS里，@media也可以**嵌套使用**。

例子：
```scss
.nav {
  width: 540px;
  @media screen and (max-width:320px) {
    width: 320px;
  }
}
```

## @content
**在@mixin内使用**，用以嵌入一堆样式。

注意：
1.在@content内使用的变量参数只有全局变量或所在块的作用域内的局部变量才有效，不能是默参。
2.在@media里使用@extend时，要确保**继承的选择器需要在@media内声明过**，否则报错。

使用场景：封装@media
```scss
@mixin query( $limit ) {
    @media screen and ( min-width: $limit) {
        & {
            @content;
        }
    }
}

.wx_wrap {
  @include query(320px) {
    .header {
      font-size: 12px;
    }
  }
}

// @extend的注意
.wx_wrap {
  @include query(320px) {
    %clearfix
      &:after {
        content: '\20';
        display: block;
        clear: both;
      }
    }
    .list {
      @extend %clearfix;
    }
    
  }
}
// 默认参数无效，全局变量有效
$color: #000;
@mixin colors($color: blue) {
  @content;
  background-color: $color;
}
.colors {
  @include colors { color: $color; }
}

// 同一作用域块内的局部变量有效
$color: #000;
@mixin colors($color: blue) {
  @content;
  background-color: $color;
}
.colors {
  $color: #eee;
  @include colors { color: $color; }
}

```

## @at-root
用来**移除父选择器**，不遵循嵌套规则。
抑或是，移除**外面的任何指令**，还可以通过空格用于移除多个指令，例如@at-root (without: media supports )同时移除了@media和@supports外部指令

使用场景：把相关代码写在同一块，但不想嵌套太深
```scss
.cate_wrap {
  @include sticky('.cate');
  @at-root .cate{
    position: relative;
  }
}
```
使用场景：移除外部指令
```scss
@media print {
  .page {
    width: 8in;
    @at-root (without: media) {
      color: red;
    }
  }
}
```
---
## 认清几种循环判断指令

### @if
`@if`指令是一个SassScript，它可以根据条件来处理样式块，如果条件为true返回一个样式块，反之false返回另一个样式块。在Sass中除了`@if`，还可以配合`@else if`和`@else` 一起使用。

### @for循环
在 Sass 的 @for 循环中有两种方式：
```scss
// $i 表示变量
// start 表示起始值
// end 表示结束值

@for $i from <start> through <end>
@for $i from <start> to <end>
```
从 <start> 开始循环，到 <end> 结束，而`through`、`to`的区别，关键字`through`表示包括end这个数，而`to`则不包括end这个数。

`@for`循环指令除了可以从小数值到大数值循环之外，还可以从大数值循环到小数值。而且两种形式都支持（递增或递减）。

### @while循环
`@while`指令也需要SassScript表达式（像其他指令一样），并且会生成不同的样式块，直到表达式值为false时停止循环。这个和`@for`指令很相似，只要`@while`后面的条件为true就会执行。

### @each
`@each`循环就是去遍历一个列表，然后从列表中取出对应的值。
`@each`循环指令形式：
```scss
@each $var in <list>
```
 $var 就是一个变量名， <list> 是一个SassScript表达式，他将返回一个列表值。变量 $var 会在列表中做出遍历，并且遍历出与 $var 对应的样式块。
 
 