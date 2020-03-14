# Deploy to github pages like a pro

GitHub Pages is a static site hosting service designed to host your personal, organization, or project pages directly from a GitHub repository.

Note that GitHub Pages is a static site hosting service and doesn’t support server-side code such as, PHP, Ruby, or Python.

To learn more about the different types of GitHub Pages sites, take a look [here](https://help.github.com/articles/user-organization-and-project-pages/).

**In this article I am gonna use vue.js and react.**

🤨 That’s the theory! Let’s start: 🤨

> ### **First of all, copy the name of your project.**

![](C:\Users\rolan\Desktop Non OneDrive\Coding\Projects\tips-patterns-practices\Vue.js\General\Copy github name.png)

The name of my repo is `deploy-react-to-gh-pages`.

> ### **If you are using react specify the `homepage` in `package.json`. [Docs](https://create-react-app.dev/docs/deployment/#building-for-relative-paths)**

```json
{
  ...
  "homepage": "/name-of-your-project/",
  ...
}
```

![](C:\Users\rolan\Desktop Non OneDrive\Coding\Projects\tips-patterns-practices\Vue.js\General\React-add-homepage.png)



If you are using **vue** create a new file in your project root named `vue.config.js` [Docs](https://cli.vuejs.org/config/#vue-config-js) and specify the `publicPath`.  [Docs](https://cli.vuejs.org/config/#publicpath)

```javascript
module.exports = {
  publicPath: '/name-of-your-project/'
}
```



> ### **Now commit and push your changes.**

**Note: if you don't commit your changes, you will lose them in the next command, so make sure you don't skip this step**



> ### **Open your terminal in your project and run:**

**Note:** For the following commands, if you are using vue, just copy paste them, if you are using react replace `dist` with `build`.
 `npm run build` creates a directory with a production build of your app. In vue, that directory in named `dist`, in react is named `build`.

- `git checkout --orphan gh-pages` [Docs](https://git-scm.com/docs/git-checkout/1.7.3.1#git-checkout---orphan)
- `npm run build`
- `git --worktree dist add --all` [Docs](https://git-scm.com/docs/git#Documentation/git.txt---work-treeltpathgt) (**for react: replace dist with build**)
- `git --work-tree dist commit -m 'Deploy'` (**for react: replace dist with build**)
- `git push origin HEAD:gh-pages --force`
- `rm -r dist` (**for react: replace dist with build**)
  We remove the dist folder after pushing, because we don't want it to be present on master (when we execute the next command)
- `git checkout -f master`
  Checkout master and discard all changes to gh-pages branch
- `git branch -D gh-pages`
  Delete the gh-pages branch
- Navigate to your github project and click 'Settings'
  ![](C:\Users\rolan\Desktop Non OneDrive\Coding\Projects\tips-patterns-practices\Vue.js\General\Github-settings.png)
- Scroll to find the section 'Github Pages' , select the `gh-pages` branch and click 'Save'
  ![](C:\Users\rolan\Desktop Non OneDrive\Coding\Projects\tips-patterns-practices\Vue.js\General\GithubPagesBranch.png)

### 🚀🚀 Congrats 🚀🚀

Your site is ready to be published.
You might have to wait a bit. Generally it takes 2–10 minutes until this process is done.

------


🤔🤔But... wait. You probably want to re-deploy. There must be a simpler solution instead of re-doing over and over again all the commands above.

> ### **Create a node.js script**

Now, we are gonna create a node.js script so whenever we want to deploy, we just run one command.

- Create a `scripts` folder in your project root.

- Inside `scripts` folder, create a `gh-pages-deploy.js` file.

- Copy and paste the code below:

  ```javascript
  const execa = require("execa");
  const fs = require("fs");
  
  (async () => {
    try {
      await execa("git", ["checkout", "--orphan", "gh-pages"]);
      console.log('Building...');
      await execa("npm", ["run", "build"]);
      // Understand if it's dist or build folder
      const prodFolderName = fs.existsSync("dist") ? "dist" : "build";
      await execa("git", ["--work-tree", prodFolderName, "add", "--all"]);
      await execa("git", [
        "--work-tree",
        prodFolderName,
        "commit",
        "-m",
        "gh-pages"
      ]);
      console.log('Pushing to gh-pages...');
      await execa("git", ["push", "origin", "HEAD:gh-pages", "--force"]);
      await execa("rm", ["-r", prodFolderName]);
      await execa("git", ["checkout", "-f", "master"]);
      await execa("git", ["branch", "-D", "gh-pages"]);
      console.log('Sucessfully deployed');
    } catch (e) {
      console.log(e.message);
      process.exit(1);
    }
  })();
  
  ```
  
- Open `package.json` and add `execa` to your `devDependencies`.

  ```json
    "devDependencies": {
      ...
      "execa": "latest"
    },
  ```

- Also add a new script in `scripts` section:

  ```json
    "scripts": {
     ...
     "gh-pages-deploy": "node scripts/gh-pages-deploy.js"
    },
  ```

- Finally, run `npm install`.



### ✨🎉 And... that's it! 🎉✨

**You can now deploy as many times as you want by just running `npm run gh-pages-deploy`.**

------

**Me-** But, hey... 🤫🤫! Wouldn't be even more exciting if we automate everything ?

**Everyone-** Do you mean that the app will be deployed itself ? 🤨🤨

**Me-** 😉😉

**Everyone-** 😲😲 

**Github Pages-** 🤭🤭

> ### Create a github action to automate deployment

Wouldn't be great if on every push on master, the app gets deployed without us doing nothing ?? 🧙‍♂️🧙‍♂️

We can achieve that by using... 🙌🙌 **Github Actions** 🙌🙌. 

GitHub Actions enables you to create custom software development life cycle (SDLC) workflows directly in your GitHub repository. [Docs](https://help.github.com/en/actions/getting-started-with-github-actions/about-github-actions)

Let's start:

- Create a `.github` (Don't forget the dot in front) folder in your project root

- Inside create another folder named `workflows`

- Inside `workflows` create a file named `gh-pages-deploy.yml` (The name is up to you).

- Now copy & paste the code below inside that file.

  ```yaml
  name: Deploy to github pages
  on:
    push:
      branches:
        - master
  jobs:
    gh-pages-deploy:
      name: Deploying to gh-pages
      runs-on: ubuntu-latest
      steps:
        - name: Setup Node.js for use with actions
          uses: actions/setup-node@v1.1.0
          with:
            version:  12.x
        - name: Checkout branch
          uses: actions/checkout@v2
  
        - name: Clean install dependencies
          run: npm ci
  
        - name: Run deploy script
          run: |
            git config user.name "GitHub Actions" && git config user.email "actions@users.noreply.github.com"
            npm run gh-pages-deploy
  ```

  [Read the docs to learn more](https://help.github.com/en/actions/configuring-and-managing-workflows/configuring-a-workflow)

- Commit and push your changes

- Now, navigate to your github project and first click *Actions* (1) then *Deploy to github pages* (2) and last click on the action (3).

  ![](C:\Users\rolan\Desktop Non OneDrive\Coding\Projects\tips-patterns-practices\Vue.js\General\Click-Actions.png)

- If everything goes well, you will see this:

  ![](C:\Users\rolan\Desktop Non OneDrive\Coding\Projects\tips-patterns-practices\Vue.js\General\Gh-Action-run.png)



### 🌟🌟 Taadaaa!!! You successfully automated deployment!!! 🌟🌟

**Now, whenever you merge a PR or push to master this action will run and will deploy your app automatically**. 👏👏



> ### Things to know

- It's definitely a good thing to know all the commands in order to push to gh-pages and creating a node.js script it's something cool. Intentionally I kept the script minimal so you can modify it (maybe adding some colors and emojis or improve it by adding some checks).

  But instead you can skip all those commands and steps and follow the instructions provided in react and vue docs.
  For react docs. See the steps [Here](https://create-react-app.dev/docs/deployment/#github-pages).
  Regarding vue, see the steps [Here](https://cli.vuejs.org/guide/deployment.html#github-pages)





🙏🙏 Thank you for reading. I would be glad to help you if you face any problem so don't hesitate to email me at rolanddoda2014@gmail.com 🙏🙏



