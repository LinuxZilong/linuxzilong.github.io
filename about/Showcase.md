---
title: 特性示例
---

本站各种组件功能演示

## 终端播放器

[AsciinemaPlayer](https://github.com/asciinema/asciinema-player) 可将 [asciinema](https://github.com/asciinema/asciinema) 录制的终端文件嵌入到 web 进行播放。

本站为其添加了外部字体 [MesloLGS NF](https://github.com/romkatv/powerlevel10k#manual-font-installation) ，以更好地支持 powerlevel10k 主题。

import AsciinemaPlayer from '@site/src/components/AsciinemaPlayer';

```jsx
import AsciinemaPlayer from "@site/src/components/AsciinemaPlayer";
```

示例

```jsx live
<AsciinemaPlayer
  src="/casts/neofetch.cast"
  poster="npt:0:5"
  preload={true}
  autoPlay={true}
  idleTimeLimit="2"
/>
```

组件参数 [`asciinema-player`](https://github.com/asciinema/asciinema-player)

| Property        | Usage                                                                                                                                                                                     |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `src`           | The location of the cast file, must be available from the browser.                                                                                                                        |
| `style`         | Any additional CSS styles to apply.                                                                                                                                                       |
| `cols`          | The number of columns in the player's terminal.                                                                                                                                           |
| `rows`          | The number of rows in the player's terminal.                                                                                                                                              |
| `autoPlay`      | Set this option to `true` if playback should start automatically.                                                                                                                         |
| `preload`       | Set this option to `true` if the recording should be preloaded on player's initialization.                                                                                                |
| `loop`          | Set this option to either true or a number if playback should be looped. When set to a number (e.g. 3) then the recording will be re-played given number of times and stopped after that. |
| `startAt`       | Start playback at a given time.                                                                                                                                                           |
| `speed`         | Playback speed. The value of 2 means 2x faster.                                                                                                                                           |
| `idleTimeLimit` | Limit terminal inactivity to a given number of seconds.                                                                                                                                   |
| `theme`         | Terminal color theme.                                                                                                                                                                     |
| `poster`        | Poster (a preview frame) to display until the playback is started.                                                                                                                        |
| `fit`           | Controls the player's fitting (sizing) behaviour inside its container element.                                                                                                            |
| `fontSize`      | Size of the terminal font.                                                                                                                                                                |

## 文档切换栏

    ```mdx-code-block
    import ContextSwitcher from '@site/src/components/ContextSwitcher';

    <div className="my-4 flex items-center justify-end px-4">
      <ContextSwitcher className="flex-[3]" />
    </div>
    ```

```mdx-code-block
import ContextSwitcher from '@site/src/components/ContextSwitcher';

<div className="my-4 flex items-center justify-end px-4">
  <ContextSwitcher className="flex-[3]" />
</div>
```

## 文档渐变卡片

    ```mdx-code-block
    import GetStartedCard from '@site/src/components/GetStartedCard';

    <div className="grid xl:grid-cols-6 gap-4">

    <GetStartedCard
      title="Kubernetes"
      className="xl:col-span-2 from-[#21D4FD] to-[#B721FF]"
      getStartedLink="/Kubernetes"
      bgClassName="h-480 rotate-[-28deg] right-[-48px] bottom-[-6rem]"
    />
    <GetStartedCard
      title="Istio"
      className="xl:col-span-2 from-[#0093E9] to-[#80D0C7]"
      getStartedLink="/Service-Mesh/Istio/"
      bgClassName="h-48 rotate-[-28deg] right-[-48px] bottom-[-6rem]"
    />
    <GetStartedCard
      title="Nginx"
      className="xl:col-span-2 from-[#FF2525] to-[#FFE53B]"
      getStartedLink="/Proxy/Nginx/"
      bgClassName="h-48 rotate-[-28deg] right-[-48px] bottom-[-6rem]"
    />
    </div>
    ```

```mdx-code-block
import GetStartedCard from '@site/src/components/GetStartedCard';

<div className="grid xl:grid-cols-6 gap-4">

<GetStartedCard
  title="Kubernetes"
  className="xl:col-span-2 from-[#21D4FD] to-[#B721FF]"
  getStartedLink="/Kubernetes"
  bgClassName="h-480 rotate-[-28deg] right-[-48px] bottom-[-6rem]"
/>
<GetStartedCard
  title="Istio"
  className="xl:col-span-2 from-[#0093E9] to-[#80D0C7]"
  getStartedLink="/Service-Mesh/Istio/"
  bgClassName="h-48 rotate-[-28deg] right-[-48px] bottom-[-6rem]"
/>
<GetStartedCard
  title="Nginx"
  className="xl:col-span-2 from-[#FF2525] to-[#FFE53B]"
  getStartedLink="/Proxy/Nginx/"
  bgClassName="h-48 rotate-[-28deg] right-[-48px] bottom-[-6rem]"
/>
</div>
```

## 代码块

```go title="src/components/demo.go"
func main() {
  http.HandleFunc("/", func(writer http.ResponseWriter, request *http.Request) {
    // highlight-next-line
    message := strings.Join([]string{"Hello", "world!"}, " ")
    _, err := writer.Write([]byte(message))
    if err != nil {
      panic(err)
    }
  })
  // highlight-start
  if err := http.ListenAndServe(":8080", nil); err != nil {
    panic(err)
  }
  // highlight-end
}

func main() {
  http.HandleFunc("/", func(writer http.ResponseWriter, request *http.Request) {
    // This will error
    message := strings.Join([]string{"Hello", "world!"}, " ")
    _, err := writer.Write([]byte(message))
    if err != nil {
      panic(err)
    }
  })
  // highlight-error-start
  if err := http.ListenAndServe(":8080", nil); err != nil {
    panic(err)
  }
  // highlight-error-end
}

func main() {
  http.HandleFunc("/", func(writer http.ResponseWriter, request *http.Request) {
    // This will success
    message := strings.Join([]string{"Hello", "world!"}, " ")
    _, err := writer.Write([]byte(message))
    if err != nil {
      panic(err)
    }
  })
  // highlight-success-start
  if err := http.ListenAndServe(":8080", nil); err != nil {
    panic(err)
  }
  // highlight-success-end
}
```

## 引入外部文档

```js
import CodeBlock from "@theme/CodeBlock";
import Source from "!!raw-loader!./kubesphere.yaml";

<CodeBlock language="yaml" title="kubesphere.yaml">
  {Source}
</CodeBlock>;
```

import CodeBlock from '@theme/CodeBlock';
import Source from '!!raw-loader!./kubesphere.yaml';

<CodeBlock language="yaml" title="kubesphere.yaml">{Source}</CodeBlock>

## 提示和标注

Docusaurus 有一个特殊的语法来创建警告和标注：

    :::tip My tip
    Use this awesome feature option
    :::

    :::note
    In practice, those are not really HTML elements, but React JSX elements, which we'll cover next!
    :::

    :::info
    This action is dangerous
    ```md title="my-blog-post.md"
    ---
    author: Joel Marcey
    author_title: Co-creator of Docusaurus 1
    author_url: https://github.com/JoelMarcey
    author_image_url: https://github.com/JoelMarcey.png
    ---
    ```
    :::

    :::danger Take care
    This action is dangerous
    :::

    :::warning
    This action is caution
    :::

:::tip My tip
Use this awesome feature option
:::

:::note
In practice, those are not really HTML elements, but React JSX elements, which we'll cover next!
:::

:::info
This action is dangerous

```md title="my-blog-post.md"
---
author: Joel Marcey
author_title: Co-creator of Docusaurus 1
author_url: https://github.com/JoelMarcey
author_image_url: https://github.com/JoelMarcey.png
---
```

:::

:::danger Take care
This action is dangerous
:::

:::warning
This action is caution
:::

## 下拉框

<details>
<summary>下拉框示例</summary>

{'展开内容'}

</details>

<details style={{backgroundColor: 'rgb(255, 248, 230)', border: '1px solid rgb(230, 167, 0)'}}>
  <summary>点击此处展开内容</summary>
  {'展开内容'}
</details>

<details style={{backgroundColor: '#e9f5e7', border: '1px solid rgb(20 163 111)'}}>
  <summary>点击此处展开内容</summary>
  {'展开内容'}
</details>

## 卡片组

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

```html
<Tabs>
  <!-- <Tabs groupId="卡片组"> -->
  <TabItem value="卡片1">
    <!-- <TabItem value="卡片1" label="卡片1标题"> -->
    {'卡片1内容...'}
  </TabItem>
  <TabItem value="卡片2">
    <!-- <TabItem value="卡片2" label="卡片2标题"> -->
    {'卡片2内容...'}
  </TabItem>
</Tabs>
```

<Tabs groupId="卡片组">
<TabItem value="卡片1" label="卡片1标题">

```yaml title="my-blog-post.md"
---
authors:
  name: Joel Marcey
  title: Co-creator of Docusaurus 1
  url: https://github.com/JoelMarcey
  image_url: https://github.com/JoelMarcey.png
  email: jimarcey@gmail.com
---
```

</TabItem>

<TabItem value="卡片2" label="卡片2标题">

```yaml title="my-blog-post.md"
---
authors:
  - name: Joel Marcey
    title: Co-creator of Docusaurus 1
    url: https://github.com/JoelMarcey
    image_url: https://github.com/JoelMarcey.png
    email: jimarcey@gmail.com
  - name: Sébastien Lorber
    title: Docusaurus maintainer
    url: https://sebastienlorber.com
    image_url: https://github.com/slorber.png
---
```

</TabItem>
</Tabs>

## 组合嵌套 下拉+信息

:::info

The `authors` system is very flexible and can suit more advanced use-case:

<details>
  <summary>Mix inline authors and global authors</summary>

You can use global authors most of the time, and still use inline authors:

```md title="my-blog-post.md"
---
authors:
  - jmarcey
  - slorber
  - name: Inline Author name
    title: Inline Author Title
    url: https://github.com/inlineAuthor
    image_url: https://github.com/inlineAuthor
---
```

</details>

<details>
  <summary>Local override of global authors</summary>

You can customize the global author's data on per-blog-post basis:

```md title="my-blog-post.md"
---
authors:
  - key: jmarcey
    title: Joel Marcey's new title
  - key: slorber
    name: Sébastien Lorber's new name
---
```

</details>

<details>
  <summary>Localize the author's configuration file</summary>

The configuration file can be localized, just create a localized copy of it at:

```bash
website/i18n/[locale]/docusaurus-plugin-content-blog/authors.yml
```

</details>

:::

## 组合嵌套 卡片+提示

:::tip

Use the callback for all your customization needs:

```mdx-code-block
<Tabs>
<TabItem value="disable-per-post" label="Per-post disabling">
```

**Disable reading time on one page:**

```js title="docusaurus.config.js"
module.exports = {
  presets: [
    [
      "@docusaurus/preset-classic",
      {
        blog: {
          showReadingTime: true,
          // highlight-start
          readingTime: ({ content, frontMatter, defaultReadingTime }) =>
            frontMatter.hide_reading_time
              ? undefined
              : defaultReadingTime({ content }),
          // highlight-end
        },
      },
    ],
  ],
};
```

Usage:

```md "my-blog-post.md"
---
hide_reading_time: true
---

This page will no longer display the reading time stats!
```

```mdx-code-block
</TabItem>
<TabItem value="passing-options" label="Passing options">
```

**Pass options to the default reading time function:**

```js title="docusaurus.config.js"
module.exports = {
  presets: [
    [
      "@docusaurus/preset-classic",
      {
        blog: {
          // highlight-start
          readingTime: ({ content, defaultReadingTime }) =>
            defaultReadingTime({ content, options: { wordsPerMinute: 100 } }),
          // highlight-end
        },
      },
    ],
  ],
};
```

```mdx-code-block
</TabItem>
<TabItem value="using-custom-algo" label="Using custom algorithms">
```

**Use a custom implementation of reading time:**

```js title="docusaurus.config.js"
const myReadingTime = require("./myReadingTime");

module.exports = {
  presets: [
    [
      "@docusaurus/preset-classic",
      {
        blog: {
          // highlight-next-line
          readingTime: ({ content }) => myReadingTime(content),
        },
      },
    ],
  ],
};
```

```mdx-code-block
</TabItem>
</Tabs>
```

:::

## 浏览器外壳

在官方基础上为右侧按钮添加了打开指定连接参数

```mdx-code-block
import BrowserWindow from '@site/src/components/BrowserWindow';

<BrowserWindow url="https://www.baidu.com">

<h3>My Doc Section</h3>

Hello world message with some **bold** text, some _italic_ text and a [link](/)

![img alt](/img/docusaurus.png)

> Easy to maintain open source documentation websites.
>
> — Docusaurus

<details>
  <summary>Toggle me!</summary>
  <div>
    <div>This is the detailed content</div>
    <br/>
    <details>
      <summary>
        Nested toggle! Some surprise inside...
      </summary>
      <div>
        😲😲😲😲😲
      </div>
    </details>
  </div>
</details>

</BrowserWindow>
```

## 交互式代码

    ```jsx live noInline
    const project = 'Docusaurus';

    const Greeting = () => <p>Hello {project}!</p>;

    render(<Greeting />);
    ```

```jsx live noInline
const project = "Docusaurus";

const Greeting = () => <p>Hello {project}!</p>;

render(<Greeting />);
```

## 注释

AnnotatedCommand 组件用于创建一个小文本注释。

import AnnotatedCommand from '@site/src/components/AnnotatedCommmand';

```jsx
import AnnotatedCommand from "@site/src/components/AnnotatedCommmand";
```

示例

```jsx live
<AnnotatedCommand annotation="Go to beginning of buffer, change two words">
  gg2cw
</AnnotatedCommand>
```

## Caret

Caret 组件可以显示一个块插入符号，这是 ASCII 终端的标准，也可以显示一个行插入符号，这可以用在 iTerm 之类的东西中来表示 Vim 的插入模式。

import Caret from '@site/src/components/Caret';

```jsx
import Caret from "@site/src/components/Caret";
```

Use the component as below:

```jsx live
<code className="language-python">
  def search_for_word<Caret caretStyle="block">(</Caret>word):
</code>
```

## 博客模板

```yaml title="website/blog/2019-09-05-博客模板.md"
---
title: Welcome Docusaurus v2
description: This is my first post on Docusaurus 2.
tags: [Features, Docusaurus-v2]
date: 2021-09-13T10:00

slug: welcome-docusaurus-v2
image: https://i.imgur.com/mErPwqL.png
hide_table_of_contents: false

authors:
  - name: Joel Marcey
    title: Co-creator of Docusaurus 1
    url: https://github.com/JoelMarcey
    image_url: https://github.com/JoelMarcey.png
  - name: Sébastien Lorber
    title: Docusaurus maintainer
    url: https://sebastienlorber.com
    image_url: https://github.com/slorber.png
---
Welcome to this blog. This blog is created with

<!-- truncate -->

This is my first post on Docusaurus 2.

A whole bunch of exploration to follow.
```

## 文档列表

```
import DocCardList from '@theme/DocCardList';

<DocCardList />
```

## 文档模板

```yaml
---
title: demo
description: 关于此页的简短描述
keywords: [描述, 中心的, 关键词]
date: 2021-09-13T10:00
# tags: [hello, docusaurus-v2]
tags: 文章标签，格式类似于数组。
  - 演示
  - 开始上手

id: 文章 ID，用于自定义 URL 地址。
slug: welcome-docusaurus-v2
image: https://i.imgur.com/mErPwqL.png

sidebar_label: demo
sidebar_position: 2
displayed_sidebar: tutorialSidebar
hide_table_of_contents: true

# authors: [jmarcey, slorber]
authors:
  name: Joel Marcey
  title: Docusaurus 1 合作创造者
  url: https://github.com/JoelMarcey
  image_url: https://github.com/JoelMarcey.png
  email: jimarcey@gmail.com
---
```

## 更多示例

- https://docusaurus.io/zh-CN/docs/next/markdown-features
- https://docusaurus.io/docs/markdown-features/code-blocks
- https://docusaurus.io/docs/styling-layout
- [导入代码片段](https://docusaurus.io/zh-CN/docs/next/markdown-features/react#importing-code-snippets)
- [导入 Markdown](https://docusaurus.io/zh-CN/docs/next/markdown-features/react#importing-markdown)
- [多文档导航栏](https://stackoverflow.com/questions/60783595/is-there-a-way-to-have-two-docs-in-docusaurus-2)
- https://markdown.com.cn/

<!--
## 书写规范

以安全分类来举例:

- 有具体归属的放在各自中文件夹下面,打上安全标签
  - k8s/安全
  - 容器/安全
  - linux/网络/安全

- 没有具体归属的则放在以tagC命名的文件夹下,打上安全标签和自身标签
  - 安全/Jumpserver
  - 安全/Metasploit
名词解释缩写格式：子网广播转发 (Subnet Broadcast Forwarding，SBF)

react引入均放在文档开头

命令行尽量不带$和#，除非命令下方贴出返回结果。避免# 用sudo 代替


readme 可以不写一句话简介，文章要写

所有实验都要有vagrant或者dockerfile


### 代码块
一般普通用户执行命令使用 $ 开头，root 用户执行命令使用 # 开头。

但 docusaurus 代码块的复制按钮不会 ignore $ 所以在书写命令时候尽量不要带 $

普通用户直接书写命令，特权用户在命令前加 sudo

## 特殊字符
"　　"这个要替换成空格

### 文档列表
每个目录添加readme 并且引入文档列表
```mdx-code-block
import DocCardList from '@theme/DocCardList';

<DocCardList />
```
## 目录结构
文章配套的环境放入_Playground
常备环境放入_HomeLab

## 备忘
1. blog 引入外部文档
```js
import CodeBlock from '@theme/CodeBlock';
import Source from '!!raw-loader!./kubesphere.yaml';

<CodeBlock language="yaml" title="kubesphere.yaml">{Source}</CodeBlock>
```
2. 图片全部转换为svg https://vectorizer.ai/
3. 添加ico
- https://www.zhangxinxu.com/sp/svgo/
- https://www.iconfinder.com/
4. 国际化命令
- `npm run docusaurus write-translations`
  默认情况下，文件会被写入 `website/i18n/<defaultLocale>/...`。
5. 统计行数
cloc --vcs git .
--exclude-dir 来过滤掉某些路径
--by-file 参数可以按文件进行统计，输出每个文件的代码行数、空行数、注释行数等信息。这种方式适合对特定文件或目录进行代码行数的统计分析。
--by-lang 参数可以按编程语言进行统计，输出每种语言的代码行数、空行

- Markdown 规范检查 https://coding.net/help/docs/ci/practice/lint/markdown.html
- https://type.cyhsu.xyz/2022/03/markdown-linter-a-primer/
- https://github.com/DavidAnson/markdownlint/blob/main/doc/Rules.md
- https://docusaurus.io/docs/using-plugins#docusauruspreset-classic
- https://docusaurus.io/docs/api/plugins/@docusaurus/plugin-content-pages
- 导入md https://docusaurus.io/zh-CN/docs/markdown-features/react#importing-markdown

https://docusaurus.io/docs/api/themes/configuration#hooks
Generate i18n npm run docusaurus write-translations
npm run docusaurus swizzle

本站分支参考：https://thewang.net/blog/github-action-create-pr-and-merge-into-main
https://github.com/wifecooky/thewang-blog/commits/main/

https://github.com/facebook/docusaurus/issues/9715
-->
