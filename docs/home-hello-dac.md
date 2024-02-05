<!-- 首页 Home Page -->

# "Hello DaC!"

:在DACREPO目录下，使用terminal运行本地服务器docsify serve，就可以在浏览器中预览网站 http://localhost:3000 ，命令行：docsify serve docs

​	最近接触到“文档即代码”管理理念（Document as Code），于是慢慢着手建立一个在线的个人知识管理仓库（PKM Repo, Personal Knowledge Management Repository），持续优化中。

​	“～/docs/repo”：中存放所有的public的在线文档。

​	技术选择：

- git：本地分布式版本控制工具；

- github：远程分布式版本控制工具；

- docsify：在线文档发布工具；

- vscode：本地编辑器；

  

接下来是一些“流水账”式的过程记录：

## （零）DaC “文档即代码” 管理理念

​	在这种模式下，文件和配置管理任务不是通过手动过程来完成，而是通过编写代码来自动化。这样做的好处包括：

1. **版本控制**：通过使用像 Git 这样的版本控制系统，可以追踪和回滚配置的更改。
2. **一致性和标准化**：代码化的方法确保了配置的一致性和可重复性。
3. **自动化**：代码化允许自动化许多常规任务，提高效率并减少人为错误。
4. **文档化**：代码本身及其注释可以作为配置的文档。



## （一）技术选型

- 同型产品：docsify，Hexo，Wordpress。

- 部署方式：宝塔一键部署，特点是快捷方便，但我想自己手动走一遍过程，所以不用。

- 其实用docsify+github虽然前期慢一点但应感比较简单吧，后面再加私云，而且内容都在GitHub上应该会很好迁移，只是需要重新配置一遍就好，以及我真的想说动手很快乐，睡够也很重要。

- 最后暂时选型：docsify + github pages.

- 因为现阶段重点是先上线，然后再逐渐调整自己的仓库内的的目录内容结构。



## （二）对 Docsify 开始了解

-- 大概知道怎么一动手发现好好玩啊：

- `index.html`为入口文件，`css`、`js`以及配置项都在此文件中修改。
- `README.md`会作为默认主页内容渲染。
- `.nojekyll`用于阻止`GitHub Pages`忽略掉下划线开头的文件。

接着调用`docsify serve`命令然后访问`http://localhost:3000`即可进行本地预览；


## （三）在 Docsify 部署文档目录结构

- 本阶段目的是：顺利的通过 docsify 部署显示本地 markdown 文档的目录结构；























