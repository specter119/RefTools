# 自定义宏批量修改 csl 生成参考文献书目的错误

鉴于 csl 还没有想好怎么支持多语言(见 [csl/schema#63](https://github.com/citation-style-language/schema/issues/63))，但 GB/T 7714-{1987,2005,2015} 都有多语言的需求，导致现在 csl 没法生成完全正确的参考文献书目。生成中错误较显著的，就是多英文作者(多于4位)省略以“等”结尾，实际应为“et al.”

> 实际问题还有很多，比如图书的版本，默认还是会写作“第 n 版”，同样与英文书不符。
>
> 但是相对而言，作者比较好匹配，行首+中英文紧邻使错误尤为突出。
>
> 同时，GB/T 7714-{1987,2005} 的 csl，完成度还不够高(csl 自述及研究过的同学反馈)，且最新版本 GB/T 7714-2015 (其实三个版本差距都不大) 还没有相匹配的 csl。真正一个完成度更高的 GB/T 7714-2015，需要正则修改的可能更多，也更难。
>
> GB/T 7714 里面一些规定也不合理，比如英文作者完全大写，最终效果也很丑。

## 1. 新建自定义宏

首先，先调出“开发工具”标签页。

[windows word 开发工具](https://support.office.com/zh-cn/article/%E6%98%BE%E7%A4%BA-%E5%BC%80%E5%8F%91%E5%B7%A5%E5%85%B7-%E9%80%89%E9%A1%B9%E5%8D%A1-e1192344-5e56-4d45-931b-e5fd9bea2d45)
[mac word 开发工具](https://support.office.com/zh-cn/article/%E5%9C%A8-word-2016-for-mac-%E4%B8%AD%E6%98%BE%E7%A4%BA-%E5%BC%80%E5%8F%91%E5%B7%A5%E5%85%B7-%E9%80%89%E9%A1%B9%E5%8D%A1-0c0778a2-fa91-4b75-9164-0685ae00e9b4)

然后，新建宏。

[新建或编辑宏](https://support.office.com/zh-cn/article/%E5%88%9B%E5%BB%BA%E6%88%96%E8%BF%90%E8%A1%8C%E5%AE%8F-c6b99036-905c-49a6-818a-dfb98b7c3c9c)

宏内容见下，且同步发布于 [GitGist/fix_csl_gbt7714](https://gist.github.com/specter119/ea9440c8573aa0df266ea87745226d37) 。

```vb
Sub deng2etal()
'
' deng2etal macro
' English 等 -> english, et al
'
    With Selection.Find
        .Forward = True
        .ClearFormatting
        .Text = "(<[A-z]@)等"
        With .Replacement
            .ClearFormatting
            .Text = "\1, et al"
        End With
        .Wrap = wdFindStop
        .Execute Replace:=wdReplaceAll, MatchWildcards:=True
    End With
End Sub
```

写宏参考 [知乎 @johnmy 相关问题回答][^johnmy] 写成，请务必不要修改 csl 的默认语言，以便生成书目均为“等”。

> 删除了之前版本包含的`etal2deng`，因为 csl 需要修改，且宏保存运行时可能出问题。
>
> 正则匹配中文看到了2个说法，[一-龥](见[Pinyin News 博文][^pinyin.info])和[⺀-￭](见[super user][^super_user])，在本人电脑上就出现了保存后 vba 再次打开就无法正常显示部分字符，且无法正常解析运行。
>
> 如果确实需要使用 `deng2etal`的功能，请尝试使用替换，以下2个方案应都可行（复制“”内的内容）。
> 1. "([⺀-￭]@)([, ]*)et al" -> "\1\2等"
> 1. "([一-龥]@)([, ]*)et al" -> "\1\2等"
>
> 如前文所言，即使运行了，还是书目会有其他个别错误，暂时没有把这个宏做的很长很丰富的计划。

## 2. 生成快速访问工具栏按钮

[为宏添加按钮](https://support.office.com/zh-cn/article/%E5%B0%86%E5%AE%8F%E5%88%86%E9%85%8D%E7%BB%99%E6%8C%89%E9%92%AE-728c83ec-61d0-40bd-b6ba-927f84eb5d2c#OfficeVersion=macOS)

## 3. 用法

如果没有选择区域，本文的宏会对光标后到文末进行替换，所以，替换前应选中所有书目或者将光标定位到“参考文献”标题位置。
点一下刚刚配置的快速访问工具栏按钮，即可。

## 参考

[^johnmy]: [Zotero 如何设置引文列表中的“等”和“et al”混排？- johnmy 的回答 - 知乎](https://www.zhihu.com/question/39156067/answer/145700137)
[^pinyin.info]: [How to find Chinese characters in an MS Word document](http://pinyin.info/news/2016/how-to-find-chinese-characters-in-an-ms-word-document/)
[^super_user]: [Visual Studio - Search through code for Chinese text](https://superuser.com/questions/983441/visual-studio-search-through-code-for-chinese-text)
