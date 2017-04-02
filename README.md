# NC State Parks

> A North Carolina state parks road trip planner made using the Google Maps API and Vue.js with WebPack.

## About this project
Created by Kate Boyd.
Portfolio at [kwboyd.com](http://kwboyd.com).
App viewable [here](http://kwboyd.com/ncparks/).
This project helps users learn about North Carolina state parks and plan potential road trips to each of them. I created this app because of my passion for local parks and the natural beauty of North Carolina that often goes unappreciated. I didn't realize before I began creating this app that no matter where you live in NC, there are at least a few parks within driving distance for a day trip. My hope is that this app will allow others to realize this too.

### Technical information
This app was built using Vue.js and Webpack. The parks' information is stored in a JSON that is loaded in through Axios. Adding a park to the trip sends a request to the Google Maps Directions API, which sends back legs of the route. The app draws a polyline on the map based on those legs and also adds up the total time and distance based on the API response. I also used Bulma as a CSS framework.

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For detailed explanation on how things work, checkout the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
