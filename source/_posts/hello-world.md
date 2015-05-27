title: Hello World
tags:
- code
- hello
- world
---
Welcome to [Hexo](http://hexo.io/)! This is your very first post. Check [documentation](http://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](http://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

``` php
$var = array("test" => "Stuff");
```

Say you have a set of colors that correspond to class names:

```scss
$category-colors: (
  "bananas": "yellow",
  "oranges": "orange",
  "blue-milk": "cyan",
  "cabbage": "lime",
  "sour-warheads": "magenta"
);
```

And sometimes those class names recolor a background, but sometimes they might override something else, like a border or text color. Try this mixin on for size:

```scss
@mixin category-colors($attr:color)
  @each $class, $color in $category-colors
    &.#{$class}
      #{$attr}: #{$color}
```

So this call will output subclasses of `.widget.bananas`, etc., each with the appropriate text color ...

```sass
.widget
  @include category-colors
```

And this one will output subclasses with the appropriate background color (or whatever other attribute you'd like).

```sass
.widget
  @include category-colors(background-color)
```