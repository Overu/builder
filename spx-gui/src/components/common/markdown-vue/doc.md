## mdast-util-from-markdown 输入输入

### 开/闭合标签

以下测试基于几种场景：

1. 只有单标签

```HTML
<tag-name></tag-name>
```

2. 标签后面跟着文字

```HTML
<tag-name></tag-name>hello
```

3. 标签后面跟着换行

```HTML
<tag-name></tag-name>
hello
```

4. 标签前后跟着文字

```HTML
hello<tag-name></tag-name>world
```

- 未知标签: inline

  1. paragraph -> [tag-name]
  2. paragraph -> [tag-name, world]
  3. paragraph -> [tag-name, \nworld]
  4. paragraph -> [hello, tag-name, world]

- 已知标签

  - a、span：inline

    1. paragraph -> [span]
    2. paragraph -> [span, world]
    3. paragraph -> [span, \nworld]
    4. paragraph -> [hello, span, world]

  - div、p(raw): block

    1. html -> [div]
    2. html -> [divworld]
    3. html -> [div\nworld]
    4. paragraph -> [hello, div, world]

#### 自闭合

以下测试基于几种场景：

1. 只有单标签

```HTML
<tag-name />
```

2. 标签后面跟着文字

```HTML
<tag-name />world
```

3. 标签后面跟着换行

```HTML
<tag-name />
hello
```

4. 标签前后跟着文字

```HTML
hello<tag-name />world
```

- 未知标签: inline

  1. html -> [tag-name/]
  2. paragraph -> [tag-name/, world]
  3. html -> [tag-nam/\nworld]
  4. paragraph -> [hello, tag-name/, world]

- 已知标签

  - a、span：inline

    1. html -> [span/]
    2. paragraph -> [span/, world]
    3. html -> [span/\nworld]
    4. paragraph -> [hello, span/, world]

  - div、p(raw): block

    1. html -> [div/]
    2. html -> [div/world]
    3. html -> [div/\nworld]
    4. paragraph -> [hello, div/, world]

#### raw

以下测试基于集中场景：

1. 前面无内容

```HTML
<tag-name>
component1

component2
component3
</tag-name>
```

2. 前面有内容

```HTML
hello<tag-name>
component1

component2
component3
</tag-name>world
```

3. 后面有内容

```HTML
hello<tag-name>
component1

component2
component3
</tag-name>world
```

- pre

  1. html -> [\<pre>\ncomponent1\n\ncomponent\n2component3\n</pre>]
  2. [paragraph -> [hello, pre, ...], paragraph -> [..., world, /pre]]
  3. html -> [\<pre>\ncomponent1\n\ncomponent\n2component3\n</pre>world]

- div

  1. [html, paragraph, html]
  2. [paragraph -> [hello, div, ...], paragraph -> [..., world, /div]]
  3. [html, paragraph, html]
