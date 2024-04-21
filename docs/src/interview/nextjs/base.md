# Next.js功能梳理

## Next.js了解篇｜一文带你梳理清楚Next.js的功能

https://juejin.cn/post/7206261082452639802

### Next.js 可以带给我们什么？

Next.js 是一个 React web 应用框架，这是官方对自己的定义，然后它主要做的事情有以下几点：

- 完善的工程化机制
- 良好的开发和构建性能
- 智能文件路由系统
- 多种渲染模式来保证页面性能体验
- 可扩展配置
- 提供其他多方面性能优化方案
- 提供性能数据，让开发者更好的分析性能。
- 提供的其他常用功能或者扩展，比如使用 mdx 来编写页面的功能等等。

#### 1.完善的工程化机制

你不需要自己去配置 webpack 方案，它已经内置了以下工程化基础：

- babel 内置，支持JS代码向后兼容
- postcss 内置，支持CSS代码向后兼容
- browserslist 支持配置兼容的浏览器信息，配合 babel 和 postcss 工作。
- TypeScript 可选择使用，保证代码的质量，以及可阅读性和可维护性。
- eslint 可选择使用，检测代码格式，可自定义规则。vscode 编写代码，或者build打包时都会有提示。
- prettier 可通过扩展使用，格式化代码，可自定义规则。
- css modules 内置
- css-in-js 可扩展使用
- tailwind css 可扩展使用

也做了一些打包优化功能：

- tree shaking
- 代码压缩
- 页面自动静态化
- 按需打包第三方 es 包（通过设置 transpilePackages 属性，让部分包可以被 next-babel 打包）
- 异步动态加载组件，和 React.lazy 功能一样，只不过实现得更早。

基本上使用了 Next.js 你不需要再去处理工程化相关事项。也可以通过很简单的方式去优化打包性能，且每次构建都会输出页面资源大小信息，如下图所示：

![nextjs0](/assets/images/nextjs0.png)

如果你需要具体的分析JS资源的组成，那么可以使用 @next/bundle-analyzer 来分析，其可具体分析服务端代码、edge运行时代吗、客户端代码

#### 2.良好的开发和构建性能

去年 next.js 13 发布会上，官方公布了 next.js 使用 turbopack 进行编译打包，称其编译速度极快，将是 webpack 的继承者，感兴趣的同学可以去看一看，我记得有翻译版本，我们就不去测试它到底有多快了，反正就是很快。我是从 next.js 10 直接切到的 13，那感觉真的不要太爽，启动秒启，首次打开页面也秒开，因为开发模式 next.js 都是动态编译，启动的时候并不会去编译页面，访问才去动态编译，而且因为是多页应用，页面非共用部分互不影响，不会因为工程的业务模块变多而导致页面访问变慢。

构建性能也很快，以我前面打包的工程为例，我算了一下，十几个页面，全部打包只用了 10.52 秒，有两个页面还动态请求了接口，不然还会更短一些。

过我听说 windows 上开发性能或者打包性能可能有一点问题，可以注意一下。

#### 3.智能文件路由系统

它在一定程度上规范了代码组织结构，这也是比较重要的。

#### 4.多种渲染模式来保证页面性能体验

渲染模式是决定页面性能很重要的因素，也是 Next.js 最核心的一部分

#### 5.可扩展配置

Next.js 的可配置性真的是一个很强大的特色，它准备了一套默认就很好用的默认配置，但这些配置基本上用户都可以 完全 控制它（完全做一个保留，但大型工程基本上都是可以支撑的）。
下面我们来分析一下它的可配置功能。

- 配置文件 next.config.js 中暴露了 webpack 实例，因此你可以完全控制 webpack
- 配置文件 next.config.js 中支持配置自定义配置，你可以把一些公用的不变的配置写在 serverRuntimeConfig 或者 publicRuntimeConfig 中，前者只会出现在服务端，后者会暴露到客户端。
- 可 自定义 server ，你可以在启动服务的时候做一些自己想要做的处理，比如 node.js 性能监控等等。
- 不自定义 server ，也可以使用它提供的 middreware 机制来拦截请求或者校验权限等事项。
- 自定义 APP，也就是 _app.js，它用于处理多个页面公共部分。
- 自定义 Document，也就是_document.js，用于自定义配置 html 生成内容，比如插入 Google 分析脚本。
- 自定义错误界面 也就是 404 或者 500 错误状态的页面。
- 自定义页面 head 属性，使用
- next/head 提供的 Head 组件，用于自定义 html document 头部的 title/meta/base 等标签信息。
- 可自定义 babel 和 postcss 等工程化规则配置。

可以看出基本上各个方面都可以自定义配置，扩展性还是很强的。

#### 6.提供其他多方面性能优化方案

提供了其他方面的优化方案：

- 图片优化：大部分页面基本上都会有或多或少的图片，图片往往是影响页面性能体验的重要因素之一，因为现在一张图片的体积很可能比页面的所有代码体积还打，因此关注性能就一定要关注图片。
- 字体优化：字体文件一般也比较大，这一块还不是很了解，因为时间问题，后续了解完再更新一下。

#### 提供性能数据

Next.js 提供了获取应用性能数据的方法 reportWebVitals, 只能在 App 组件中使用。

#### 提供的其他常用功能或扩展

其他功能：
- API Routes
- next/amp: 用于支持开发 AMP 应用。

扩展：
@next/mdx: 用于支持使用 mdx 来编写页面，也就是如果要开发一个 md 的文档工程，直接接入它即可。


## Next.js 13 的 app 目录模式功能梳理

https://juejin.cn/post/7221162775074734135


### Next.js app 目录模式的功能梳理

在梳理 Next.js 13 新目录模式之前，先来简单梳理一下旧的目录模式几个痛点：

1. pages 目录的 js 文件全都会当成页面文件，导致组件不能写在 pages 目录下，使用起来不符合大部分人的代码组织习惯，虽然有一些方法可以处理，但仍然是一个不友好的点。
2. 几个入门级的 api ，比如 getInitialProps/getServerSideProps/getStaticProps 等等方法的使用并不是那么简单，不去深入了解渲染模式，对于 Next.js 初学者来说，不容易理解。
3. 服务端渲染和客户端渲染的代码有时候耦合会太深，有时候不好分清楚代码是在服务端渲染时执行的还是在客户端渲染时执行的，也容易出现一些错误，导致页面首次渲染时出现 hydrate 异常，这一点和第 2 点也有一些关系。

Next.js app 目录模式相对于旧模式的功能列表说明：

1. 完善的工程化机制：变化较少
2. 良好的开发和构建性能：变化较少
3. 智能文件路由系统：app 目录模式完全解决了痛点 1 描述的问题，且极大的增强了代码的组织能力。
4. 多种渲染模式来保证页面性能体验：使用更加简单的方式让开发者来进行数据请求，且提供了数据缓存方式，以便于更方便的实现多种渲染模式，解决痛点 2 和 3 描述的问题，且带来了更好的客户端性能（尽量减少客户端需要加载的JS资源）。
5. 可扩展配置：提供了更加友好的配置方式，增强工程配置能力。
6. 提供其他多方面性能优化方案：变化较少
7. 提供性能数据，让开发者更好的分析性能：变化较少
8. 提供的其他常用功能或者扩展：供了更多更好的扩展方式。

其中最核心的变化就是第3、4点，下面对这两点单独进行简要的说明，更详细的内容还在写作中，结尾会有预告。

### 智能文件路由系统

#### 约定页面相关内容

#### 平行路由和插槽功能

#### 约定 web api 路由实现

### 多种渲染模式来保证页面性能体验

*默认的js文件都只会运行在服务端，不会出现在客户端，如果需要在客户端进行交互的组件，那么需要在 js 文件最顶部添加 "use client" 来标识，表明代码需要在客户端运行，这时候这部分代码才会出现在客户端。*

也就是代码默认只在服务端，那怎么实现多种渲染模式呢？

- SSG：页面默认就是 SSG
- CSR：在使用 "use client" 的客户端组件中进行请求数据，也是基于SSG，然后在客户端 hydrate 后进行请求数据更新页面内容。
- SSR：服务端组件声明为异步组件，也就是 async 函数组件，且数据请求关闭缓存，也就是fetch请求时第二个参数中的cache字段设置为 no-store 、 no-cache 或者 设置revalidate 为 0 的时候，才会是动态服务端渲染。
- ISR：在请求中设置 revalidate ，或者在 page.js 中设置 revalidate :export revalidate = 60 60秒进行增量静态化，也可以继续使用 pages/api/revalidate 的指令方式，需要注意还是需要写在 pages 目录。

其实 app 模式中不再需要根据一些默认导出函数来决定函数的渲染方式，但也需要注意页面到底使用的是哪种渲染模式，才能对整个应用的页面的性能有一定的把控。

## 你好，Next.js 13

https://juejin.cn/post/7160084572942630926

### Turbopack

urbopack 特点

- 开箱即用 TypeScript, JSX, CSS, CSS Modules, WebAssembly 等
- 增量计算： Turbopack 是建立在 Turbo 之上的，Turbo 是基于 Rust 的开源、增量记忆化框架，除了可以缓存代码，还可以缓存函数运行结果。
- 懒编译：例如，如果访问 localhost:3000，它将仅打包 pages/index.jsx，以及导入的模块。

为什么不选择 Vite 和 Esbuild？

Vite 依赖于浏览器的原生 ES Modules 系统，不需要打包代码，这种方法只需要转换单个 JS 文件，响应更新很快，但是如果文件过多，这种方式会导致浏览器大量级联网络请求，会导致启动时间相对较慢。所以作者选择同 webpack 一样方式，打包，但是使用了 Turbo 构建引擎，一个增量记忆化框架，永远不会重复相同的工作。

Esbuild 是一个非常快速的打包工具，但它并没有做太多的缓存，也没有 HMR（热更新），所以在开发环境下不适用。

### 为什么要改基于文件的路由系统

Next 13 另一个比较大的改动是基于文件的路由系统，增加了一个 app 目录，每一层路由必须建一个文件夹，在该文件夹中建立 page.tsx 作为该路由主页面。

主要有以下 3 点原因:

- 实现嵌套路由和持久化缓存
- 支持 React 18 中的 React server Component，实现 Streaming（流渲染）
- 实现代码目录分组，将当前路由下的测试文件、组件、样式文件友好地放在一起，避免全局查找

### app 文件夹下的约定式路由

Next13 新增了 app 文件夹 来实现约定式路由，完美地实现了持久化缓存。

路由分组

app 同层级目录下还支持多个 layout， 使用 （文件夹）区分，（文件夹）不会体现在路由上，只是单纯用来做代码分组。

```
./app
├── (checkout)
│   ├── checkout
│   │   └── page.tsx
│   ├── layout.tsx
│   └── template.tsx
├── (main)
│   ├── layout.tsx
│   ├── page.tsx
│   └── template.tsx
```
### React Server Components

