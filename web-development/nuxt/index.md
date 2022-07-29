# Nuxt notes

Nuxt - v2.15.8
Static site generation with dynamic pages `/blog/_slug.vue` and `/blog/page/_id.vue`.

In `nuxt.config.js` we add the following:
```diff-javascript
+ import fetchRoutes from './api/blog.js'

export default {
    target: 'static',
    env: {
+       PER_PAGE: process.env.PER_PAGE || 8
    },
    generate: {
        // removes redirect to pages with trailing slash
        subFolders: false,
        // deactivates crawler
        crawler: false,
        // enable custom error page
        fallback: true,
+       routes() {
+           return fetchRoutes()
+       }
    },
}
```
`fetchRoutes` function will return an array with the routes in this format `/blog/:slug` and `/blog/page/:id` that Nuxt uses to generate the html files.
These two formats represent the dynamic blog post routes and the blog pagination routes.

If we want to, after we have called our API to fetch some data, we can also build a route object with the response data and attatch a payload. We then make sure to push it to the array `global_routes` that our `fetchRoutes` function will return.

It will look something like this:
```js
// ./api/blog.js

let fetchRoutes = async () => {

    let global_routes = []

    await axios.get(url).then((response) => {
            global_routes.push({
                route: response.full_slug,
                payload: response.content
            })
        })

    return global_routes
}
```
