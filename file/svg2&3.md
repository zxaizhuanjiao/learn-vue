### 使用Vue3实现svg-icon的全局引入(未使用TS，只是在原Vue2升级)   
1、安装   
npm install svg-sprite-loader -S   
2、配置文件vue.config.js   
const path = require('path')
function resolve(dir) {
  return path.join(__dirname, dir)
}

module.exports={
  chainWebpack: config => {
    // set svg-sprite-loader
    config.module
      .rule('svg')
      .exclude.add(resolve('src/icons'))
      .end()
    config.module
      .rule('icons')
      .test(/\.svg$/)
      .include.add(resolve('src/icons'))
      .end()
      .use('svg-sprite-loader')
      .loader('svg-sprite-loader')
      .options({
        symbolId: 'icon-[name]'
      })
      .end()
  },
}   
或者   
config.module.rules.delete("svg"); //重点:删除默认配置中处理svg,
		config.module
		    .rule('svg-sprite-loader')
		    .test(/\.svg$/)
		    .include
		    .add(resolve('src/icons')) // 处理svg目录
		    .end()
		    .use('svg-sprite-loader')
		    .loader('svg-sprite-loader')
		    .options({
		        symbolId: 'icon-[name]'
		    })   
3、创建index.js文件，用来解析svg图片   
在src下新建icons文件夹，icons文件夹下新建svg文件夹和index.js,svg文件夹是用来存放svg图片   
如图：   
![index.js文件](image/svg1.png)    
(vue2)vue2中对应的index.js(vue2)   
import Vue from 'vue'    
import SvgIcon from '@/components/SvgIcon'// svg component   
// register globally   
// 可以直接注册全局组件   
Vue.component('svg-icon', SvgIcon)   
const req = require.context('./svg', false, /\.svg$/)   
const requireAll = (requireContext) => requireContext.keys().map(requireContext)   
requireAll(req)   
(vue3)vue3中对应的index.js(vue3)  
const req = require.context('./svg', false, /\.svg$/)   
const requireAll = (requireContext) => requireContext.keys().map(requireContext)   
requireAll(req)   
4、创建vue模板   
如图：    
![component下](image/svg2.png)   
参考my-vue3-demo/componets/svgIcon/index.vue   
5、main.js中引入并注册成全局   
(vue2)vue2中对应的main.js配置，因为在index.js已经将组件注册为全局，在main.js引入index.js即可(vue2)   
import './icons' // icon   
(vue3)vue3对应的main.js配置(vue3)   
import { createApp } from 'vue'   
import App from './App.vue'   
import './icons' // icon   
// 在main.js配置对应的组件，不能在index.js中进行配置，不然会出现空白   
import SvgIcon from '@/components/SvgIcon/index.vue' // svg component   
createApp(App).component('SvgIcon', SvgIcon).mount('#app')   
总结：v3和v2区别   
（1）v3需要使用createApp api，不能像v2一样，直接引入Vue   
（2）注册组件时，v3需要   
createApp(App).component('SvgIcon', SvgIcon).mount('#app')   
v2直接Vue.component('svg-icon', SvgIcon)   

转载自：https://blog.csdn.net/qq_28846389/article/details/117023902








