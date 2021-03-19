# vintri-frontend

## Introduction

`vintri-frontend` application to visualize data through Veu.js UI.
* Start date: 202103190130
* V0: 202103190135 Project setup.
* V1: 202103190116 Integrating API.
* V2: 202103190320 UI tested.

## Axios
* Since lack of time, I decided to just integrate localhost URLs for testing without any config files, and just basic UIs.


## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

## Use case
* Clone API: https://github.com/wyjwyj1104/vintri-backend
* Run /vintri_backend on http://localhost:3000
* Run /vintri_frontend
* Go to http://localhost:8080/
* Use search box at the top of the page to search for names including the searching keywords.
* Click on the rating column cells to edit and update their rating from 1-5.

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
