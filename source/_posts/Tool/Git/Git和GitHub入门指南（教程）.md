# Git和GitHub入门指南（教程）

> product.hubspot.com/blog/git-and-github-tutorial-for-beginners

8月，我们在HubSpot主持了“Women Who Code meetup”聚会，并主持了针对初学者的使用git和GitHub的研讨会。 我首先浏览了有关git的基础知识和背景的幻灯片演示，然后我们分组讨论了我创建的模拟大型协作项目工作的教程。 活动结束后我们得到了反馈，它是一个实用的动手介绍。 因此，如果您也是git的新手，请按照以下步骤操作，以轻松地更改代码库，打开拉取请求（PR）并将代码合并到master分支中。 任何重要的git和GitHub术语均以粗体显示，并带有指向官方git参考资料的链接。

## Step 0: Install git and create a GitHub account 

您需要做的前两件事是安装git并创建一个免费的GitHub帐户。

请按照 [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) 的说明安装git（如果尚未安装）。 请注意，对于本教程，我们将仅在命令行上使用git。 尽管有一些很棒的git GUI（图形用户界面），但我认为先使用git特定的命令学习git，然后在更熟悉该命令后尝试git GUI会更容易。

完成此操作后，请在[here](https://github.com/join)创建一个GitHub帐户。 （公共存储库是免费的，~~但是私人存储库是收费的。~~[现在免费了，感谢巨硬，如果它不是想干掉开源界的话]）

## Step 1: Create a local git repository 

When creating a new project on your local machine using git, you'll first create a new **[repository](https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository)** (or often, '**repo**', for short). 

使用git在本地计算机上创建新项目时，首先要创建一个新的存储库（或简称为“ repo”）。

