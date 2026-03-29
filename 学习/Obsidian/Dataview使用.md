# Obsidian Dataview  黑曜石数据视图

[](https://github.com/blacksmithgu/obsidian-dataview?tab=readme-ov-file#obsidian-dataview)

Treat your [Obsidian Vault](https://obsidian.md/) as a database which you can query from. Provides a JavaScript API and pipeline-based query language for filtering, sorting, and extracting data from Markdown pages. See the Examples section below for some quick examples, or the full [reference](https://blacksmithgu.github.io/obsidian-dataview/) for all the details.  
把你的 [Obsidian Vault](https://obsidian.md/) 当作一个可以查询的数据库。提供 JavaScript API 和基于流水线的查询语言，用于从 Markdown 页面中筛选、排序和提取数据。请参阅下方的示例部分，获取一些简短示例，或完整[参考资料](https://blacksmithgu.github.io/obsidian-dataview/)获取所有细节。

## Examples  示例

[](https://github.com/blacksmithgu/obsidian-dataview?tab=readme-ov-file#examples)

Show all games in the game folder, sorted by rating, with some metadata:  
按评分排序，显示游戏文件夹中的所有游戏，并附带一些元数据：

```md
```dataview
table time-played, length, rating
from "games"
sort rating desc
```
```

[![Game Example](https://github.com/blacksmithgu/obsidian-dataview/raw/master/docs/docs/assets/game.png)](https://github.com/blacksmithgu/obsidian-dataview/blob/master/docs/docs/assets/game.png)

---

List games which are MOBAs or CRPGs.  
列出哪些是 MOBA 或 CRPG 的游戏。

```md
```dataview
list from #game/moba or #game/crpg
```
```

[![Game List](https://github.com/blacksmithgu/obsidian-dataview/raw/master/docs/docs/assets/game-list.png)](https://github.com/blacksmithgu/obsidian-dataview/blob/master/docs/docs/assets/game-list.png)

---

List all markdown [tasks](https://blacksmithgu.github.io/obsidian-dataview/data-annotation/#tasks) in un-completed projects:  
列出未完成项目中的所有 Markdown [任务](https://blacksmithgu.github.io/obsidian-dataview/data-annotation/#tasks) ：

```md
```dataview
task from #projects/active
```
```

[![Task List](https://github.com/blacksmithgu/obsidian-dataview/raw/master/docs/docs/assets/project-task.png)](https://github.com/blacksmithgu/obsidian-dataview/blob/master/docs/docs/assets/project-task.png)

---

Show all files in the `books` folder that you read in 2021, grouped by genre and sorted by rating:  
显示你 2021 年阅读的`书籍`文件夹中的所有文件，按类型分组并按评分排序：

```md
```dataviewjs
for (let group of dv.pages("#book").where(p => p["time-read"].year == 2021).groupBy(p => p.genre)) {
	dv.header(3, group.key);
	dv.table(["Name", "Time Read", "Rating"],
		group.rows
			.sort(k => k.rating, 'desc')
			.map(k => [k.file.link, k["time-read"], k.rating]))
}
```
```

[![Books By Genre](https://github.com/blacksmithgu/obsidian-dataview/raw/master/docs/docs/assets/books-by-genre.png)](https://github.com/blacksmithgu/obsidian-dataview/blob/master/docs/docs/assets/books-by-genre.png)

## Usage  用途

[](https://github.com/blacksmithgu/obsidian-dataview?tab=readme-ov-file#usage)

For a full description of all features, instructions, and examples, see the [reference](https://blacksmithgu.github.io/obsidian-dataview/). For a more brief outline, let us examine the two major aspects of Dataview: _data_ and _querying_.  
有关所有功能、说明和示例的完整描述，请参见[参考文献](https://blacksmithgu.github.io/obsidian-dataview/) 。为了更简要的概述，让我们来看看 Dataview 的两个主要方面： _数据_和_查询_ 。

#### **Data  数据**

[](https://github.com/blacksmithgu/obsidian-dataview?tab=readme-ov-file#data)

Dataview generates _data_ from your vault by pulling information from **Markdown frontmatter** and **Inline fields**.  
Dataview 通过从 **Markdown 前置和****内联字段**提取信息，从你的保险库生成_数据_ 。

- Markdown frontmatter is arbitrary YAML enclosed by `---` at the top of a markdown document which can store metadata about that document.  
    Markdown 前置页是任意的 YAML，被 `markdown` 文档顶部的---包围，可以存储该文档的元数据。
- Inline fields are a Dataview feature which allow you to write metadata directly inline in your markdown document via `Key:: Value` syntax.  
    内联字段是 Dataview 的一个功能，允许你直接在 Markdown 文档中内联写入元数据，通过以下方式 `关键：： 值`语法。

Examples of both are shown below:  
以下展示了两者的示例：

```yaml
---
alias: "document"
last-reviewed: 2021-08-17
thoughts:
  rating: 8
  reviewable: false
---
```

```md
# Markdown Page

Basic Field:: Value
**Bold Field**:: Nice!
You can also write [field:: inline fields]; multiple [field2:: on the same line].
If you want to hide the (field3:: key), you can do that too.
```

#### **Querying  查询**

[](https://github.com/blacksmithgu/obsidian-dataview?tab=readme-ov-file#querying)

Once you've annotated documents and the like with metadata, you can then query it using any of Dataview's four query modes:  
一旦你用元数据注释文档等内容，就可以使用 Dataview 的四种查询模式中的任意一种进行查询：

1. **Dataview Query Language (DQL)**: A pipeline-based, vaguely SQL-looking expression language which can support basic use cases. See the [documentation](https://blacksmithgu.github.io/obsidian-dataview/query/queries/) for details.  
    **Dataview 查询语言（DQL）：** 一种基于流水线、略带 SQL 风格的表达式语言，支持基本用例。详情请参见[文档](https://blacksmithgu.github.io/obsidian-dataview/query/queries/) 。
    
    ```md
    ```dataview
    TABLE file.name AS "File", rating AS "Rating" FROM #book
    ```
    ```
    
2. **Inline Expressions**: DQL expressions which you can embed directly inside markdown and which will be evaluated in preview mode. See the [documentation](https://blacksmithgu.github.io/obsidian-dataview/reference/expressions/) for allowable queries.  
    **内联表达式** ：DQL 表达式，你可以直接嵌入 Markdown 中，并在预览模式下进行评估。请参阅[允许查询的文档](https://blacksmithgu.github.io/obsidian-dataview/reference/expressions/) 。
    
    ```md
    We are on page `= this.file.name`.
    ```
    
3. **DataviewJS**: A high-powered JavaScript API which gives full access to the Dataview index and some convenient rendering utilities. Highly recommended if you know JavaScript, since this is far more powerful than the query language. Check the [documentation](https://blacksmithgu.github.io/obsidian-dataview/api/intro/) for more details.  
    **DataviewJS**：一个高性能的 JavaScript API，提供对 Dataview 索引的完全访问和一些方便的渲染工具。如果你懂 JavaScript，强烈推荐，因为它比查询语言强大得多。 [详情请查阅相关文档](https://blacksmithgu.github.io/obsidian-dataview/api/intro/) 。
    
    ```md
    ```dataviewjs
    dv.taskList(dv.pages().file.tasks.where(t => !t.completed));
    ```
    ```
    
4. **Inline JS Expressions**: The JavaScript equivalent to inline expressions, which allow you to execute arbitrary JS inline:  
    **内联 JS 表达式** ：JavaScript 中相当于内联表达式，允许你内联执行任意 JS：
    
    ```md
    This page was last modified at `$= dv.current().file.mtime`.
    ```
    

#### JavaScript Queries: Security Note  
JavaScript 查询：安全说明

[](https://github.com/blacksmithgu/obsidian-dataview?tab=readme-ov-file#javascript-queries-security-note)

JavaScript queries are very powerful, but they run at the same level of access as any other Obsidian plugin. This means they can potentially rewrite, create, or delete files, as well as make network calls. You should generally write JavaScript queries yourself or use scripts that you understand or that come from reputable sources. Regular Dataview queries are sandboxed and cannot make negative changes to your vault (in exchange for being much more limited).  
JavaScript 查询功能强大，但它们的访问权限与其他 Obsidian 插件相同。这意味着他们可能可以重写、创建或删除文件，并进行网络调用。你通常应该自己写 JavaScript 查询，或者使用你理解的脚本，或者来自权威来源的脚本。常规 Dataview 查询是沙箱式的，不能对你的保险库做负面更改（但交换条件是限制更大）。

## Contributing  贡献

[](https://github.com/blacksmithgu/obsidian-dataview?tab=readme-ov-file#contributing)

Contributions via bug reports, bug fixes, documentation, and general improvements are always welcome. For more major feature work, make an issue about the feature idea / reach out to me so we can judge feasibility and how best to implement it.  
欢迎通过错误报告、修复错误、文档和整体改进来贡献。对于更重要的功能工作，可以围绕功能创意提出问题/联系我，这样我们可以评估可行性以及如何最好地实现它。

#### Local Development  地方发展

[](https://github.com/blacksmithgu/obsidian-dataview?tab=readme-ov-file#local-development)

The codebase is written in TypeScript and uses `rollup` / `node` for compilation; for a first time set up, all you should need to do is pull, install, and build:  
代码库使用 TypeScript 编写，并使用 `rollup` / `node` 进行编译;第一次安装时，只需拉出、安装并搭建：

```shell
foo@bar:~$ git clone git@github.com:blacksmithgu/obsidian-dataview.git
foo@bar:~$ cd obsidian-dataview
foo@bar:~/obsidian-dataview$ npm install
foo@bar:~/obsidian-dataview$ npm run dev
```

This will install libraries, build dataview, and deploy it to `test-vault`, which you can then open in Obsidian. This will also put `rollup` in watch mode, so any changes to the code will be re-compiled and the test vault will automatically reload itself.  
这样可以安装库，构建数据视图，并部署到`测试库` ，然后你可以在 Obsidian 中打开。这也会让 `rollup` 进入监控模式，代码的更改会被重新编译，测试保险库会自动重新加载。

#### Preparing for creating pull requests  
准备创建拉取请求

[](https://github.com/blacksmithgu/obsidian-dataview?tab=readme-ov-file#preparing-for-creating-pull-requests)

If you plan on doing pull request, we would also recommend to do the following in advance of creating the pull request:  
如果你打算做拉取请求，我们也建议在创建拉取请求前先完成以下操作：

```shell
foo@bar:~$ npm run dev
foo@bar:~$ npm run check-format
foo@bar:~$ npm run format
foo@bar:~$ npm run test
```

The third step of `npm run format` is only needed if the format check reports some issue.  
只有在格式检查报告某些问题时才需要进行 `npm 运行格式`的第三步。

#### Installing to Other Vaults  
安装到其他保险库

[](https://github.com/blacksmithgu/obsidian-dataview?tab=readme-ov-file#installing-to-other-vaults)

If you want to dogfood dataview in your real vault, you can build and install manually. Dataview is predominantly a read-only store, so this should be safe, but watch out if you are adjusting functionality that performs file edits!  
如果你想在真实的仓库里做数据视图，可以手动构建和安装。Dataview 主要是只读存储，所以这样应该安全，但如果你调整了执行文件编辑的功能，要注意！

```shell
foo@bar:~/obsidian-dataview$ npm run build
foo@bar:~/obsidian-dataview$ ./scripts/install-built path/to/your/vault
```

#### Building Documentation  建筑文献

[](https://github.com/blacksmithgu/obsidian-dataview?tab=readme-ov-file#building-documentation)

We use `MkDocs` for documentation (found in `docs/`). You'll need to have python and pip to run it locally:  
我们用 `MkDocs` 来做文档（见 `docs/`）。你需要有 Python 和 Pip 才能本地运行：

```shell
foo@bar:~/obsidian-dataview$ pip3 install mkdocs mkdocs-material mkdocs-redirects
foo@bar:~/obsidian-dataview$ cd docs
foo@bar:~/obsidian-dataview/docs$ mkdocs serve
```

This will start a local web server rendering the documentation in `docs/docs`, which will live-reload on change. Documentation changes are automatically pushed to `blacksmithgu.github.io/obsidian-dataview` once they are merged to the main branch.  
这会启动一个本地 Web 服务器，在`文档中`渲染文档，文档在更改时实时重新加载。文档变更一旦合并到主分支，就会自动推送到 `blacksmithgu.github.io/obsidian-dataview` 那里。

#### Using Dataview Types In Your Own Plugin  
在您自己的插件中使用 Dataview 类型

[](https://github.com/blacksmithgu/obsidian-dataview?tab=readme-ov-file#using-dataview-types-in-your-own-plugin)

Dataview publishes TypeScript typings for all of its APIs onto NPM (as `blacksmithgu/obsidian-dataview`). For instructions on how to set up development using Dataview, see [setup instructions](https://blacksmithgu.github.io/obsidian-dataview/plugin/develop-against-dataview/).  
Dataview 将所有 API 的 TypeScript 类型发布到 NPM（作为 `blacksmithgu/obsidian-dataview` ）。关于如何使用 Dataview 设置开发的说明，请参见[设置说明](https://blacksmithgu.github.io/obsidian-dataview/plugin/develop-against-dataview/) 。