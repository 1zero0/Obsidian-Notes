这样写可以生成一个在左侧的注释
```text
> [!NOTE|aside-l] 左侧注释
> 注释内容
```
>[!Note|aside-l] 左侧注释
>这特么就是好东西


这样写可以生成一个在右侧的注释
```text
> [!NOTE|aside-r] 右侧注释
> 注释内容
```
>[!note|aside-r] 右侧注释
>我宣布，这特么就是最好用的工具！


如果想要注释可以折叠，你可以用 callout 的折叠语法
```text
> [!NOTE|aside-l]+ 默认展开的注释
> 注释内容

> [!NOTE|aside-r]- 默认折叠的注释
> 注释内容
```
>[!note|aside-l]+ 默认展开的注释
>你知道为什么这么牛逼么？

>[!note|aside-r]- 默认折叠的注释
>其实我也不知道~哎嘿~


如果你想试试其他样式的边注 callout，可以这样写
```text
> [!ERROR|aside-l] 红色样式
> 还可以用别的callout，例如important, cite 等
```
>[!error|aside-l] 红色样式
>还可以用别的callout，例如important，cite等


### 基本使用

```text
> [!NOTE] Title
> Content
```

复制上面两行内容粘贴到 obsidian 中，他们会自动渲染成 callout 的样式。

分为四个部分：

- 开头的 >
- 标明 callout 类型的 NOTE
- Callout 的标题 Title
- Callout 的正文 Content

obsidian 共提供了 13 种 callout

- 他们是大小写不敏感的，例如 `> [!BUG]`,`> [!bug]` 和 `> [!Bug]` 的效果是一样的；
- 同一类型的 callout 可能有很多种别名，例如 `> [!info]` 和 `> [!todo]` 的样式是一样的；  
    
- note
>[!NOTE]
- abstract, summary, tldr
>[!abstract] abstract，summary，tldr
- info
> [!info]
- todo
> [!todo]
- tip, hint, important
>[!tip] tip，hint，important
- success, check, done
>[!success] success，check，done
- question, help, faq
>[!question]

>[!help]

>[!faq]
- warning, caution, attention
>[!warning] warning，caution，attention
- failure, fail, missing
>[!failure] failure,fail,missing
- danger, error
>[!danger] danger,error
- bug
>[!bug] bug
- example
>[!example]
- quote, cite
>[!quote] quote,cite

### Callout 的嵌套使用

每到下一层多加个一个 `>` 即可，例如

```text
> [!question] Can callouts be nested?
> > [!todo] Yes!, they can.
> > > [!example]  You can even use multiple layers of nesting.
```
> [!question] Can callouts be nested?
> > [!todo] Yes!, they can.
> > > [!example]  You can even use multiple layers of nesting.

### Callout 的展开与折叠

```text
> [!NOTE]+ 左边多了个加号
> 代表 Callout 默认展开

> [!NOTE]- 左边多了个减号
> 代表 Callout 默认折叠
```
> [!NOTE]+ 左边多了个加号
> 代表 Callout 默认展开

> [!NOTE]- 左边多了个减号
> 代表 Callout 默认折叠

### Callout 的换行

要换行的地方用 `>` 连接保证 Callout 不断即可，你也可以直接用 shift enter 换行，这样 obsidian 会自动补充开头的 `>`

```text
> [!NOTE] Callout 的换行
> 第一行
> 
> 第二行
```
> [!NOTE] Callout 的换行
> 第一行
> 
> 第二行

### Callout 自定义

如果你觉得 obsidian 提供的 13 种 callout 都适用于你当前需要的场景，obsidian 还提供了自定义 callout 需要的两个 css 变量

```css
--callout-color
--callout-icon
```

- 前者用于控制 callout 的背景颜色
- 后者用于控制 callout 的图标，图标代码可以从 [Lucide](https://link.zhihu.com/?target=https%3A//lucide.dev/) 找到，也可以使用自己找的 svg 图标

举个例子，如果你想要获得一个名为 custom-question-type 的 Callout，那么你只需要创建一个 css 文件，在里面写到

```css
.callout[data-callout="custom-question-type"] {
    --callout-color: 0, 0, 0;
    --callout-icon: lucide-alert-circle;
}
```

之后你可以在笔记中用到

```text
> [!custom-question-type] Title
> Contents
```

需要注意，Callout 的名称中不能包含空格。

使用 svg 作为图标的格式为

```text
--callout-icon: '<svg>...custom svg...</svg>';
```

你可以在 [[Obsidian样式-Callout样式]] 中找到许多已经写好的 callout 样式，直接放到你的库的配置文件中即可使用。

修改图标和颜色足以实现大部分 callout 的自定义，下面给出另一种 callout 自定义的玩法：**添加修饰词而非创建新的 callout 名称**

这种用法也挺常见，例如 blue topaz 主题的实现 callout 的百分比显示

```text
> [!Note|50%] Title
> Content
```
> [!Note|50%] Title
> Content

