# github_API_gitee_API

#### 介绍
des
link:https://www.softwaretestinghelp.com/github-rest-api-tutorial/
GitHub REST API – An Interface To Interact Programmatically With GitHub:

In our earlier tutorials on GitHub, we explore the various aspects of usage from a developer perspective using the web interface.

Today, most of the organizations have been looking at automation opportunities in almost every area and REST APIs have been useful for automating various scenarios for different tools.

Of course, there could be other areas as well where REST API’s could be used.

=> Visit Here For The Exclusive GitHub Training Tutorial Series.


GitHub REST API



What You Will Learn: [hide]

GitHub REST API Integration
Creating A Personal Access Token
Repository
Collaborators
Organization
Branches
Pull Requests
Labels, Milestones & Issues
Teams
Search Repositories, Code, Issues
Releases
Conclusion
Recommended Reading
GitHub REST API Integration
REST APIs (Representational State Transfer) primarily use HTTP requests to do the following.

GET – Retrieve the resource
PUT/PATCH – Update resource
POST – Create a resource
DELETE – Delete resource
We will not dive deep into how REST API’s work, rather we will directly jump into REST API support in GitHub using the CURL command to perform most of the tasks that we saw in our previous tutorials on GitHub through REST API’s.

The current version of GitHub API is v3 and this tutorial covers the most important activities that a developer would need through these APIs.

Creating A Personal Access Token
For REST APIs to work through the command line, we need to authenticate to the GitHub server. Hence, we need to provide our credentials. Well, we don’t want to expose our password used with our GitHub account, thus we will generate a personal access token to be used with the command line to authenticate to GitHub.

Log in to your GitHub account and click on Settings under your profile.

Settings

Go to Developer Settings ->Personal Access Tokens. Generate a new token.

Personal access tokens

Add a name and select the scope for the API access and click on Create Token.

Create Token

In the next screen, make sure to copy the token and save it in a file. This token will be used in the command line to access GitHub API.

Copy the token and save it

The token created can also be used during the git clone operation when asked for a password. Now, as we have the token in place, we will see how to access the API from the command line using the CURL program.

As a pre-requisite, you will need to download and install ‘curl’.

Repository
The REST API’s examples shown here are run on the Windows machine. This section will showcase some of the GitHub Repository operations.

#1) To list Public Repositories for a user, run the following command in a single line.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/users/<user-name>/repos | grep -w clone_url

#2) To list Public Repositories under an organization.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/orgs/<Org-Name>/repos | grep -w clone_url

#3) Create a Personal Repository.

curl -X POST -u <UserName>:<Generated-Token>https://api.github.com/user/repos -d “{\”name\”: \”Demo_Repo\”}”

In the above command name is a parameter. Let’s look at some other parameters that can be used while creating personal user repositories.

curl -X POST -u <UserName>:<Generated-Token>https://api.github.com/user/repos -d “{\”name\”: \”Demo_Repo\”,\”description\”: \”This is first repo through API\”,\”homepage\”: \”https://github.com\”,\”public\”: \”true\”,\”has_issues\”: \”true\”,\”has_projects\”:\”true\”,\”has_wiki\”: \”true\”}”

In the above command, name, description, homepage, public, has_projects, has_wiki are all parameters that take a string value and are enclosed in \”. Also note that there is a SPACE between : and \

For Example, public parameter makes the repo public. The command also enables issues, projects, wikis to be created.

#4) Rename the Repository.

curl -X POST -u <UserName>:<Generated-Token> -X PATCH -d “{\”name\”:\”<NewRepoName>\”}” https://api.github.com/repos/<user-name>/<OldRepoName>

#5) Update the has_wiki parameter in the repository and set the value to false.

curl -u <UserName>:<Generated-Token>-X PATCH -d “{\”has_wiki\”:\”false\”}” https://api.github.com/repos/user-name/<reponame>

#6) Delete the Repository.

curl -X DELETE -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<reponame>

#7) Create a Repository in an Organization.

curl -X POST -u <UserName>:<Generated-Token>https://api.github.com/orgs/<Enter-Org-name>/repos“{\”name\”: \”Demo_Repo_In_Org\”,\”description\”: \”This is first repo in org through API\”,\”homepage\”: \”https://github.com\”,\”public\”: \”true\”,\”has_issues\”: \”true\”,\”has_projects\”:\”true\”,\”has_wiki\”: \”true\”}”

Create a repository in an Organization

#8) List Forks for a Repository.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<User-Repo>/forks | grep -w html_url

The above command will list the URL to browse the forked repo. The same can be seen under the user repository and ‘Insights TAB =>Forks’.

List forks for a repository

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<User-Repo>/forks | grep -w clone_url

The above command will list the URL to clone the forked repo.

#9) Fork a Repository in the organization.

curl -X POST -u <UserName>:<Generated-Token>-d “{\”organization\”: \”<Org-Name-To-Fork>\”}” https://api.github.com/repos/<user-name>/<repo-name>/forks

Collaborators
#1) List Collaborators for a Repository.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/collaborators | grep -w login

#2) Check if a user is in the Collaborator list.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/collaborators/<user-name-to-check>

If the user is a part of collaborator, then there is no content displayed as output else the following message is displayed.

{
“message”: “<user-name>is not a user”,
“documentation_url”: “https://developer.github.com/v3/repos/collaborators/#get”
}

#3) Check user’s Permission.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/collaborators/<user-name-to-check–for-permission>/permission| grep -w permission

#4) Add user as Collaborator to the Repository.

curl -X PUT -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name-to-add-collaborator>/collaborators/<user-name-to-add-as-collaborator>

Post this, the invitee will need to accept the invitation to join as collaborator. If a user is already added as collaborator, then no content is displayed else the output is displayed.

#5) Removing user as Collaborator.

curl -X DELETE -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name-to-remove-collaborator>/collaborators/<user-name-to-remove>

No content is displayed once the command is run successfully.

Organization
Note: Creating Organizations is not provided by GitHub API.

#1) List all organization accounts for a user.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/user/orgs | grep -w login

#2) Update an Organization.

curl -X PATCH -u <UserName>:<Generated-Token>-d “{\”name\”: \”TeamVN\”,\”billing_email\”: \”vniranjan72@outlook.com\”,\”email\”: \”vniranjan72@outlook.com\”,\”location\”:\”Bangalore\”,\”\”description\”: \”Updating the organization details\”}”https://api.github.com/orgs/<Org-Name>

Branches
#1) List branches in a user repository. The command will list all the branches in a repository.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/branches | grep -w name

#2) List all protected branches in a user repository.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/branches?protected=true | grep -w name

#3) List all un-protected branches in a user repository

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/branches?protected=false | grep -w name

#4) Remove Branch Protection.

curl -X DELETE -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/branches/master/protection

Pull Requests
#1) List Pull requests.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/pulls?state=open | grep -w title

Options for the state parameter are Open, Closed, All.

#2) Create a Pull request.

curl -X POST -u <UserName>:<Generated-Token>-d “{\”title\”:\”Great feature added\”,\”body\”: \”Please pull the great change made in to master branch\”,\”head\”: \”feature\”,\”base\”: \”master\”}” https://api.github.com/repos/<user-name>/<repo-name>/pulls

Create a Pull request

#3) List the number of the Pull requests created.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/pulls?state=open | grep -w number

#4) Update Pull request body or any other parameter (Maximum of 250 commits only).

curl -X PATCH -u <UserName>:<Generated-Token>-d “{\”body\”: \”Mandatory to pull the great change made in feature branch to master branch\”}” https://api.github.com/repos/<user-name>/<repo-name>/pulls/31

#5) List Pull request commits.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/pulls/31/commits

#6) List Pull request files (Maximum of 300 files only).

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/pulls/31/files| grep -w filename

#7) Merge Pull request.

curl -X PUT -u <UserName>:<Generated-Token>-d “{\”commit_message\”: \”Good Commit\”}”https://api.github.com/repos/<user-name>/<repo-name>/pulls/31/merge

Response if merged

{
“sha”: “e5db2ce465f48ada4adfb571cca2d6cb859a53c6”,
“merged”: true,
“message”: “Pull Request successfully merged”
}

Response if pull request cannot be merged

{
“message”: “Pull Request is not mergeable”,
“documentation_url”: “https://developer.github.com/v3/pulls/#merge-a-pull-request-merge-button”
}

Labels, Milestones & Issues
Labels

#1) List all labels in a repository.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/labels | grep -w name

#2) List specific label in a repository.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/labels/bug

#3) To create a label.

curl -X POST -u <UserName>:<Generated-Token>-d “{\”name\”: \”defect\”,\”description\”: \”To raise a defect\”,\”color\”: \”ff493b\”}”https://api.github.com/repos/<user-name>/<repo-name>/labels

The hexadecimal color code for the color parameter can be set from Color-hex

#4) Update label

curl -X PATCH -u <UserName>:<Generated-Token> -d “{\”color\”: \”255b89\”}” https://api.github.com/repos/<user-name>/<repo-name>/labels/defect

#5) Delete label

curl -X DELETE -u <UserName>:<Generated-Token>https://api.github.com/repos/vniranjan1972/Demo_Project_Repo_VN/labels/defect

Issues

#6) List a specific issue in a repository.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/issues/20 | grep -w title

#7) List all issues in a repository.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/issues | grep -w title

#8) Create an issue.

curl -X POST -u <UserName>:<Generated-Token>-d “{\”title\”: \”New welcome page\”,\”body\”: \”To design a new page\”,\”labels\”: [\”enhancement\”],\”milestone\”: \”3\”,\”assignees\”: [\”<user-name1>\”,\”<user-name2\”],\”state\”: \”open\”}” https://api.github.com/repos/<user-name>/<repo-name>/issues

In the above command, labels and assignees parameters are array of strings where multiple values can be provided. State parameter will have the value either open or closed.

#9) Add a label to an issue.

curl -X POST -u <UserName>:<Generated-Token> -d “{\”labels\”: [\”enhancement\”]}” https://api.github.com/repos/<user-name>/<repo-name>/issues/30/labels

#10) Edit an issue and update the parameters E.g, Labels to it.

curl -X PATCH -u <UserName>:<Generated-Token>-d “{\”labels\”: [\”bug\”,\”enhancement\”]}” https://api.github.com/repos/<user-name>/<repo-name>/issues/30

In the above command, update labels for the issue number 30.

#11) Remove a label from a specific issue.

curl -X DELETE -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/issues/30/labels/bug

#12) Remove ALL labels from a specific issue.

curl -X DELETE -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/issues/30/labels

Milestones

#13) List all Milestones.

curl -X GET -u <UserName>:<Generated-Token>-d “{\”state\”: [\”open\”]}”https://api.github.com/repos/<user-name>/<repo-name>/milestones | grep -w title

#14) List details of a specific Milestone.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/milestones/1 | grep -w title

#15) Create a Milestone.

curl -X POST -u <UserName>:<Generated-Token>-d “{\”title\”: \”R5\”,\”state\”: \”open\”,\”description\”: \”Track for milestone R5\”,\”due_on\”: \”2019-12-05T17:00:01Z\”}” https://api.github.com/repos/<user-name>/<repo-name>/milestones

In the above command the due_on is a timestamp ISO 8601 in YYYY-MM-DDTHH:MM:SSZ format. More about this can be found @ ISO 8601

#16) Update a Milestone.

curl -X PATCH -u <UserName>:<Generated-Token>-d “{\”state\”: \”closed\”}” https://api.github.com/repos/<user-name>/<repo-name>/milestones/3

#17) Delete a Milestone.

curl -X DELETE -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/milestones/3

Teams
#1) List Teams in an organization.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/orgs/<Org-Name>/teams| grep -w name

List by team ID

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/orgs/<Org-Name>/teams| grep -w id

#2) List teams by user.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/user/teams | grep -w name

#3) Create a Team, add members and add repository to the team.

curl -X POST -u <UserName>:<Generated-Token>-d “{\”name\”:\”<Team Name>\”,\”description\”: \”Enter brief description\”,\”maintainers\”: [\”<user-name>\”],\”repo_names\”: [\”<Org-name>/<Repo-Name>\”]}” https://api.github.com/orgs/Demo-Proj-Org/teams

#4) Edit team name and description.

curl -X PATCH -u <user-name>:<Generated-Token>-d “{\”name\”: \”New Team Name\”,\”description\”: \”Latest Description\”}”https://api.github.com/teams/<Team-Id>

Team ID can be retrieved by running the command from step 1.

#5) Add a repository to an existing team..

curl -X PUT -u <user-name>:<Generated-Token>https://api.github.com/teams/<Team-Id>/repos/<Org-Name>/<repo-name>

#6) Remove repository from a team.

curl -X DELETE -u <user-name>:<Generated-Token>https://api.github.com/teams/<Team-Id/repos/<Org-Name>/<repo-name-to-be-deleted-from-team>

#7) Delete a team.

curl -X DELETE -u <UserName>:<Generated-Token>https://api.github.com/teams/<Team-Id>

Search Repositories, Code, Issues
The Search API allows to search for any item.

#1) For Example, if you want to search all repositories owned by a particular user.

curl -X GET https://api.github.com/search/repositories?q=user:<user-name> | grep -w “name”

Required parameter is q that contains the search criteria consisting of keywords and qualifiers to limit the search in a specific area in Github.

#2) Search all repositories owned by a particular user that contains the words V and Niranjan in README file

curl -X GET https://api.github.com/search/repositories?q=V+Niranjan+in:readme+user:<user-name>| grep -w name

#3) Search for a keyword in the content of a file. In the below example, search for the keyword ‘System’ and ‘addEmployee’ within a file in a repository owned by a user.

curl -X GET https://api.github.com/search/code?q=System+addEmployee+in:file+language:java+repo:<user-name>/<repo-name> | grep -w name

#4) Search for the keyword ‘welcome’ within open issues and label as enhancement.

curl -X GET https://api.github.com/search/issues?q=welcome+label:enhancement+state:open+repo:<user-name>/<repo-name>| grep -w name

#5) Search for the keyword ‘address’ within closed issues and label as enhancement.

curl -X GET https://api.github.com/search/issues?q=address+label:enhancement+state:closed+repo:<user-name>/<repo-name> | grep -w name

Releases
#1) List releases in a repository by tag name and id.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/releases | grep -w tag_name

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/releases | grep -w id

#2) Get details of a single release.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/releases/<rel-id> | grep -w tag_name

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/releases/<rel-id> | grep -w body

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/releases/<rel-id> | grep -w name

#3) Get details of the LATEST release.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/releases/latest| grep -w tag_name

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/releases/latest| grep -w name

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/releases/latest| grep -w body

#4) Get release details by Tag.

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/releases/tags/<Tag-Name>| grep -w name

curl -X GET -u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name>/releases/tags/<Tag-Name>| grep -w body

#5) Create a release.

curl -X POST -u <UserName>:<Generated-Token>-d “{\”tag_name\”: \”R3.0\”,\”target_commitish\”: \”master\”,\”name\”: \”Release 3.0\”,\”body\”: \”This is for Release 3.0 of the product\”,\”draft\”: “false”,\”prerelease\”: “false”}” https://api.github.com/repos/<user-name>/<repo-name/releases

Note: In the command to create a release the parameters ‘draft’ and ‘prerelease’ takes Boolean values. Enter true or false without \”.

The draft value false means the published release is created and for true it is a un-published release.
Prerelease false means it is a full release. True value means it is a prerelease.
#6) Edit or update the release.

curl -X PATCH-u <UserName>:<Generated-Token>-d “{\”tag_name\”: \”R3.1\”}” https://api.github.com/repos/<user-name>/<repo-name/releases/<rel-id>

#7) Delete the release.

curl -X DELETE-u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name/releases/<rel-id>

#8) List assets for the release.

curl -X DELETE-u <UserName>:<Generated-Token>https://api.github.com/repos/<user-name>/<repo-name/releases/<rel-id>/assets

Conclusion
In this GitHub REST API tutorial, we saw how REST API’s can be used for various actions to GET, PUT, POST, PATCH, DELETE data.

The URL used for REST API’s to work directly with GitHub.com is https://api.github.com. Whereas, if the teams are using GitHub enterprise in their organization then the URL to use with REST API would be https://<GitHubServerName>/api/v3

All the tutorials in this series so far concentrated on the usage of GitHub from a developer perspective along with the best practices of collaboration while working in a team for version control of various types of artifacts directly on GitHub and not locally.

Our upcoming tutorial will focus on how a developer will work offline on a local repository cloned from GitHub using the Git Client interfaces like GitHub Desktop and TortoiseGit and push the changes back to the remote repository.

=> Visit Here To Learn GitHub From Scratch.

#### 软件架构
软件架构说明


#### 安装教程

1.  xxxx
2.  xxxx
3.  xxxx

#### 使用说明

1.  xxxx
2.  xxxx
3.  xxxx

#### 参与贡献

1.  Fork 本仓库
2.  新建 Feat_xxx 分支
3.  提交代码
4.  新建 Pull Request


#### 特技

1.  使用 Readme\_XXX.md 来支持不同的语言，例如 Readme\_en.md, Readme\_zh.md
2.  Gitee 官方博客 [blog.gitee.com](https://blog.gitee.com)
3.  你可以 [https://gitee.com/explore](https://gitee.com/explore) 这个地址来了解 Gitee 上的优秀开源项目
4.  [GVP](https://gitee.com/gvp) 全称是 Gitee 最有价值开源项目，是综合评定出的优秀开源项目
5.  Gitee 官方提供的使用手册 [https://gitee.com/help](https://gitee.com/help)
6.  Gitee 封面人物是一档用来展示 Gitee 会员风采的栏目 [https://gitee.com/gitee-stars/](https://gitee.com/gitee-stars/)
