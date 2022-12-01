## 1. Setup package.json
Add the name of your organization or your github account to your package name
```json
"name": "@myUsername/myPackage"
```

Define where you're file will be build
```json
"files": [
    "dist/*"
],
```
Define the repository of your package
```json
"repository": {
    "url": "https://github.com/myUsername/myPackage"
},
```

Define publishConfig
```json
"publishConfig": {
  "registry":"https://npm.pkg.github.com/@myUsername"
},
```
Update your <b>version</b> in package.json
```json
"version": "1.4.5" to "version": "1.4.6"
```

---

If you want to create a VueJS package, it's recommend to follow this step to build your package :

Add this command in scripts (in package.json) to build your package.

```json
"build-library": "vue-cli-service build --target lib --name @myUsername/mycomponent ./install.js",
```

Create install.js file into root folder
```javascript
import myComponent from 'src/components/myComponent.vue
export default {
	install(Vue) {
		Vue.component('MyComponent', myComponent)
    }
}
```

Then build with this command : 
```html
npm run build-library
```
---

## 2. Github action

Push your code to your repo and then go yo your github repository > Action > <b>set up a workflow yourself</b>

![App Screenshot](https://user-images.githubusercontent.com/71376080/198026877-d1d7805a-6db6-47a6-84d8-cccb46334d95.png)

Then <b>paste</b> this code in editor field
```C
name: myPackage

on:
  push:
    branches:
      - main

jobs:
  publish-gpr:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v1
        with:
          node-version: 12
          registry-url: https://npm.pkg.github.com/
          scope: '@myUsername'
      - run: npm install
      - run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{secrets.GITHUB_TOKEN}}
```
You need to <b>replace</b> scope value with your organization or profile name

This script will will automate the creation/update of your package in github when a commit is pushed on the main branch

Once done, click <b>Start commit</b>, then <b>Commit new file</b>

![App Screenshot](https://user-images.githubusercontent.com/71376080/198028897-3fe59af8-a7b0-4371-bc58-27239d6bfb0b.png)

## 3. Generate token access

Click on your github profile and go to <b>settings</b>

![App Screenshot](https://user-images.githubusercontent.com/71376080/198030237-606d1ee2-f854-44c0-b26f-ea5dc7304246.png)


Then scroll to <b>Developer settings</b> and click on it

![App Screenshot](https://user-images.githubusercontent.com/71376080/198030564-5fc4424d-ba59-488f-863a-fab8606adbb7.png)

Go to <b>Token (classic)</b>

![App Screenshot](https://user-images.githubusercontent.com/28560613/198031188-e3fb3fae-7a18-4156-867c-e4100e7975f2.png)

Click on <b>Generate new token (classic)</b>

![App Screenshot](https://user-images.githubusercontent.com/28560613/198031990-8fa1d39e-ffc4-46a8-9eba-0a0c53316858.png)

1. Fill Note
2. Set No expiration
3. Allow read:packages

![App Screenshot](https://user-images.githubusercontent.com/28560613/198032614-993b4ff4-a92b-4b4f-b8d5-7ce05ea866e0.png)

Click on <b>Generate token</b> (at bottom)

![App Screenshot](https://user-images.githubusercontent.com/28560613/198033139-80e386e2-72ee-443b-8c4c-0d11ebe70061.png)

Copy your token <b>now</b> because you won't be able to see it again !
## 4. Install your package

Now go to your project and create <b>.npmrc</b> file in root folder, then paste this and replace with your username and your token
```html
@YOUR_USERNAME:registry=https://npm.pkg.github.com/
//npm.pkg.github.com/:_authToken=YOUR_TOKEN
```

Now you can <b>install</b> the package just like you would any other NPM package.

```js
npm install @myUsername/myPackage
```
---
If you want to install a VueJS package into a VueJS project follow this step :

Import your component into your Vue project
<b>main.js (Vue 3)</b>
```js
import { createApp } from 'vue'
import App from './App.vue'
import myComponent from '@myUsername/myComponent/'

const app = createApp(App)
app.use(myComponent)

app.mount('#app')
```

<b>main.js (Vue 2)</b>
```js
import Vue from 'vue'
import App from './App.vue'
import myComponent from '@myUsername/myComponent/'

Vue.use(components)

new Vue({
	render : function (h) {
		return h(App)
	}
}).$mount('#app')
```
---
