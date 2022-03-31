---
title: Writing a New Post
author: cotes
date: 2019-08-08 14:10:00 +0800
categories: [jekyll 포스팅 튜토리얼 번역]
tags:
render_with_liquid: false
---

이 게시물은 _Chirpy_ 테마에 대한 게시물을 작성하는 방법을 안내합니다.    
이전에 _Jekyll_을 사용한 경험이 있더라도 이 기사는 읽을 가치가 있습니다. 많은 기능에서 특정 변수를 설정해야 하기 때문입니다.   

## 이름 지정 및 경로   



`YYYY-MM-DD-TITLE.EXTENSION`{: .filepath}라는 이름의 파일을 생성하고.    
블로그 폴더 내의  `_posts`{: .filepath} 폴더에 집어넣습니다.   

파일의 확장자는 .md나 .markdown 이어야 합니다.   
파일 생성 시간을 절약하려면 이 플러그인 [`Jekyll-Compose`](https://github.com/jekyll/jekyll-compose) 을 사용하여 이를 수행하는 것이 좋습니다.   

## 머리말   

기본적으로 게시물 상단에 다음과 같은 [머리말](https://jekyllrb.com/docs/front-matter/) 을 작성해야 합니다.   

```yaml
---
title: 타이틀
date: YYYY-MM-DD HH:MM:SS +/-TTTT
categories: [카테고리1, 카테고리2]
tags: [태그1]     # 태그 이름은 소문자여야합니다
---
```

주의 점!
> 게시물의 _레이아웃_은 기본적으로 설정되어 있으므로 머리말 블록에 `layout : post` 를 추가할 필요가 없습니다!
{: .prompt-tip }
}


### 날짜의 시간대

게시물의 릴리즈 날짜를 정확히 기록하려면 `_config.yml`{: .filepath}} 파일의 `timezone`설정을 해야합니다. 한국은 `Asia/Soul` 입니다.   
또한 게시물의 타임존을 머리말 블록의 `date`에 제공합니다. 형식은 `+/-TTTT` 예를들어 `+0800` 같이   

### 카테고리와 태그

각 `categories`포스트의 요소는 최대 2개의 요소를 포함하도록 설계되었으며 요소의 `tags`수는 0에서 무한대일 수 있습니다.    
예를 들어:


```yaml
---
categories: [Animal, Insect]
tags: [bee]
---
```

### 작성자 정보

The author information of the post usually does not need to be filled in the _Front Matter_ , they will be obtained from variables `social.name` and the first entry of `social.links` of the configuration file by default. But you can also override it as follows:
게시물의 작성자 정보는 일반적으로 _머리말_에 입력할 필요가 없으며 기본적으로 `_config.yml` 파일의 `social.name` 변수와 `social.links`의 첫 번째 항목에서 가져옵니다.    

Add author information in `_data/authors.yml` (If your website doesn't have this file, don't hesitate to create one.)
`_data/authors.yml` 에 작성자 정보를 추가하는 것을 통해 재정의 가능합니다 (웹 사이트에 이 파일이 없으면 주저하지 말고 작성하세요!)

```yaml
<author_id>:
  name: <full name>
  twitter: <twitter_of_author>
  url: <homepage_of_author>
```
{: file="_data/authors.yml" }

그런 다음 게시물의 YAML 블록에서 사용자 지정 작성자를 설정합니다

```yaml
---
author: <author_id>
---
```

> Another benefit of reading the author information from the file `_data/authors.yml`{: .filepath } is that the page will have the meta tag `twitter:creator`, 
which enriches the [Twitter Cards](https://developer.twitter.com/en/docs/twitter-for-websites/cards/guides/getting-started#card-and-content-attribution) and is good for SEO.
{: .prompt-info }

> `_data/authors.yml`{: .filepath } 파일에서 작성 정보를 읽는 방식의 장점은 페이지가 `twitter:creator` 메타 태그를 가지게 되어 [Twitter Cards](https://developer.twitter.com/en/docs/twitter-for-websites/cards/guides/getting-started#card-and-content-attribution)
를 풍부하게 하고 검색엔진 최적화에 좋다는 것입니다.


## 목차 만들기

By default, the **T**able **o**f **C**ontents (TOC) is displayed on the right panel of the post. If you want to turn it off globally,
go to `_config.yml`{: .filepath} and set the value of variable `toc` to `false`. 
If you want to turn off TOC for a specific post, add the following to the post's [Front Matter](https://jekyllrb.com/docs/front-matter/):

목차는 오른쪽 패널에 표시됩니다.   
1.  블로그 전체에서 목차를 끄려면 `_config.yml`에서 아래와 같이 `toc` 값을 `false` 로 수정합니다.
2. 특정 게시물에서만 목차를 끄려면 _머릿말_ 에 아래 코드와 같이 씁니다. 

```yaml
(머릿말)
---
toc: false
---
```

## 댓글

The global switch of comments is defined by variable `comments.active` in the file `_config.yml`{: .filepath}. After selecting a comment system for this variable, comments will be turned on for all posts.

If you want to close the comment for a specific post, add the following to the **Front Matter** of the post:


```yaml
---
comments: false
---
```

## Mathematics

For website performance reasons, the mathematical feature won't be loaded by default. But it can be enabled by:

```yaml
---
math: true
---
```

## Mermaid

[**Mermaid**](https://github.com/mermaid-js/mermaid) is a great diagrams generation tool. To enable it on your post, add the following to the YAML block:

```yaml
---
mermaid: true
---
```

Then you can use it like other markdown languages: surround the graph code with ```` ```mermaid ```` and ```` ``` ````.

## Images

### Caption

Add italics to the next line of an image，then it will become the caption and appear at the bottom of the image:

```markdown
![img-description](/path/to/image)
_Image Caption_
```
{: .nolineno}

### Size

In order to prevent the page content layout from shifting when the image is loaded, we should set the width and height for each image:

```markdown
![Desktop View](/assets/img/sample/mockup.png){: width="700" height="400" }
```
{: .nolineno}

Starting from _Chirpy v5.0.0_, `height` and `width` support abbreviations (`height` → `h`, `width` → `w`). The following example has the same effect as the above:

```markdown
![Desktop View](/assets/img/sample/mockup.png){: w="700" h="400" }
```
{: .nolineno}

### Position

By default, the image is centered, but you can specify the position by using one of the classes `normal`, `left`, and `right`.

> Once the position is specified, the image caption should not be added.
{: .prompt-warning }

- **Normal position**

  Image will be left aligned in below sample:

  ```markdown
  ![Desktop View](/assets/img/sample/mockup.png){: .normal }
  ```
  {: .nolineno}

- **Float to the left**

  ```markdown
  ![Desktop View](/assets/img/sample/mockup.png){: .left }
  ```
  {: .nolineno}

- **Float to the right**

  ```markdown
  ![Desktop View](/assets/img/sample/mockup.png){: .right }
  ```
  {: .nolineno}

### Shadow

The screenshots of the program window can be considered to show the shadow effect, and the shadow will be visible in the `light` mode:

```markdown
![Desktop View](/assets/img/sample/mockup.png){: .shadow }
```
{: .nolineno}

### CDN URL

If you host the images on the CDN, you can save the time of repeatedly writing the CDN URL by assigning the variable `img_cdn` of `_config.yml`{: .filepath} file:

```yaml
img_cdn: https://cdn.com
```
{: file='_config.yml' .nolineno}

Once `img_cdn` is assigned, the CDN URL will be added to the path of all images (images of site avatar and posts) starting with `/`.

For instance, when using images:

```markdown
![The flower](/path/to/flower.png)
```
{: .nolineno}

The parsing result will automatically add the CDN prefix `https://cdn.com` before the image path:

```html
<img src="https://cdn.com/path/to/flower.png" alt="The flower">
```
{: .nolineno}

### Image Path

When a post contains many images, it will be a time-consuming task to repeatedly define the path of the images. To solve this, we can define this path in the YAML block of the post:

```yml
---
img_path: /img/path/
---
```
{: .nolineno }

And then, the image source of Markdown can write the file name directly:

```md
![The flower](flower.png)
```
{: .nolineno }

The output will be:

```html
<img src="/img/path/flower.png" alt="The flower">
```
{: .nolineno }

### Preview Image

If you want to add an image to the top of the post contents, specify the attribute `path`, `width`, `height`, and `alt` for the image:

```yaml
---
image:
  path: /path/to/image/file
  width: 1000   # in pixels
  height: 400   # in pixels
  alt: image alternative text
---
```

Except for `alt`, all other options are necessary, especially the `width` and `height`, which are related to user experience and web page loading performance. The above section "[Size](#size)" also mentions this.

Starting from _Chirpy v5.0.0_, the attributes `height` and `width` can be abbreviated: `height` → `h`, `width` → `w`. In addition, the [`img_path`](#image-path) can also be passed to the preview image, that is, when it has been set, the  attribute `path` only needs the image file name.

## Pinned Posts

You can pin one or more posts to the top of the home page, and the fixed posts are sorted in reverse order according to their release date. Enable by:

```yaml
---
pin: true
---
```

## Prompts

There are several types of prompts: `tip`, `info`, `warning`, and `danger`. They can be generated by adding the class `prompt-{type}` to the blockquote. For example, define a prompt of type `info` as follows:

```md
> Example line for prompt.
{: .prompt-info }
```
{: .nolineno }

## Syntax

### Inline Code

```md
`inline code part`
```
{: .nolineno }

### Filepath Hightlight

```md
`/path/to/a/file.extend`{: .filepath}
```
{: .nolineno }

### Code Block

Markdown symbols ```` ``` ```` can easily create a code block as follows:

````md
```
This is a plaintext code snippet.
```
````

#### Specifying Language

Using ```` ```{language} ```` you will get a code block with syntax highlight:

````markdown
```yaml
key: value
```
````

> The Jekyll tag `{% highlight %}` is not compatible with this theme.
{: .prompt-danger }

#### Line Number

By default, all languages except `plaintext`, `console`, and `terminal` will display line numbers. When you want to hide the line number of a code block, add the class `nolineno` to it:

````markdown
```shell
echo 'No more line numbers!'
```
{: .nolineno }
````

#### Specifying the Filename

You may have noticed that the code language will be displayed at the top of the code block. If you want to replace it with the file name, you can add the attribute `file` to achieve this:

````markdown
```shell
# content
```
{: file="path/to/file" }
````

#### Liquid Codes

If you want to display the **Liquid** snippet, surround the liquid code with `{% raw %}` and `{% endraw %}`:

````markdown
{% raw %}
```liquid
{% if product.title contains 'Pack' %}
  This product's title contains the word Pack.
{% endif %}
```
{% endraw %}
````

Or adding `render_with_liquid: false` (Requires Jekyll 4.0 or higher) to the post's YAML block.

## Learn More

For more knowledge about Jekyll posts, visit the [Jekyll Docs: Posts](https://jekyllrb.com/docs/posts/).
