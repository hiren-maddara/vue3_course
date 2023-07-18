# VUE crash course

1. Counter

- Introduce vue
- State
- Event handler

2. Notes App

- Directives

3. Quiz app

- Components
- Props
- Watch
- Emit
- Routes
- Compute

4. TV shows app

- Fetch data from an api
- Lifecycle hooks
- Suspense
- Slots
- Keep alive
- Composables

5. Instagram clone app

- Authentication
- Pinia
- DB relations
- Uplaoding photos
- Pagination
- Vue UI library

## Intro

> This is a section of the full udemy course about Vue, take a look

- Install Node, Npm, VScode, add extensions: Volar, Vue(syntax highlight)
- Install Vue devtools chrome extension
- Vue js is an open-source modelview-viewmodal front end Js framework for building user interfaces and single page apps
- Creating a vue app: using the cli

```bash
npm init vue@latest
# give project name
# specify which features to add
cd projectname
npm install
npm run dev
```

## 1. Counter App

- We write a counter with both vanilla js and vue to see the dffcs btn the two
- Vue makes it easier to design UIs
- Vue uses a lightweight bundler: `vite` which is far easier to use and configure compared to `webpack`
- This is a plus for Vue unlike react
- The `<style scoped>` ensures that the styles are applied only to the current component, without leaking to other parts of the UI

- `<script scoped>`

- Mustache syntax (double curly braces) inside the template tag of a component, allows us to add Js in the template.
- They allows us access the Js we wote in the `script` tag
- With event handlers, the text we put in strings i actual Js not just a string. We can use this method to call certain functions when a certain event occurs. `@click="myFunction()"`

- Use the `ref` function to declare state variables. Changes to state variables trigger rerendering of the component
- This returns an object, and the actual/initial valueis stored in Object.value
- But within the template markup, whenever we use a state variable, the value is automatically extracted

#### Options API vs Composition API

- The compisition API is the new approach to writing vue apps

```js
//options API
//<script> /**without setup keyword */
export default {
  data() {
    //this method returns all the component's state vars
    return {
      count: 0,
      anotherStateVar: false,
    };
  },
  methods: {
    //this object contain state setter functions
    addToCount() {
      this.count++;
    },
  },
};
```

```js
// <script setup> //the setup keyword indicates that you want to use the composition API
const count = ref(0);
const stateVar = ref(false);

function addToCount() {
  count.value++;
}
```

## 2. Notes App

- Directives
- 2-way data binding

## 3. Quiz

- Search functionality, with 2-way binding
- watch
- SFCs / components
- Passing props
- Routing & single page applications: using dfft components

```js
//ROUTING RULES
// src/router/index.js
import { createRouter, createWebHistory } from "vue-router";
import Home from "../views/Home.vue";
import About from "../views/About.vue";

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: "/",
      name: "name",
      component: Home,
    },
    {
      path: "/about",
      name: "about",
      component: About,
    },
  ],
});

export default router;
```

- Usage

```js
// /src/main.js

import { createApp } from "vue";
import App from "./App.vue";

const app = createApp(App);
app.use(router);

app.mount("#app");
```

```js
//App.vue
<script setup>
import {RouterView} from "vue-router"
</script>

<template>
    <div>
        <RouterView />
    </div>
</template>


```

# The Docs: 2023

## 1. Quick start

- With an up-to-date node version installed, use the `npm init vue@latest` to jump a start a vue app
- Then run the specifed commands to install the required dependencies

#### Using Vue from CDN

- You can use vue directly from a CDN via a script tag

```js
<script src="https://unpkg.com/vue@3/dist/vue.global.js"> </script>
```

- When using the CDN Vue, no build step is required. It is simple to setup an dsuitable for enhancing static HTML. You wont be able to use SFC
- You can also download the Vue scripts and serve them from your server, or package them as part of your project
- The global build link, loads the global build of vue, whre all top-levels APIs are exposed as properties on the global `Vue` object.

```js
<script src="https://unpkg.com/vue@3/dist/vue.global.js"> </script>

<div id="app">{{message}} </div>

<script>
    const {createApp, ref} = Vue
</script>
```

- You can also use the ES module builds, so you r able to use import/export
- You can slpit the files into dfft modules

```html
<!-- index.html -->
<div id="app"></div>

<script type="module">
    import { createApp } from 'vue'
    import MyComponent from './my-component.js'

    createAppp(MyComponent).mount('#app')
```

```js
//my-component.js
import {ref} from 'vue'

export default {
    setup(){
        const count = ref(0)
        return {count}
    }

    template: `
        <div> Count is {{count}} </div>
    `
}
```

## 2. Creating a vue application

- Every vue app starts by creating a new application instance with the `createApp` function

```js
import { createApp } from "vue";

const app = createApp({
  // root component options
});
```

- The root component
- Every app requires a root component that can contain other components as its children. The object we pass into `createApp` is a component
- If we use single file components(SFCs), we import the component from another file

```js
import { createApp } from "vue";

import App from "./App.vue";

const app = createApp(App);
```

> Most real apps are organised into a tree of nested, reusable components

- Mounting the app
- An app instance wont render anything until its `.mount()` method is called. It expects a container argument which can be an actual DOM element or a selector string

```js
<div id="app"></div>;

app.mount("#app");

//or

app.mount(document.querySelector("#app"));
```

- App configurations
- The app instance exposes a `.config` object that allows us to configure a few app-level options, for exmaple defining an app-level error handler that captures errors from all the descendant components

```js
app.config.errorHandler = (err) => {};
```

## 3. Template syntax

- Vue uses an html-based syntax that allows you to declaratively bind the rendered DOM to the underlying component's data.
- Under the hood, vue compiles the templates into optimised Js code
- If you ae familiar with Virtual DOM concepts an prefer the raw power of Js, you can alos directly write render functions instead of templates, with optional JSX support

#### Text interpolation:

- Using th mustache syntax (double curly braces) is the basic form of data binding

#### raw HTML

- The double curlies interpret th data as plain text not HTML.
- To output real HTML, you need to use the `v-html` directive

```html
<p>Text Interpolation: {{rawHTML}}</p>
<p>v-html directive: <span v-html="rawHTML"></span></p>
```

> Directives are prefixed with v- to indicate that they are special attributes provided by Vue. They apply secial reactive behavior to thr renered DOM
> The v-html replaces the elements inner html with the binding
> Dynamically rendering arbitrary HTML on your website can very dangerous bse it leads to XSS vulnerabilities.

#### Attribute bindings

- Mustaches cannot be use inside HTML attributes
- Instead, use a `v-bind` directive

```html
<div v-bind:id="dynamicId">
  <!-- short hand syntax -->
  <div :id="dynamicId"></div>
</div>
```

- YOu can dynamically bind multiple attributes

```html
<div v-bind="{id: 'container', class="wrapper"}" >
```

#### Using Js expressions

- Vue supports the full power of JS expressions insid eall data bindings

```html
{{ num + 1 }} {{ ok ? 'yes' : 'no' }} {{ message.split('').reverse().join('') }}
<div :id="`list-${id}`"></div>
```

To use Js expressions;

- inside text interpolations, use Mustaches
- In the attribute value of any Vue directive(inside ouble quotes)

> Template expressions are sandboxed and have access to a restrited list of globals

#### Directives

- Their values are expected to b single JS expressions, except `v-for`, `v-on`, `v-slot`
- Theye apply updates to the DOM when the value of its expression chnages

```html
<!-- Only render if `seen` is truthy -->
<p v-if="seen"> Now you see me</p
```

- They can tak arguments

```html
<a v-bind:href="url">...</a>
<!-- shorthand -->
<a :href="url"> ... </a>

<a v-on:click="doSomething">....</a>

<!-- shorthand -->
<a @click="doSomething"> ...</a>
```

- Dynamic arguments

```html
<a v-bind:[attrName]="url"> ... </a>
<a :[attrName]="url">....</a>

<a @[eventName]="doSomething"></a>
```

- Dynamic Argument Value Constraints

  - Expected to evaluate to a string with exception of `null`
  - The special value `null` can be used to explicitely remove the binding.
  - Any otherr non-string value will trigger a warning

- Dynamic Argument syntax contraints

  - Quotes and spaces are inside HTML attribute names
  - If you need to pass a complex dynamic arg use a computed property

- Modifiers

```html
<form @submit.prevent="submitForm">
  <!-- e.preventDefault() -->
</form>
```

## 4. Reactivity

- in composition API, you are recommended to declare reactive state using `ref()`
- `ref()` takes the argument and returns it wapped in a `ref` object with a `.value` property

```js
const count = ref(0);
count.value++;
console.log(count.value); //-> 1
```

- refs are automatically unwrapped when used inside templates
- You can also mutate it directly in event handlers

> Manually exposing state and methods via `setup()` can be verbose. It can be avoided when using SFCs , we simple specify `<script setup>`

- Any changes to refs are detected by Vue and automatically trigger rerendering of the componentto reflect the changes
- In JS, there's no way to detect if a variable was mutated, but we can intercept a property's get and set operations. This way we watch for when the value is

- Refs can be an Js data structures: Objs, Arrays, simple text/boolean/int, Maps, Sets. Just any change however deep will trigger rendering

##### DOM update timing

- DOM updates are not applied synchronously. They are buffered until the `next tick` in the update cycle to ensure that each component updates only once now matter how many state changes you hav emade to it

- To wait for the DOM upadtetooo complete after a state chnage, use the `nextTick` global API

```js
import { nextTick } from "vue";

async function increment() {
  count.value++;

  await nextTick();
  //now that the DOM is updated, go on
}
```

#### reactive() API

- Another way to declare reactive state
- reactive makes an object itself reactive

```js
import {reactive} from 'vue'

const state = reactive({ count: 0 })

//template
<button @click="state.count++" >{{state.count}}</button>
```

> There's also `shallowReactive` for opting out deep reactivity

- Limitations of reactive()
  - Limited value types: cannot hold primitve types
  - Cannot replace entire object
  - Not destructure friendly

## 5. Computed properties

- For complex logic that includes reactive data,it is reommended to use computed poperty

## 6. Class and style bindings

- `class` and `style` are both attributes, we can use `v-bind` shorthand syntax to bind them
- We pass an object to `:class` to dynamically toggle classes

```html
<div :class="{ active: isActive, 'text-danger': !hasError}"></div>

<!-- The presence of 'active' is determined by the truthness of the data property `isActive` -->
```

- We can bind :class to an array a list of classes

```js
const activeClass = ref('active')
const errorClass = ref('text-danger')

<div :class="[activeClass, errorClass]">


```

- Inline styles

```js

<div :style="{'font-size': '30px'}">
```

## 7. Conditional rendering: `v-if`

- The directive `v-if` is used to consitionally rnder a block.
- The block will be rendered only if the directive's expression returns a truthy value

```html
<h1 v-if="awesome">Vue is awesome</h1>
```

- `v-else`
- You can use this t indicate an else block for v-if

```html
<button @click="awsome = !awesome"> Toggle </b>
<h1 v-if="awesome"> Vue is awesome</h1>
<h1 v-else>Oh no</h1>
```

- `v-else-if` serves as an else-if block

```html
<div v-if="type === 'A'">A</div>
<div v-if="type === 'B'">B</div>
<div v-if="type === 'C'">C</div>
<div v-else>Not A/B/C</div>
```

- V-if should be attached to a single element, but if we want ot toggle more then one element, we attached it on a common parent or on a `template` element.

## 8. List rendering

- Use `v-for` directive

```js
const items = ref([{ message: "foo" }, { message: "Bar" }]);
```

```html
<li v-for="item in items">{{ item.message}}</li>

<!-- 2 -->
<li v-for="(item, index) in items">{{index}} - {{item}}</li>

<!-- with an object -->
<li v-for="(value, key, index) in myObj">{{index}} - {{Value}}</li>

<!-- With a range -->
<li v-for="n in 10">{{ n }}</li>
```

- Can be use with `v-if`

> You need to provide a unique key attribute for each of th eitems in a list, to help Vue identify them uniquely

## 9. Event Handling

- We can use the `v-on` directive or its shortcut (`@event`)
- `@event="handler"`
- The handler can be an inline handler or a method hndler on an object
- We can also call handlers with their arguments

- Modifiers

  - `.stop, .prevent, .self, .capture, .once, .passive`

- Key modifiers

```js
<input @keyup.enter="submit" />

//@keyup.page-down

// Key aliases
.enter .tab .delete /**both del and Backspac */ .esc .space .up .down .left .right

// system aliases
.ctrl .alt .shift .meta

<input @keyup.alt.enter="clear" />

<a @click.ctrl="doIt" />
// ctrl + click

.exact
// only fire if ctrl is clicked and not any othe keys
<button @click.ctrl.enter="onCtrlClick" />

//mouse btn modifiers
.left .right .middle
```

## 10. Form input bindings

- When dealing with forms on the frontend, we need to sync the state of form input elements with corresponding state in the Js
- The `v-model` directive is useful instead of attaching change event handlers
- The `v-model` can be used on inputs of dfft types

> `v-model` ignores the initial value, chcked or selected attributes found on any form elements. It will always treat the current bound Js state as the source of truth.
> You should declare the initial value using reactivity APIs

```js
// basic usage
<p>Message is: {{message}}</p>
<input v-modl="message" placeholder="edit me"/>

//checkbox
<input type="checkbox" id="checkbox" v-model="checked" />
<label for="checkbox">{{checked}} </label>

///checked items
const checkedNames = ref([])
<div>
    Checked names: {{ checkedNames }}
</div>
<input type="checkbox" name="jack" id="jack" v-model="checkedNames" />
<label for="jack">Jack</label>


// select

//Modifiers
.lazy //syncs the input with the data after each `input`
//synced after "change" instead of input
<input v-model.lazy="msg" />

.number
.trim
```

## 11. Lifecycle hooks

- Each vue component instance goes through a series of initialization steps when it is created
- Set up data observation, compile the template, mount the instance to the DOM, update the DOM when data changes

- Vue also runs functions along the way, called lifecycle hooks, giving uses the opportunity to add their own code at specific stages

- onMounted: run code after the component has finishd the initial rndering and created DOMnodes

```js
<script setup>
import {onMounted} from 'vue'

onMounted(() => {
    console.log('the component is now mounted')
})
<script>
```

- onUpdated, onUnmounted,

## 12. Watchers

- Computed properties allow us to declaratively compute derived values.
- However, there r cases where we need to perform 'side effects' in reaction to state changes like mutating the DOM, or changing another piece of statebased on the result of an async operation
- With the composition API we can use the `watch` function to trigger a callback whenever a piece of state changes

```js
//<script setup>
import {ref, watch} from 'vue'

const question = ref('')
const answer = ref('Questions contain a question mark')


//watch works directly on a ref
watch(question, async(newQuestion, oldQuestion) => {
    if(newQuestion.indexOf('?') > -1) {
        answer.value = 'Thinking...'
        try {
            const res = await fetch('https://yesno.wtf/api')
            anwser.value = (await res.json()).answer
        } catch(err){
            answer.value = 'Error! Couldnt reach the API' + error
        }
    }
})

//</script>

<template>
    <p>
        Ask a yes/no question:
        <input v-model="question">
    </p>
    <p>
        {{answer}}
    </p>
<template />
```
