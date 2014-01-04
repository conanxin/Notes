翻译自：[ADDING A TABLE OF CONTENTS TO YOUR GITHUB WIKI](http://blackbe.lt/github-wiki-sidebar-table-contents-header-footer/)

你不仅可以给github wiki pages添加sidebar，你也可以定制header和footer。Github wiki pages是通过他们自己开源的git驱动的wiki引擎gollum实现的。

###Sidebar Files
Sidebar Files可以给你的wiki pages添加一个侧栏。它们遵循_Siderbar.ext的命名规则，其中.ext是你的wiki格式。Sidebar files会影响同一个目录下的所以pages，以及子目录下没有sidebar files的pages。

####如何添加一个Sidebar
1. 通过git，然后Checkout你的wiki。找到你的wiki页面，点击Git Access.
2. 将Github上的wiki内容pulled down到本地，创建一个新的文件，命名为_Sidebar.md。
3. 在这个新创建的_Sidebar.md文件，你可以添加适当的 [[link]]标记内容。
4. 添加新的文件git add _Sidebar.md，然后提交git commit _Sidebar.md -m "Adding new sidebar."，最后push到github上git push origin master。

####Header Files
