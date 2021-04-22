# Vue - Demo how to force `.vue` extension in imports with `eslint` in default `vue-cli` project
*This is built on top of an empty Vue Cli 4.2.5 project with default config: `vue create yourProjectName --default`*
  ## How do I check if this repo actually forces .vue extension?
```sh
git clone https://github.com/3nuc/demo-vue-eslint-force-dot-vue-extension-in-imports 
cd demo-vue-eslint-force-dot-vue-extension-in-imports 
npm ci 
npm run lint
```
You should get three eslint errors in `App.vue`. The repo intentionally holds code that fails linting to demonstrate that the .vue forcing on imports is working.

## I have an existing project and I want to add forcing .vue extension in it. What to copy from this repo?

Do this:  
1.
  ```sh
  #I'm assuming you already have eslint
  npm install --save-dev eslint-plugin-import eslint-import-resolver-alias
  ```

2. Add the following to your eslint config file (`.eslintrc.js`/`.eslintrc.json`/`eslintConfig in package.json`/`.eslintrc.yml`)
```js
// I'm using .eslintrc.js
module.exports = {
//...unimportant properties like rot, env, extends, parserOptions etc
  plugins: ["import"],
  settings: {
    "import/resolver": {
      alias: {
        map: [
          ["@", "./src"], //default @ -> ./src alias in Vue, it exists even if vue.config.js is not present
          /* 
          *... add your own webpack aliases if you have them in vue.config.js/other webpack config file
          * if you forget to add them, eslint-plugin-import will not throw linting error in .vue imports that contain the webpack alias you forgot to add
          */
        ],
        extensions: [".vue", ".json", ".js"]
      }
    }
  }
}
```

## Why would you want to force the .vue extension in imports?
Because in the VS Code `Vetur` extension, you need to have `.vue` extension in imports in order to `Go to definition` right click option working in template.
More info: https://vuejs.github.io/vetur/guide/FAQ.html#vetur-cannot-recognize-my-vue-component-import-such-as-import-comp-from-comp
