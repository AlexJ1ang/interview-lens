# 前端开发岗题库与出题方法论

## 一、出题方法论

- **一面（基础）**：HTML/CSS/JS 核心、浏览器原理、框架基础——考广度与准确度。
- **二面（深化）**：实现机制、工程化、性能优化和故障定位；若用户选择项目深挖模式，再以项目为主线考察。
- **三面（架构/主管）**：前端架构、跨端、技术选型、团队协作、领导力。

## 二、高频考点清单

### 1. JavaScript 核心
- 数据类型与类型转换、== 与 ===、闭包、作用域链、原型链、this 指向
- 事件循环（Event Loop）、宏任务/微任务、Promise/async-await
- 深浅拷贝、防抖节流、数组/对象常用方法的行为与使用边界
- ES6+：解构、展开、class、箭头函数、模块化（ESM/CommonJS）、Symbol、迭代器/生成器

### 2. HTML / CSS
- 盒模型、BFC、flex/grid 布局、水平垂直居中、重排重绘、合成层
- 响应式与移动端适配（rem/vw/媒体查询/刘海屏）
- 语义化标签、SEO 基础、HTML5 新特性
- 常见场景：圣杯/双飞翼布局、1px 问题、CSS 性能

### 3. 浏览器原理
- 从输入 URL 到页面展示的完整过程、DNS、TCP、HTTP、渲染流程
- 浏览器缓存（强缓存/协商缓存）、跨域（CORS/JSONP/proxy/postMessage）、Cookie/Storage/Token
- 回流（reflow）与重绘（repaint）、图层合成、白屏优化
- V8：垃圾回收、JIT、v8 内存管理

### 4. 框架（Vue / React 选配）
- Vue：响应式原理（Proxy/defineProperty）、虚拟 DOM、diff 算法、组件通信、生命周期、watch/computed 区别、nextTick、keep-alive、v-model 原理、Pinia/Vuex
- React：函数组件与 Hook、useState/useEffect 原理、React.memo、useMemo/useCallback、Fiber 架构、调度机制、合成事件、HOC/Render Props、useRef、状态管理（Redux/Zustand）
- 追问深挖：diff 时间复杂度、key 的作用、为什么用虚拟 DOM、React 18 并发特性

### 5. 构建与工程化
- Webpack/Vite 原理：loader 与 plugin、热更新（HMR）、tree-shaking、代码分割、性能优化
- 模块化规范、npm/yarn/pnpm 包管理、husky/lint 规范、CICD
- 微前端、Monorepo

### 6. 其他高频
- 前端安全：XSS、CSRF、点击劫持、防注入
- 性能优化：首屏优化、图片懒加载、资源预加载（preload/prefetch）、长列表虚拟滚动
- 网络：HTTP 缓存、HTTP2/3、WebSocket、SSE
- TypeScript：类型系统、泛型、工具类型、与 JS 对比

## 三、示例题与答案要点

**例 1：以浏览器环境为前提，说说事件循环，下面代码输出顺序？**
```js
console.log(1)
setTimeout(() => console.log(2), 0)
new Promise(r => { console.log(3); r() }).then(() => console.log(4))
console.log(5)
```
答案要点：输出 1 3 5 4 2。同步代码 → 微任务（then）→ 后续任务（setTimeout）。追问：async/await 和 queueMicrotask；只有明确切换到 Node.js 环境时才讨论 process.nextTick，并先说明版本。

**例 2：Vue 的响应式原理？为什么 Vue3 用 Proxy？**
答案要点：Vue2 的响应式核心基于 Object.defineProperty，Vue3 对对象使用 Proxy。Proxy 能覆盖属性新增/删除、数组索引和 Map/Set 等操作，减少 Vue2 需要专门 API 处理的限制；不要笼统断言所有场景都“性能更优”。追问：Proxy 的局限、reactive vs ref、依赖收集与触发更新。

**例 3：从输入 URL 到页面渲染发生了什么？**
答案要点：DNS 解析 → TCP 连接（TLS）→ 发 HTTP 请求 → 服务器响应 → 浏览器解析 HTML 构建 DOM → 构建 CSSOM → 合成渲染树 → 布局 → 绘制 → 合成。追问：DNS 细节、渲染阻塞（CSS/JS）、关键渲染路径优化、回流触发条件。

**例 4：说说浏览器缓存机制。**
答案要点：强缓存（Cache-Control: max-age、Expires）+ 协商缓存（ETag/If-None-Match、Last-Modified/If-Modified-Since）。追问：两者优先级、ETag vs Last-Modified、如何让文件长期缓存 + 更新及时生效（文件名 hash）。

**例 5：React 的 Fiber 是什么？解决了什么？**
答案要点：Fiber 是 React 内部的工作单元与树结构表示，使渲染工作能够被拆分、调度并按优先级处理，为可中断渲染和并发特性提供基础。不要把它简单等同为“一个链表”或承诺所有渲染都不会阻塞。追问：双缓冲树、调度优先级、并发渲染与提交阶段的区别。

**例 6：解释防抖和节流的区别，并评审一段已有实现。**
答案要点：防抖通常在连续触发停止一段时间后执行，节流限制一段时间内的执行频率。结合搜索输入、滚动或拖拽说明选择，并检查立即执行、尾调用、取消和组件卸载等边界；不要求现场手写实现。

## 四、追问技巧

- 答对概念后，按目标层级追问实现机制或设计取舍；只有经历和 JD 确实要求源码经验时才深入源码。
- 给真实场景：如"列表 10 万条怎么优化渲染""首屏白屏怎么查"。
- 用代码阅读、运行结果判断、性能剖析或故障日志验证理解，不要求现场手写代码。
- 考察工程判断："这个方案还有什么坑""线上出了问题怎么定位"。
