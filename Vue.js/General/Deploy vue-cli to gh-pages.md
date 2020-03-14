# Deploy vue-cli 3 or 4 project to github pages

![](C:\Users\rolan\Desktop Non OneDrive\Coding\Projects\tips-patterns-practices\Vue.js\General\GithubPagesVue.png)


GitHub Pages is a static site hosting service designed to host your personal, organization, or project pages directly from a GitHub repository.

Note that GitHub Pages is a static site hosting service and doesn’t support server-side code such as, PHP, Ruby, or Python.

To learn more about the different types of GitHub Pages sites, take a look [here](https://help.github.com/articles/user-organization-and-project-pages/).

That’s the theory! Let’s start: 🤨

So, you have a github project and you want to host it on netlify. 😂 I’ am just kidding. You are here because you want to host it on github pages. 😉 Please follow carefully the following steps:



- Create a new local branch in your project and name it `gh-pages`.

- Go to github and copy the name of the repository. Let’s assume the repository name is `my-first-project`

- Create a new file in root directory of your project and name it `vue.config.js`. Learn more about this file [here](https://cli.vuejs.org/config/#global-cli-config).

- In `vue.config.js` file paste the following code:

  ```javascript
  module.exports = {
    publicPath: '/my-first-project/'
  }
  ```

- Run `npm run build`, and wait for it to finish.

- `git add dist -f `

- `git commit dist -m 'gh-pages'`

- Run `git subtree push --prefix dist origin gh-pages`

- Navigate to your github project and click 'Settings'
  ![](C:\Users\rolan\Desktop Non OneDrive\Coding\Projects\tips-patterns-practices\Vue.js\General\Github-settings.png)

- Scroll to find the section 'Github Pages' , select the `gh-pages` branch and click 'Save'
  ![](C:\Users\rolan\Desktop Non OneDrive\Coding\Projects\tips-patterns-practices\Vue.js\General\GithubPagesBranch.png)

- And... that's it!! You site is ready to be published. 🎆🎆 🔥 🔥.
  You might have to wait a bit. Generally it takes 2–10 minutes until this process is done.



# 😻😻 Pro tip: 😻😻

By taking advantage of npm scripts you can deploy your app to gh-pages easily. Please follow the steps below:

- Open `package.json` and add a new script which you can copy/paste below:

  ```json
    "scripts": {
      // ...  
      "deploy-gh-pages": "git checkout -B gh-pages && npm run build && git add dist -f && git commit dist -m 'gh-pages' --no-verify && git subtree push --prefix dist origin gh-pages && git checkout master && git branch -D gh-pages"
    }
  ```

- Open you terminal and run `npm run deploy-gh-pages`

------

If something goes wrong feel free to contact me at  ‘rolanddoda2014@gmail.com’ .

Feel free to give any feedback and if you want to follow me!

Thanks for reading and for your time!!