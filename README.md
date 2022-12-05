# Deploy `json-server` to `{{ free hosting site }}`

> Instructions how to deploy the full fake REST API [json-server](https://github.com/typicode/json-server) to various free hosting sites. Should only be used in development purpose but can act as a simpler database for smaller applications.

* [**Create your database**](#create-your-database)
* [Deploy to **Heroku**](#deploy-to-heroku)
* [Deploy to **Glitch**](#deploy-to-glitch)
* [Deploy to **Azure**](#deploy-to-azure)

## Create your database

1. Press the green `Use this template`-button in the right corner
2. Give your new repo a name and press the green `Create repository from template`-button
3. Clone your newly created repository to your computer

4 . Change the contents of `db.json` to **your own content** according to the [`json-server example`](https://github.com/typicode/json-server#example) and then `commit` your changes to git locally.

_this example will create `/posts` route , each resource will have `id`, `title` and `content`. `id` will auto increment!_
```json
{
  "posts":[
    {
      "id" : 0,
      "title": "First post!",
      "content" : "My first content!"
    }
  ]
}
```

---

## Deploy to **Heroku**

<img align="right" width="100px" height="auto" src="https://cdn.worldvectorlogo.com/logos/heroku.svg" alt="Heroku">

Heroku is a free hosting service for hosting small projects. Easy setup and deploy from the command line via _git_.

###### Pros

* Easy setup
* Free

###### Cons

* App has to sleep a couple of hours every day.
* "Powers down" after 30 mins of inactivity. Starts back up when you visit the site but it takes a few extra seconds. Can maybe be solved with [**Kaffeine**](http://kaffeine.herokuapp.com/)

---

### Install Heroku

1 . [Create your database](#create-your-database)

2 . Create an account on <br/>[https://heroku.com](https://heroku.com)

3 . Install the Heroku CLI on your computer: <br/>[https://devcenter.heroku.com/articles/heroku-cli](https://devcenter.heroku.com/articles/heroku-cli)

4 . Connect the Heroku CLI to your account by writing the following command in your terminal and follow the instructions on the command line:
```bash
heroku login
```

5 . Then create a remote heroku project, kinda like creating a git repository on GitHub. This will create a project on Heroku with a random name. If you want to name your app you have to supply your own name like `heroku create project-name`:
```bash
heroku create my-cool-project
```

6 . Push your app to __Heroku__ (you will see a wall of code)
```bash
git push heroku master
```

7 . Visit your newly create app by opening it via heroku:
```bash
heroku open
```

8 . For debugging if something went wrong:
```bash
heroku logs --tail
```

---

#### How it works

Heroku will look for a startup-script, this is by default `npm start` so make sure you have that in your `package.json` (assuming your script is called `server.js`):
```json
 "scripts": {
    "start" : "node server.js"
 }
```

You also have to make changes to the port, you can't hardcode a dev-port. But you can reference herokus port. So the code will have the following:
```js
const port = process.env.PORT || 4000;
```

## Deploy to Glitch

Not tested 100%. Same as with Heroku, will sleep after a while.

1. Register for [Glitch](https://glitch.com/) or go to [Glitch/edit](https://glitch.com/)
2. Click **New Project**
3. Click **Import from GitHub**
4. Paste `https://github.com/jesperorb/json-server-heroku.git` into the URL-input and click OK.
5. Wait for it to setup
6. Press **Share**-button to get your URL to live site. It should be something for example like: `https://fallabe-pie-snake.glitch.me`. And your DB will be at `https://fallabe-pie-snake.glitch.me/posts`

## Deploy to **Azure**

<img align="right" width="100px" height="auto" src="https://docs.microsoft.com/en-us/azure/media/index/azure-germany.svg" alt="Azure">

You can also use _Microsoft Azure_ to deploy a smaller app for free to the Azure platform. The service is not as easy as _Heroku_ and you might go insane because the documentation is really really bad at some times and it's hard to troubleshoot.

The **pros** are that on _Azure_ the app **will not be forced to sleep**. It will sleep automatically on inactivity but you can just visit it and it will start up.

## Installation

1 . Create a Microsoft Account that you can use on Azure: </br>
https://azure.microsoft.com/

2 . Install the `azure-cli`: <br/>
https://docs.microsoft.com/en-us/cli/azure/install-azure-cli
_This might cause some trouble, you will see. Remember to restart your terminal or maybe your computer if the commands after this does not work_

3 . Login to the service via the command line and follow the instructions: </br>
```bash
az login
```
_You will be prompted to visit a website and paste a confirmation code_


## Create the project

1 . [Create your database](#create-your-database)

2 . Create a resource group for your projects, replace the name to whatever you want just be sure to use the same group name in all commands to come. You only have to create the resource group and service plan once, then you can use the same group and plan for all other apps you create if you like.

```bash
az group create -n NameOfResourceGroup -l northeurope
```

3 . Create a service plan:

```
az appservice plan create -n NameOfServicePlan -g NameOfResourceGroup
```

4 . Create the actual app and supply the service plan and resource group
```bash
az webapp create -n NameOfApp -g NameOfResourceGroup --plan NameOfServicePlan
```

5 . Create deployment details. A git-repo is not created automatically so we have to create it with a command:

```bash
az webapp deployment source config-local-git -n NameOfApp -g NameOfResourceGroup
```

6 . From the command in step 5 you should get a **url** in return. Copy this url and add it as a remote to your local git project, for example:

```bash
git remote add azure https://jesperorb@deploy-testing.scm.azurewebsites.net/deploy-testing.git
```

7 . Now you should be able to push your app:
```bash
git push azure master
```

You should be prompted to supply a password, this should be the pass to your account. If not, you can choose a different password at your Dashboard for Azure: **[https://portal.azure.com/](https://portal.azure.com/)**

Choose **App Services** in the sidebar to the left and the choose your app in the list that appears then go to **Deployment Credentials** to change your password for deployment:<br>
https://docs.microsoft.com/en-us/azure/app-service/app-service-deployment-credentials


<!-- steps created demo 

Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.


PS E:\Github\json-server-heroku> git status
On branch master

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   db.json

no changes added to commit (use "git add" and/or "git commit -a")
PS E:\Github\json-server-heroku> git add .
PS E:\Github\json-server-heroku> git commit -m " initial commit"
[master 00c28a1]  initial commit
 1 file changed, 126 insertions(+), 16 deletions(-)
 rewrite db.json (84%)
PS E:\Github\json-server-heroku> heroku create netmed-clone-v1-server
 »   Warning: heroku update available from 7.53.0 to 7.60.2.
Creating ⬢ netmed-clone-v1-server... done
https://netmed-clone-v1-server.herokuapp.com/ | https://git.heroku.com/netmed-clone-v1-server.gitPS E:\Github\json-server-heroku> git push heroku master
Enumerating objects: 103, done.
Counting objects: 100% (103/103), done.
Delta compression using up to 4 threads
Compressing objects: 100% (61/61), done.
Writing objects: 100% (103/103), 87.53 KiB | 43.76 MiB/s, done.
Total 103 (delta 41), reused 99 (delta 40), pack-reused 0
remote: Compressing source files... done.
remote: Building source:
remote: 
remote: -----> Building on the Heroku-20 stack
remote: -----> Determining which buildpack to use for this app
remote: -----> Node.js app detected
remote:        
remote: -----> Creating runtime environment
remote:
remote:        NPM_CONFIG_LOGLEVEL=error
remote:        USE_YARN_CACHE=true
remote:        NODE_VERBOSE=false
remote:        NODE_ENV=production
remote:        NODE_MODULES_CACHE=true
remote:
remote: -----> Installing binaries
remote:        engines.node (package.json):  unspecified
remote:        engines.npm (package.json):   unspecified (use default)
remote:        engines.yarn (package.json):  unspecified (use default)
remote:
remote:        Resolving node version 16.x...
remote:        Downloading and installing node 16.15.1...
remote:        Using default npm version: 8.11.0
remote:        Resolving yarn version 1.22.x...
remote:        Downloading and installing yarn (1.22.18)
remote:        Installed yarn 1.22.18
remote:        
remote: -----> Installing dependencies
remote:        Installing node modules (yarn.lock)
remote:        yarn install v1.22.18
remote:        [1/4] Resolving packages...
remote:        [2/4] Fetching packages...
remote:        [3/4] Linking dependencies...
remote:        [4/4] Building fresh packages...
remote:        Done in 2.98s.
remote:
remote: -----> Build
remote:        
remote: -----> Pruning devDependencies
remote:        yarn install v1.22.18
remote:        [1/4] Resolving packages...
remote:        [2/4] Fetching packages...
remote:        [3/4] Linking dependencies...
remote:        [4/4] Building fresh packages...
remote:        warning Ignored scripts due to flag.
remote:        Done in 0.56s.
remote:        
remote: -----> Caching build
remote:        - yarn cache
remote:        
remote: -----> Build succeeded!
remote: -----> Discovering process types
remote:        Procfile declares types     -> (none)
remote:        Default types for buildpack -> web
remote:
remote: -----> Compressing...
remote:        Done: 34.3M
remote: -----> Launching...
remote:        Released v3
remote:        https://netmed-clone-v1-server.herokuapp.com/ deployed to Heroku
remote:
remote: Verifying deploy... done.
To https://git.heroku.com/netmed-clone-v1-server.git
 * [new branch]      master -> master
PS E:\Github\json-server-heroku> 


 -->