# vue-tab

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

For detailed explanation on how things work, consult the [docs for vue-loader](http://vuejs.github.io/vue-loader).


## options

Props | Type | Description | Default
:-: | :-: | :-: | :-: 
list | Array | 分类列表 | 
offset | Number | 当前 tab 距离屏幕左边的偏移量(px) | 130
newIndex | Number | 跳至新索引 | 0

Event | params | description
:-: | :-: | :-: |
indexChange | curentIndex | 点击tab时触发
