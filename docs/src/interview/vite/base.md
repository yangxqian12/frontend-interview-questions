# Vite

## 

https://juejin.cn/post/7207659644487893051

## 如何指定 vite 插件 的执行顺序？

可以使用 enforce 修饰符来强制插件的位置:

pre：在 Vite 核心插件之前调用该插件
post：在 Vite 构建插件之后调用该插件

##  Vite是否支持 commonjs 写法？

纯业务代码，一般建议采用 ESM 写法。如果引入的三方组件或者三方库采用了 CJS 写法，vite 在预构建的时候就会将 CJS 模块转化为 ESM 模块。

## 为什么说 vite 比 webpack 要快

vite 不需要做全量的打包
vite 在解析模块依赖关系时，利用了 esbuild，更快（esbuild 使用 Go 编写，并且比以 JavaScript 编写的打包器预构建依赖快 10-100 倍）
按需加载：在HMR（热更新）方面，当改动了一个模块后，vite 仅需让浏览器重新请求该模块即可，不像 webpack 那样需要把该模块的相关依赖模块全部编译一次，效率更高。
由于现代浏览器本身就支持 ES Module，会自动向依赖的Module发出请求。vite充分利用这一点，将开发环境下的模块文件，就作为浏览器要执行的文件，而不是像 webpack 那样进行打包合并。
按需编译：当浏览器请求某个模块时，再根据需要对模块内容进行编译，这种按需动态编译的方式，极大的缩减了编译时间。
webpack 是先打包再启动开发服务器，vite 是直接启动开发服务器，然后按需编译依赖文件。由于 vite在启动的时候不需要打包，也就意味着不需要分析模块的依赖、不需要编译，因此启动速度非常快。

## vite 对比 webpack ，优缺点在哪？
（1）优点：

更快的冷启动： Vite 借助了浏览器对 ESM 规范的支持，采取了与 Webpack 完全不同的 unbundle 机制
更快的热更新： Vite 采用 unbundle 机制，所以 dev server 在监听到文件发生变化以后，只需要通过 ws 连接通知浏览器去重新加载变化的文件，剩下的工作就交给浏览器去做了。

（2）缺点：

开发环境下首屏加载变慢：由于 unbundle 机制， Vite 首屏期间需要额外做其它工作。不过首屏性能差只发生在 dev server 启动以后第一次加载页面时发生。之后再 reload 页面时，首屏性能会好很多。原因是 dev server 会将之前已经完成转换的内容缓存起来
开发环境下懒加载变慢：由于 unbundle 机制，动态加载的文件，需要做 resolve 、 load 、 transform 、 parse 操作，并且还有大量的 http 请求，导致懒加载性能也受到影响。
webpack支持的更广：由于 Vite 基于ES Module，所以代码中不可以使用CommonJs；webpack更多的关注兼容性, 而 Vite 关注浏览器端的开发体验。


当需要打包到生产环境时，Vite使用传统的rollup进行打包，所以，vite的优势是体现在开发阶段，缺点也只是在开发阶段存在。

## Vite和webpack的区别
Vite 和 Webpack 都是现代化的前端构建工具，它们可以帮助开发者优化前端项目的构建和性能。虽然它们的目标是相似的，但它们在设计和实现方面有许多不同之处。

两者原理图：

区别如下：
（1）构建原理： Webpack 是一个静态模块打包器，通过对项目中的 JavaScript、CSS、图片等文件进行分析，生成对应的静态资源，并且可以通过一些插件和加载器来实现各种功能；Vite 则是一种基于浏览器原生 ES 模块解析的构建工具。
（2）打包速度： Webpack 的打包速度相对较慢，Vite 的打包速度非常快。
（3）配置难度： Webpack 的配置比较复杂，因为它需要通过各种插件和加载器来实现各种功能；Vite 的配置相对简单，它可以根据不同的开发场景自动配置相应的环境变量和配置选项。
（4）插件和加载器： Webpack 有大量的插件和加载器可以使用，可以实现各种复杂的构建场景，例如代码分割、按需加载、CSS 预处理器等；Vite 的插件和加载器相对较少
（5）Vite是按需加载，webpack是全部加载： 在HMR（热更新）方面，当改动了一个模块后，vite仅需让浏览器重新请求该模块即可，不像webpack那样需要把该模块的相关依赖模块全部编译一次，效率更高。
（6）webpack是先打包再启动开发服务器，vite是直接启动开发服务器，然后按需编译依赖文件 由于vite在启动的时候不需要打包，也就意味着不需要分析模块的依赖、不需要编译，因此启动速度非常快。当浏览器请求某个模块时，再根据需要对模块内容进行编译，这种按需动态编译的方式，极大的缩减了编译时间。

## Vite 常见的配置

https://juejin.cn/post/7207659644487893051

（1）css.preprocessorOptions： 传递给 CSS 预处理器的配置选项，例如，我们可以定义一个全局变量文件，然后再引入这个文件：

（2）css.postcss： PostCSS 也是用来处理 CSS 的，只不过它更像是一个工具箱，可以添加各种插件来处理 CSS（解决浏览器样式兼容问题、浏览器适配等问题）。例如：移动端使用 postcss-px-to-viewport 对不同设备进行布局适配：

（3）resolve.alias： 定义路径别名也是我们常用的一个功能，我们通常会给 scr 定义一个路径别名：

（4）resolve.extensions： 导入时想要省略的扩展名列表。默认值为 ['.mjs', '.js', '.ts', '.jsx', '.tsx', '.json'] 。

（5）optimizeDeps.force： 是否开启强制依赖预构建。node_modules 中的依赖模块构建过一次就会缓存在 node_modules/.vite/deps 文件夹下，下一次会直接使用缓存的文件。而有时候我们想要修改依赖模块的代码，做一些测试或者打个补丁，这时候就要用到强制依赖预构建。
javascript复制代码

（6）server.host： 指定服务器监听哪个 IP 地址。默认值为 localhost ，只会监听本地的 127.0.0.1。

（7）server.proxy： 反向代理也是我们经常会用到的一个功能，通常我们使用它来解决跨域问题：

（8）base： 开发或生产环境服务的公共基础路径。

（9）build.outdir： 指定打包文件的输出目录，默认值为 dist。

（10）build.assetsDir： 指定生成静态资源的存放目录，默认值为 assets。

（11）build.assetsInlineLimit： 图片转 base64 编码的阈值。为防止过多的 http 请求，Vite 会将小于此阈值的图片转为 base64 格式，可根据实际需求进行调整。

（12）plugins： 插件相信大家都不陌生了，我们可以使用官方插件，也可以社区插件。