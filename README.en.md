介绍
des 链接：https : //www.softwaretestinghelp.com/github-rest-api-tutorial/ GitHub REST API - 以编程方式与 GitHub 交互的接口：

在我们早期的 GitHub 教程中，我们使用 Web 界面从开发人员的角度探讨了使用的各个方面。

今天，大多数组织一直在寻找几乎每个领域的自动化机会，而 REST API 对于不同工具的各种场景自动化非常有用。

当然，可能还有其他领域可以使用 REST API。

=> 访问此处获取独家 GitHub 培训教程系列。

GitHub REST API

你将学到什么：[隐藏]

GitHub REST API 集成 创建个人访问令牌 存储库 合作者 组织 分支 拉取请求 标签、里程碑和问题 团队 搜索存储库、代码、问题发布 结论 推荐阅读 GitHub REST API 集成 REST API（代表性状态传输）主要使用 HTTP 请求来执行以下操作.

GET – 检索资源 PUT/PATCH – 更新资源 POST – 创建资源 DELETE – 删除资源 我们不会深入探讨 REST API 的工作原理，而是使用 CURL 命令直接跳转到 GitHub 中的 REST API 支持以执行大部分我们在之前的 GitHub 教程中通过 REST API 看到的任务。

GitHub API 的当前版本是 v3，本教程涵盖了开发人员通过这些 API 需要的最重要的活动。

创建个人访问令牌 为了让 REST API 通过命令行工作，我们需要对 GitHub 服务器进行身份验证。因此，我们需要提供我们的凭据。好吧，我们不想暴露我们的 GitHub 帐户使用的密码，因此我们将生成一个个人访问令牌，以与命令行一起使用以对 GitHub 进行身份验证。

登录到您的 GitHub 帐户，然后单击您的个人资料下的设置。

设置

转到开发人员设置 -> 个人访问令牌。生成新令牌。

个人访问令牌

添加名称并选择 API 访问范围，然后单击创建令牌。

创建令牌

在下一个屏幕中，确保复制令牌并将其保存在文件中。此令牌将在命令行中用于访问 GitHub API。

复制令牌并保存

当要求输入密码时，创建的令牌也可以在 git clone 操作期间使用。现在，由于我们已经有了令牌，我们将看到如何使用 CURL 程序从命令行访问 API。

作为先决条件，您需要下载并安装“curl”。

存储库 此处显示的 REST API 示例在 Windows 机器上运行。本节将展示一些 GitHub 存储库操作。

#1) 要列出用户的公共存储库，请在一行中运行以下命令。

curl -X GET -u : https://api.github.com/users/ /repos | grep -w clone_url

#2) 列出组织下的公共存储库。

curl -X GET -u : https://api.github.com/orgs/ /repos | grep -w clone_url

#3) 创建个人存储库。

curl -X POST -u : https://api.github.com/user/repos -d “{\”name\”: \”Demo_Repo\”}”

在上面的命令名称是一个参数。让我们看看在创建个人用户存储库时可以使用的其他一些参数。

curl -X POST -u : https://api.github.com/user/repos -d “{\”name\”: \”Demo_Repo\”,\”description\”: \”这是第一个通过API的repo \”,\”主页\”: \” https://github.com\”,\”public\” : \”true\”,\”has_issues\”: \”true\”,\”has_projects\” :\”true\”,\”has_wiki\”: \”true\”}”

在上面的命令中，name、description、homepage、public、has_projects、has_wiki都是取字符串值的参数，用\"括起来。另请注意，在 : 和 \ 之间有一个空格

例如，public 参数使 repo 公开。该命令还可以创建问题、项目、wiki。

#4) 重命名存储库。

curl -X POST -u : -X PATCH -d “{\”name\”:\”\”}” https://api.github.com/repos/ /

#5) 更新存储库中的 has_wiki 参数并将值设置为 false。

curl -u :-X PATCH -d “{\”has_wiki\”:\”false\”}” https://api.github.com/repos/user-name/

#6) 删除存储库。

curl -X 删除：https : //api.github.com/repos//

#7) 在组织中创建存储库。

curl -X POST -u : https://api.github.com/orgs/ /repos“{\”name\”: \”Demo_Repo_In_Org\”,\”description\”: \”这是org中的第一个repo通过API\”,\”homepage\”: \” https://github.com\”,\”public\” : \”true\”,\”has_issues\”: \”true\”,\”has_projects\ ”:\”true\”,\”has_wiki\”:\”true\”}”

在组织中创建存储库

#8) 列出存储库的分叉。

curl -X GET -u : https://api.github.com/repos/ //forks | grep -w html_url

上面的命令将列出 URL 以浏览分叉的 repo。在用户存储库和“Insights TAB => Forks”下可以看到相同的内容。

列出存储库的分支

curl -X GET -u : https://api.github.com/repos/ //forks | grep -w clone_url

上面的命令将列出克隆分叉存储库的 URL。

#9) 在组织中创建一个存储库。

curl -X POST -u :-d “{\”organization\”: \”\”}” https://api.github.com/repos/ //forks

协作者 #1) 列​​出存储库的协作者。

curl -X GET -u : https://api.github.com/repos/ //合作者 | grep -w 登录

#2) 检查用户是否在协作者列表中。

curl -X GET -u : https://api.github.com/repos/ //collaborators/

如果用户是协作者的一部分，则不会显示任何内容作为输出，否则会显示以下消息。

{ “消息”：“不是用户”，“documentation_url”：“ https://developer.github.com/v3/repos/collaborators/#get” }

#3) 检查用户的权限。

curl -X GET -u : https://api.github.com/repos/ //collaborators/<user-name-to-check–for-permission>/permission| grep -w 权限

#4) 将用户作为协作者添加到存储库。

curl -X PUT -u : https://api.github.com/repos/ //collaborators/

发布此内容后，受邀者将需要接受邀请才能以协作者身份加入。如果已将用户添加为协作者，则不显示任何内容，否则将显示输出。

#5) 将用户移除为协作者。

curl -X DELETE: https://api.github.com/repos/ // 合作者/

命令运行成功后不显示任何内容。

组织 注意：GitHub API 不提供创建组织。

#1) 列​​出用户的所有组织帐户。

curl -X GET -u：https ://api.github.com/repos/user/orgs | grep -w 登录

#2) 更新组织。

curl -X PATCH -u :-d “{\”name\”: \”TeamVN\”,\”billing_email\”: \” vniranjan72@outlook.com \”,\”email\”: \” vniranjan72@outlook .com \”,\”location\”:\”Bangalore\”,\”\”description\”: \”更新组织详情\”}” https://api.github.com/orgs/

分支 #1) 列​​出用户存储库中的分支。该命令将列出存储库中的所有分支。

curl -X GET -u : https://api.github.com/repos/ //分支 | grep -w 名字

#2) 列出用户存储库中所有受保护的分支。

curl -X GET -u : https://api.github.com/repos/ //branches?protected=true | grep -w 名字

#3) 列出用户存储库中所有未受保护的分支

curl -X GET -u : https://api.github.com/repos/ //branches?protected=false | grep -w 名字

#4) 移除分支保护。

curl -X DELETE: https://api.github.com/repos/ // 分支/主/保护

拉取请求 #1) 列​​出拉取请求。

curl -X GET -u : https://api.github.com/repos/ //pulls?state=open | grep -w 标题

state 参数的选项是 Open、Closed、All。

#2) 创建一个拉取请求。

curl -X POST -u :-d "{\"title\":\"Great feature added\",\"body\": \"请将所做的重大更改拉到 master 分支\",\"head\ ”: \”feature\”,\”base\”: \”master\”}” https://api.github.com/repos/ //pulls

创建拉取请求

#3) 列出创建的拉取请求的数量。

curl -X GET -u : https://api.github.com/repos/ //pulls?state=open | grep -w 数字

#4) 更新拉取请求正文或任何其他参数（最多 250 次提交）。

curl -X PATCH -u :-d “{\”body\”: \”强制将功能分支中所做的巨大更改拉到主分支\”}” https://api.github.com/repos/ //拉/ 31

#5) 列出拉取请求提交。

curl -X GET -u : https://api.github.com/repos/ //pulls/31/commits

#6) 列出拉取请求文件（最多 300 个文件）。

curl -X GET -u : https://api.github.com/repos/ //pulls/31/files| grep -w 文件名

#7) 合并拉取请求。

curl -X PUT -u :-d “{\”commit_message\”: \”Good Commit\”}” https://api.github.com/repos/ //pulls/31/merge

合并时的响应

{ “sha”: “e5db2ce465f48ada4adfb571cca2d6cb859a53c6”, “merged”: true, “message”: “拉取请求成功合并”}

拉取请求无法合并时的响应

{ “消息”：“拉取请求不可合并”，“documentation_url”：“ https://developer.github.com/v3/pulls/#merge-a-pull-request-merge-button” }

标签、里程碑和问题标签

#1) 列​​出存储库中的所有标签。

curl -X GET -u : https://api.github.com/repos/ //标签 | grep -w 名字

#2) 列出存储库中的特定标签。

curl -X GET -u : https://api.github.com/repos/ //labels/bug

#3) 创建标签。

curl -X POST -u :-d “{\”name\”: \”defect\”,\”description\”: \”提出缺陷\”,\”color\”: \”ff493b\”} ” https://api.github.com/repos/ //标签

颜色参数的十六进制颜色代码可以从 Color-hex 设置

#4) 更新标签

curl -X PATCH -u : -d “{\”color\”: \”255b89\”}” https://api.github.com/repos/ //labels/defect

#5) 删除标签

curl -X 删除：https ://api.github.com/repos/vniranjan1972/Demo_Project_Repo_VN/labels/defect

问题

#6) 列出存储库中的特定问题。

curl -X GET -u : https://api.github.com/repos/ //issues/20 | grep -w 标题

#7) 列出存储库中的所有问题。

curl -X GET -u : https://api.github.com/repos/ //问题 | grep -w 标题

#8) 创建一个问题。

curl -X POST -u :-d "{\"title\":\"新建欢迎页面\",\"body\":\"要设计一个新页面\",\"labels\": [\"增强\”],\”里程碑\”: \”3\”,\”assignees\”: [\”\”,\”<user-name2\”],\”state\”: \”open\” }” https://api.github.com/repos/ //问题

在上述命令中，标签和受让人参数是可以提供多个值的字符串数组。状态参数将具有打开或关闭的值。

#9) 为问题添加标签。

curl -X POST -u : -d “{\”labels\”: [\”enhancement\”]}” https://api.github.com/repos/ //issues/30/labels

#10) 编辑问题并更新参数，例如，标签。

curl -X PATCH -u :-d “{\”labels\”: [\”bug\”,\”enhancement\”]}” https://api.github.com/repos/ //issues/30

在上述命令中，更新问题编号 30 的标签。

#11) 从特定问题中删除标签。

curl -X DELETE: https://api.github.com/repos/ //问题/30/标签/错误

#12) 从特定问题中删除所有标签。

curl -X DELETE: https://api.github.com/repos/ // 问题/30/标签

里程碑

#13) 列出所有里程碑。

curl -X GET -u :-d “{\”state\”: [\”open\”]}” https://api.github.com/repos/ //里程碑 | grep -w 标题

#14) 列出特定里程碑的详细信息。

curl -X GET -u : https://api.github.com/repos/ //里程碑/1 | grep -w 标题

#15) 创建一个里程碑。

curl -X POST -u :-d “{\”title\”: \”R5\”,\”state\”: \”open\”,\”description\”: \”里程碑 R5\”, \”due_on\”: \”2019-12-05T17:00:01Z\”}” https://api.github.com/repos/ //里程碑

在上述命令中，due_on 是 YYYY-MM-DDTHH:MM:SSZ 格式的时间戳 ISO 8601。可以在@ISO 8601 中找到更多相关信息

#16) 更新里程碑。

curl -X PATCH -u :-d “{\”state\”: \”closed\”}” https://api.github.com/repos/ //milestones/3

#17) 删除​​里程碑。

curl -X DELETE -u: https://api.github.com/repos/ // 里程碑 / 3

团队 #1) 列​​出组织中的团队。

curl -X GET -u : https://api.github.com/orgs/ /teams| grep -w 名字

按团队 ID 列出

curl -X GET -u : https://api.github.com/orgs/ /teams| grep -w id

#2) 按用户列出团队。

curl -X GET -u：https ://api.github.com/user/teams | grep -w 名字

#3) 创建团队，添加成员并将存储库添加到团队。

curl -X POST -u :-d “{\”name\”:\”\”,\”description\”:\”输入简要说明\”,\”maintainers\”: [\”\”],\ “repo_names\”: [\”/\”]}” https://api.github.com/orgs/Demo-Proj-Org/teams

#4) 编辑团队名称和描述。

curl -X PATCH -u :-d “{\”name\”: \”New Team Name\”,\”description\”: \”Latest Description\”}” https://api.github.com/teams /

可以通过运行步骤 1 中的命令来检索团队 ID。

#5) 将存储库添加到现有团队..

curl -X PUT -u：https ://api.github.com/teams//repos//

#6) 从团队中删除存储库。

curl -X DELETE: https://api.github.com/teams/ <Team-Id / repos //

#7) 删除​​一个团队。

curl -X 删除：https ://api.github.com/teams/

搜索存储库、代码、问题 搜索 API 允许搜索任何项目。

#1) 例如，如果您想搜索特定用户拥有的所有存储库。

curl -X GET https://api.github.com/search/repositories?q=user : | grep -w “名字”

必需的参数是 q，包含由关键字和限定符组成的搜索条件，以限制在 Github 中特定区域的搜索。

#2) 搜索 README 文件中包含单词 V 和 Niranjan 的特定用户拥有的所有存储库

curl -X GET https://api.github.com/search/repositories?q=V+Niranjan+in:readme+user :| grep -w 名字

#3) 在文件内容中搜索关键字。在下面的示例中，在用户拥有的存储库中的文件中搜索关键字“System”和“addEmployee”。

curl -X GET https://api.github.com/search/code?q=System+addEmployee+in:file+language:java+repo :/ | grep -w 名字

#4) 在未解决的问题中搜索关键字“欢迎”并标记为增强。

curl -X GET https://api.github.com/search/issues?q=welcome+label:enhancement+state:open+repo :/| grep -w 名字

#5) 在已关闭的问题中搜索关键字“地址”并标记为增强。

curl -X GET https://api.github.com/search/issues?q=address+label:enhancement+state:closed+repo :/ | grep -w 名字

发布 #1) 按标签名称和 ID 列出存储库中的发布。

curl -X GET -u : https://api.github.com/repos/ //发布 | grep -w 标签名

curl -X GET -u : https://api.github.com/repos/ //发布 | grep -w id

#2) 获取单个版本的详细信息。

curl -X GET -u : https://api.github.com/repos/ //releases/ | grep -w 标签名

curl -X GET -u : https://api.github.com/repos/ //releases/ | grep -w 正文

curl -X GET -u : https://api.github.com/repos/ //releases/ | grep -w 名字

#3) 获取最新版本的详细信息。

curl -X GET -u : https://api.github.com/repos/ //releases/latest| grep -w 标签名

curl -X GET -u : https://api.github.com/repos/ //releases/latest| grep -w 名字

curl -X GET -u : https://api.github.com/repos/ //releases/latest| grep -w 正文

#4) 通过标签获取发布细节。

curl -X GET -u : https://api.github.com/repos/ //releases/tags/| grep -w 名字

curl -X GET -u : https://api.github.com/repos/ //releases/tags/| grep -w 正文

#5) 创建一个版本。

curl -X POST -u :-d “{\”tag_name\”: \”R3.0\”,\”target_commitish\”: \”master\”,\”name\”: \”Release 3.0\”, \"body\": \"This is for Release 3.0 of the product\",\"draft\": "false",\"prerelease\": "false"}" https://api.github.com/ repos/ /<repo-name/releases

注意：在创建发布的命令中，参数“draft”和“prerelease”采用布尔值。输入 true 或 false，不带 \"。

草稿值 false 表示已创建已发布版本，为 true 表示未发布版本。Prerelease false 表示它是完整版本。真正的价值意味着它是一个预发布版本。#6) 编辑或更新版本。

curl -X PATCH-u :-d “{\”tag_name\”: \”R3.1\”}” https://api.github.com/repos/ /<repo-name/releases/

#7) 删除​​版本。

curl -X DELETE: https://api.github.com/repos//<repo-name/releases/

#8) 列出发布的资产。

curl -X DELETE: https://api.github.com/repos/ / <repo-name/releases // 资产

结论 在本 GitHub REST API 教程中，我们了解了 REST API 如何用于获取、放置、发布、修补、删除数据的各种操作。

用于 REST API 直接与 GitHub.com 一起工作的 URL 是https://api.github.com。然而，如果团队在他们的组织中使用 GitHub 企业，那么与 REST API 一起使用的 URL 将是 https:///api/v3

到目前为止，本系列中的所有教程都集中于从开发人员的角度使用 GitHub，以及在团队中直接在 GitHub 上而不是在本地对各种类型的工件进行版本控制时协作的最佳实践。

我们即将发布的教程将重点介绍开发人员如何使用 GitHub Desktop 和 TortoiseGit 等 Git 客户端界面在从 GitHub 克隆的本地存储库上脱机工作，并将更改推送回远程存储库。

=> 访问这里从头开始学习 GitHub。
