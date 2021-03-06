
import { themes } from 'mdx-deck'

export const theme = {
  ...themes.prism
}

<Footer>

@debs_obrien

</Footer>

# Building Nuxt.js Applications 🔥


---

# Intro

Who are you?  Who am I? 😅

<Notes>
Use miro to create a fun graph of names and features we like
</Notes>

---

# What is Nuxt?

<Notes>
Use Jamboard to get them to write post it notes of what Nuxt is for them
</Notes>

---

# What we will cover!

<Steps>

- Create Nuxt app
- Dynamic Pages
- Data fetching
- Nuxt Content
- Nuxt commands and deployments
- Build an application
🤯

</Steps>

<Notes>
Quick intro to what we will cover. 
</Notes>

---

# How are we gonna do this?

<Steps>
- Breakout Rooms
- CodeSandbox
- VSCode Live Share
- All on own and push to repo
</Steps>

<Notes>
Decide if we are going to do breakout room or pair program or all together - depends on numbers and how people feel
</Notes>

---

# Creating a Nuxt.js project


```bash
yarn create nuxt-app <project-name>
```

<Notes>
Everyone starts the project
</Notes>

---

# Let's get started

```bash
cd <project-name>
yarn dev
```

---

# Project Structure

## What's in my folders?

<Notes>
Go over folder structure created and files
</Notes>

---

# Routing

## How are routes created?

## How do we link from one page to another?

<Notes>
Ask question if don't know answer show in docs NuxtLink
Show where router.js file lives and that it is code splitted
</Notes>

---

# Exercise

<Steps>
1. Create a page called about
2. Add a link to the about page and home page
3. And a link in the about page to the home page
</Steps>
---

# Solution

pages/index.vue


```html
<template>
  <main>
    <h1>Home page</h1>
    <NuxtLink to="/about">
      About
    </NuxtLink>
  </main>
</template>
```
---

pages/about.vue

```html
<template>
  <main>
    <h1>About page</h1>
    <NuxtLink to="/Home">
      Home
    </NuxtLink>
  </main>
</template>
```

---

# Images

Static: 

```html
<img src="my-image.jpg />
```

Assets:

```html
<img src="~/assets/my-image.jpg />
```

---

# Images

Background Image:

```html
background: url('~assets/my-image.jpg')
```

Dynamic image:

```html
<img src="require(`~/assets/${image}.jpg` />
```

---

# Components

## How do I add a component?

`nuxt.config.js`

```jsx
export default {
  components: true
}
```
<Notes>
Make sure component is set to `true`in your nuxt.config file
</Notes>

---

# Components

## How do I add a component?

```bash
components/
  TheHeader.vue
```

`pages/index.vue`

```html
<template>
  <div>
    <TheHeader />
  </div>
</template>
```

<Notes>
Add your component to components folder and then use direct in your template
</Notes>


---

# Components

## How can I Lazy Load it?

`pages/index.vue`

```html
<template>
  <div>
    <TheHeader />
    <LazyTheFooter />
  </div>
</template>
```

<Notes>
  Show how to lazy load with simple show on click
</Notes>

---

# Components

## What about Nested Directories?

```bash
components/
  base/
    Button.vue
```

```jsx
components: {
  dirs: [
    '~/components',
      {
        path: '~/components/base/',
        prefix: 'Base'
      }
  ]
}
```

```html
<BaseButton />
```

^ 

---

# Exercise Time

<Steps>
1. Create a folder called base in the components folder and inside:
    - Create an Alert.vue component
    - Create a Button.vue component
2. Prefix your components to have the word 'Base' without changing the component name
3. Add both components to your page as BaseAlert and BaseButton
4. LazyLoad the Alert component
5. When you click on the Button component it will show the alert component
</Steps>

---

# Solution

Folder structure

```bash
components/
	Base/
		Button.vue
		Alert.vue
```

---

# Solution

nuxt.config.js

```jsx
components: {
  dirs: [
    '~/components',
      {
        path: '~/components/base/',
        prefix: 'Base'
      }
  ]
}
```

---

# Solution

pages/index.vue

```html
<template>
  <div>
    <LazyBaseAlert></BaseAlert>
    <button v-if="!show" @click="showAlert">Click Me</button>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        show: false
      }
    },
    methods: {
      showAlert() {
        this.show = true
      }
    }
  }
</script>
```

---

# Data Fetching

- AsnycData Hook
- Fetch Hook

<Notes>
  What are the methods to fetch data and the differences when fetching data in Nuxt
</Notes>

---

# The Fetch Hook

`fetch` is a hook called during server-side rendering after the component instance is created, and on the client when navigating. The fetch hook should return a promise (whether explicitly, or implicitly using `async/await`) that will be resolved:

- On the server before the initial page is rendered
- On the client some time after the component is mounted

---

# The Fetch Hook

```jsx
export default {
  data() {
    return {
      mountains: []
    }
  },
  async fetch() {
    this.mountains = await fetch('https://api.nuxtjs.dev/mountains').then(res =>
      res.json()
    )
  },
}
```

---

# The Fetch Hook

It exposes `$fetchState` at the component level with the following properties:

- `pending` is a `Boolean` that allows you to display a placeholder when `fetch` is being called *on client-side*.
- `error` is either `null` or an `Error` thrown by the fetch hook
- `timestamp` is a timestamp of the last fetch, useful for [caching with `keep-alive`](https://nuxtjs.org/guides/features/data-fetching#caching)

---

# The Fetch Hook

```html
<template>
  <p v-if="$fetchState.pending">Fetching mountains...</p>
  <p v-else-if="$fetchState.error">An error occurred :(</p>
  <div v-else>
    <h1>Nuxt Mountains</h1>
    <ul>
      <li v-for="mountain of mountains">{{ mountain.title }}</li>
    </ul>
  </div>
</template>
```

---

# The Fetch Hook

You can manually call fetch in your component by calling `this.$fetch()`.

```html
<button @click="$fetch">Refresh</button>
```

---

# The Fetch Hook

You can use `keep-alive` directive in `<nuxt/>` and `<nuxt-child/>` component to save fetch calls on pages you already visited:

```html
<template>
  <nuxt keep-alive />
</template>
```

---

# Exercise

<Steps>
1. Create a card component, image, title, description etc
2. Use fetch to call the river API 
3. Add the card component to a page
4. Add a button to refetch the data on click
5. Use fetchState to show loading message
6. Use fetchState to show error message
</Steps>

---
# Nuxt API
[https://api.nuxtjs.dev/rivers](https://api.nuxtjs.dev/rivers)

---

# Solution

```html
<template>
  <p v-if="$fetchState.pending">Fetching rivers...</p>
  <p v-else-if="$fetchState.error">An error occurred :(</p>
  <div v-else>
    <h1>Nuxt Rivers</h1>
    <ul>
      <li v-for="river of rivers">
				<h2>{{ river.title }}</h2>
				<img :src="river.image" :alt="river.title" />
				<p>{{ river.description }}</p>
			</li>
    </ul>
		<button @click="$fetch">Refresh</button>
  </div>
</template>
```

---

# Solution

```jsx
export default {
  data() {
    return {
      rivers: []
    }
  },
  async fetch() {
    this.rivers = await fetch('https://api.nuxtjs.dev/rivers').then(res =>
      res.json()
    )
  },
}
```

---

# AsyncData Hook

<Steps>
- Unlike fetch, the promise returned by the asyncData hook is resolved during route transition. This means that no "loading placeholder" is visible during client-side transitions. Nuxt will instead wait for the asyncData hook to be finished before navigating to the next page or display the error page
- asyncData simply merges its return value into your component's local state
- This hook can only be used for page-level components
- Unlike fetch, asyncData cannot access the component instance (this)
</Steps>

---

# AsyncData Hook

```jsx
export default {
  async asyncData() {
    this.rivers = await fetch('https://api.nuxtjs.dev/rivers').then(res =>
      res.json()
    )
  },
}
```

---

# Handling Errors with Fetch API

With fetch API we always get a response even if the response is not ok

We should only show the response if it is ok

```jsx
if(response.ok){
	return response.json()
}
```

---

# Handling Errors with Fetch API

In order to catch an error in a try catch block we must first throw the new Error

```jsx
throw new Error(response.status)
```

---

# Exercise

<Steps>
1. Add a try/catch block
2. catch your errors
3. show an error message/component
</Steps>

```bash
https://api.nuxtjs.dev/rivers
```

---

# Solution

```jsx
export default {
  async asyncData() {
    try { 
			const rivers = await fetch('https://api.nuxtjs.dev/rivers').then(response => {
				if (response.ok) {
					return response.json()
	      }
				throw new Error('there was an error fetching data')
			})
			return { rivers }
    } catch (error) {
			return { error }
  },
}
```

---

# Solution

```html
<div v-if="error">There was an error</div>
<div v-else>....
```

---

# AsyncData with Axios

Import axios package and use it.

```jsx
import axios from 'axios'

export default {
  async asyncData() {
			const rivers = await axios.get('https://api.nuxtjs.dev/rivers').then(response => {
					return response.data
	    })
			return { rivers } 
	}
}
```

---

# AsyncData with Axios Module

Install it once and it can be used on any page

```bash
yarn add @nuxtjs/axios
```

```jsx
export default {
  modules: ['@nuxtjs/axios']
}
```

---

# AsyncData with Axios Module

- uses $ as helper
- returning data is easier

```jsx
export default {
  async asyncData( {$axios} ) {
			const rivers = await $axios.$get('https://api.nuxtjs.dev/rivers')
			return { rivers }
    }
}
```
<Notes>
Axios module
</Notes>


---

# Fetch Hook with Axios module

```jsx
export default {
	data() {
     return {
       rivers: []
     }
  },
  async fetch() {
			this.rivers = await this.$axios.$get('https://api.nuxtjs.dev/rivers')
			return { rivers }
    }
}
```

---

# Exercise

Create a page that fetches data from an api using AsyncData and axios module

```bash
https://api.nuxtjs.dev/rivers
```

---

# Solution

```bash
yarn add @nuxtjs/axios
```

```jsx
export default {
  modules: ['@nuxtjs/axios']
}
```

```jsx
export default {
  async asyncData( {$axios} ) {
			const rivers = await $axios.$get('https://api.nuxtjs.dev/rivers')
			return { rivers }
    }
}
```

---

# Creating Dynamic pages

To create a dynamic route you need to add an underscore before the .vue file name or before the name of the directory

`pages/_rivers.vue`

---

# Exercise

<Steps>
1. Create a dynamic page for the river details
2. Link from the river cards to the dynamic page and back
</Steps>

---

# Solution

```jsx
export default {
  async asyncData( {$axios, params} ) {
			const river = await $axios.$get(`https://api.nuxtjs.dev/rivers/${params.rivers}`')
			return { river }
    }
}
```

---

# Nuxt content, markdown, yaml, components in Markdown

Beer app works with Content

<Notes>
add a component and extra features as per blog post

how how live edit works
</Notes>


---

# Exercise

Re-create the beer App

---

# Solution

[https://github.com/debs-obrien/vue-toronto](https://github.com/debs-obrien/vue-toronto)

[http://nuxt-beers.surge.sh/](http://nuxt-beers.surge.sh/)

---

# Deploying your Nuxt.js Application

Let's share our code with the web

---

# target: 'server'

What commands are available?

<Notes>
run dev, build and start and then show the .nuxt folder
</Notes>

---

# target: 'static'

What commands are available?

<Notes>
run dev, build and start and then show the dist folder and that things are generated
</Notes>

---

# ssr: false

What happens when we don't want ssr

<Notes>
Add ssr false and build/generate our app depending on target. Show no content is generated
</Notes>

---

# Exercise

Deploy you application

---

# Solution

lets see it live

---

# Solution

---

# Nuxt plugins and modules, perhaps we will even create our own

modules.nuxtjs.org

<Notes>
chat about favourite modules
</Notes>

---

# Q and A

Question Time

---
# The End


---
