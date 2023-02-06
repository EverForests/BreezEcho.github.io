# 博客维护日志-230206


  <!--more-->

## 1. encrypt加密

1. 找到`themes\LoveIt\layouts\posts\single.html`文件,在`{{- $params := .Scratch.Get "params" -}}`的下一行添加下列内容：

```html
{{- $password := $params.password | default "" -}}
{{- if ne $password "" -}}
	<script>
		(function(){
			if({{ $password }}){
				if (prompt('请输入文章密码') != {{ $password }}){
					alert('密码错误！');
					if (history.length === 1) {
						window.opener = null;
						window.open('', '_self');
						window.close();
					} else {
						history.back();
					}
				}
			}
		})();
	</script>
{{- end -}}
```

2. 之后只要在文章的头部加上password属性即可进行加密，只有输入了正确密码才能打开文章，否则会回退到之前的页面。用法如下：

```toml
---
title: 随笔
password: test
---
```

## 2. 博客主题更新

在博客根目录下进行如下操作：

```bash
git clone https://github.com/khusika/FeelIt.git themes/FeelIt
```

在themes文件目录下会生成一个名为FeelIt的主题文件。

为了同步更新主题文件，可以将FeelIt设置为网站目录的子模块。

```bash
git submodule add https://github.com/khusika/FeelIt.git themes/FeelIt
```

之后在`themes/FeelIt/content/examplesite`目录下找到主题的配置文件，为了更好地维护，建议将其作为网站根目录的配置文件，即将本主题设置为默认主题。不要在主题文件中进行配置，以防配置更新导致数据丢失。

最后，可以将主题目录下的assets和layout文件夹直接复制到网站根目录下，保证展示效果。

若要进行展示，可以执行:

```bash
hugo serve --disableFastRender
```

不过由于修改了配置文件，而public文件并没有进行更新，所以，建议先删除原来的public文件，然后重新执行`hugo`命令来进行生成。

经过了以上操作，博客更新就大体完成了。

## 3. config.toml文件说明

初始的配置文件结构不太清晰，需要进行二次描述，做结构切分。

个人感觉语言切换这个功能目前不太常用，所以可以选择一个主语言，将其设置为默认状态，然后将其它语言的配置部分删除，注意保证留下语言和默认语言的一致性。

## 4. 网页角标设置

本主题使用的符号集为fontawesome，可以通过html引用相关图标，具体格式为：

```html
<i class='fas fa-img fa-fw'></i>
```

其中`fa-img`可以替换为你想要的图标名称，具体可在官网进行查询。不过需要注意的是，博客使用的本地字体集版本可能较低，图标引用语法与最新方式有所不同，所以需要以本地引用格式为准。

## 5. 备忘录

- 文章banner设置的图片标准为1600*840
- win11下的任务管理器快捷键为`ctrl+shift+esc`

