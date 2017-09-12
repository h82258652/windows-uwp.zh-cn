# <a name="contributing-to-uwp-conceptual-documentation"></a>向 UWP 概念性文档投稿

感谢你对通用 Windows 平台 (UWP) 文档的关注！ 感谢你向我们的文档提供反馈以及编辑和添加内容。

此页面介绍为开发人员文档投稿的基本步骤。

## <a name="public-and-private-repos"></a>公共和私有存储库

UWP 概念文档托管在两个不同的存储库中，这些存储库随后会合并并更新到单个站点：一个存储库用于来自所有人的投稿，另一个仅用于 Microsoft 员工。

如果你***不是*** Microsoft 员工，则使用[公共内容存储库](https://github.com/MicrosoftDocs/windows-uwp)。

如果你***是*** Microsoft 员工，则可以使用公共存储库或[私有内容存储库](https://cpubwin.visualstudio.com/_git/windows-uwp)。 员工可以通过在私有存储库中投稿来稍微加快实时推送更改的速度，或将[特定分支](https://review.docs.microsoft.com/en-us/windows-authoring-guide/uwp/conceptual/setup-local-repo-for-large-changes#what-branch-should-i-use-for-my-authoring)用于需要在将来的某个日期之前保密的更改。

## <a name="editing-topics-on-the-public-repo"></a>在公共存储库上编辑主题

我们已尝试尽可能简单地对现有文件进行编辑。 
- 如果你已处于存储库中，则只需导航到文件，然后单击**编辑**按钮。  
- 或者，如果你在浏览器中查看 Docs.microsoft.com 页面，请单击页面右上角的**编辑**按钮。 我们会将你重定向到存储库中的正确 Markdown 源文件，在其中可以单击**编辑**按钮。 

GitHub 会自动将官方存储库分叉到你的个人 GitHub 帐户中，在其中可以进行更改。 完成后，将拉取请求提交回“文档”分支。 创建拉取请求之后，UWP 文档团队的成员会审查你的更改。 如果你的请求已被接受，则更新会发布到 https://docs.microsoft.com/windows/。

只需几分钟便可以了解 Markdown 的基础知识。  若要开始使用，请查看[掌握 Markdown](https://guides.github.com/features/mastering-markdown/)。

## <a name="making-more-substantial-changes"></a>进行更重大更改

若要对现有文章进行重大更改、添加或更改图像或是投稿新文章，需要创建私有内容存储库的本地副本。 按照[我们 Windows 创作指南中的说明](https://review.docs.microsoft.com/en-us/windows-authoring-guide/uwp/conceptual/)进行操作。 如果尚未设置 GitHub 帐户和加入域的 Microsoft 别名，则[在此处开始](https://review.docs.microsoft.com/en-us/windows-authoring-guide/github-account)。

## <a name="using-issues-to-provide-feedback-on-uwp-conceptual-documentation"></a>使用问题提供有关 UWP 概念性文档的反馈

如果只想提供反馈而不是直接修改实际文档页面，则可以[在公共存储库中创建问题](https://github.com/MicrosoftDocs/windows-uwp/issues)。 单击“问题”选项卡，然后单击**新问题**按钮。 请务必包含主题标题和页面的 URL。

UWP 文档团队的成员会定期审查问题，并且对它们进行会审、分配和相应处理。

*对于内部问题，请使用 [http://aka.ms/pubrequest](http://aka.ms/pubrequest) 上的 WDG 内容请求工具。 
