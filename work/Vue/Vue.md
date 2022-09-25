# 1、vue概述

Vue (读音/vju/, 类似于view)是一套用于构建用户界面的渐进式JavaScript框架，发布于2014年2月。与其它大型框架不同的是，Vue被设计为可以自底向上逐层应用。Vue的核心库只关注视图层，不仅易于上手，还便于与第三方库(如: vue-router: 跳转，vue-resource: 通信，vuex:管理)或既有项目整合

官网：https://cn.vuejs.org/v2/guide/



**vue.js优点**

1. 体积小

   压缩后33K

2. 更高的运行效率

   基于虚拟dom，一种可以预先通过JavaScript进行各种计算，把最终的dom操作计算出来并优化的技术，由于这个dom操作时预处理，并没有真实操作dom，所以叫做虚拟dom

3. 双向数据绑定

   让开发者不用再去操作dom对象，把更多的精力投入到业务逻辑上。

4. 生态丰富，学习成本低

   市场上拥有大量成熟、稳定的基于vue.js的ui框架，常用组件，拿来即用实现快速开发！

   对初学者友好，入门容易，学习资料多



**vue.js -- 前端开发人员必修技能**

1. 使用场景广泛

   被广泛的应用于web端，移动端，跨平台应用开发

2. 招聘市场需求大，前景较好



# 2、前端知识体系

## 2.1、前端三要素

- HTML (结构) :超文本标记语言(Hyper Text Markup Language) ，决定网页的结构和内容
- CSS (表现) :层叠样式表(Cascading Style sheets) ，设定网页的表现样式
- JavaScript (行为) :是一种弱类型脚本语言，其源代码不需经过编译，而是由浏览器解释运行,用于控制网页的行为

## 2.2、结构层（html）

太简单，略

## 2.3、表现层（css）

CSS层叠样式表是一门标记语言，并不是编程语言，因此不可以自定义变量，不可以引用等，换句话说就是不具备任何语法支持，它主要缺陷如下：

- 语法不够强大，比如无法嵌套书写，导致模块化开发中需要书写很多重复的选择器；

- 没有变量和合理的样式复用机制，使得逻辑上相关的属性值必须以字面量的形式重复输出，导致难以维护；

这就导致了我们在工作中无端增加了许多工作量。为了解决这个问题，前端开发人员会使用一种称之为**【CSS预处理器】**的工具,提供CSS缺失的样式层复用机制、减少冗余代码，提高样式代码的可维护性。大大的提高了前端在样式上的开发效率。



> 什么是CSS预处理器？

CSS预处理器定义了一种新的语言，其基本思想是，用一种专门的编程语言，为CSS增加了一些编程的特性，将CSS作为目标生成文件，然后开发者就只需要使用这种语言进行CSS的编码工作。转化成通俗易懂的话来说就是“**用一种专门的编程语言，进行Web页面样式设计，再通过编译器转化为正常的CSS文件，以供项目使用**”。



> 常用的CSS预处理器有哪些？

- SASS：基于Ruby ，通过服务端处理，功能强大。解析效率高。需要学习Ruby语言，上手难度高于LESS。
- LESS：基于NodeJS，通过客户端处理，使用简单。功能比SASS简单，解析效率也低于SASS，但在实际开发中足够了，所以如果我们后台人员如果需要的话，建议使用LESS。



## 2.4、行为层（JavaScript）

JavaScript一门弱类型脚本语言，其源代码在发往客户端运行之前不需要经过编译，而是将文本格式的字符代码发送给浏览器，由浏览器解释运行。



> Native 原生JS开发

原生JS开发，也就是让我们按照【ECMAScript】标准的开发方式，简称ES，特点是所有浏览器都支持。截至到当前，ES标准以发布如下版本：

- es3
- es4（内部，未正式发布）
- es5（全浏览器支持）
- es6（常用，当前主流版本：webpack打包成为ES5支持）
- es7
- es8
- es9（草案阶段）

区别就是逐步增加新特性。



> TypeScript 微软的标准

TypeScript是一种由微软开发的自由和开源的编程语言。它是JavaScript的一个超集， 而且本质上向这个语言添加了可选的静态类型和基于类的面向对象编程。由安德斯·海尔斯伯格(C#、Delphi、TypeScript之父； .NET创立者) 主导。该语言的特点就是除了具备ES的特性之外还纳入了许多不在标准范围内的新特性，所以会导致很多浏览器不能直接支持TypeScript语法， 需要编译后(编译成JS) 才能被浏览器正确执行。



> JavaScript框架

- JQuery：大家熟知的JavaScript库，优点就是简化了DOM操作，缺点就是DOM操作太频繁，影响前端性能；在前端眼里使用它仅仅是为了兼容IE6，7，8
- Angular：Google收购的前端框架，由一群Java程序员开发，其特点是将后台的MVC模式搬到了前端并增加了**模块化开发**的理念，与微软合作，采用了TypeScript语法开发；对后台程序员友好，对前端程序员不太友好；最大的缺点是版本迭代不合理（如1代–>2 代，除了名字，基本就是两个东西；截止发表博客时已推出了Angular6）
- React：Facebook 出品，一款高性能的JS前端框架；特点是提出了新概念 【**虚拟DOM**】用于减少真实 DOM 操作，在内存中模拟 DOM操作，有效的提升了前端渲染效率；缺点是使用复杂，因为需要额外学习一门【JSX】语言
- Vue：一款渐进式 JavaScript 框架，所谓渐进式就是逐步实现新特性的意思，如实现模块化开发、路由、状态管理等新特性。其特点是综合了 Angular（模块化）和React(虚拟 DOM) 的优点
- Axios：前端通信框架；因为 Vue 的边界很明确，就是为了处理 DOM，所以并不具备通信能力，此时就需要额外使用一个通信框架与服务器交互；当然也可以直接选择使用jQuery 提供的AJAX 通信功能



> UI框架

- Ant-Design：阿里巴巴出品，基于React的UI框架
- ElementUI、iview、ice：饿了么出品，基于Vue的UI框架
- BootStrap：Teitter推出的一个用于前端开发的开源工具包
- AmazeUI：又叫“妹子UI”，一款HTML5跨屏前端框架

> JavaScript构建工具

- Babel：JS编译工具，主要用于浏览器不支持的ES新特性，比如用于编译TypeScript
- WebPack：模块打包器，主要作用就是打包、压缩、合并及按序加载

## 2.5、三端统一

> 混合开发（Hybrid APP）

主要目的是实现一套代码三端统一（PC、Android【.apk】、iOS【.ipa】）并能够调用到设备底层硬件（如：传感器、GPS、摄像头等），打包方式主要有以下两种：

- 云打包：HBuild -> HBuildX，DCloud 出品；API Cloud
- 本地打包： Cordova（前身是 PhoneGap）



> 微信小程序

详见微信官网，这里就是介绍一个方便微信小程序UI开发的框架：WeUI

## 2.6、后端技术

前端人员为了方便开发也需要掌握一定的后端技术但我们Java后台人员知道后台知识体系极其庞大复杂，所以为了方便前端人员开发后台应用，就出现了NodeJS这样的技术。

NodeJS的作者已经声称放弃NodeJS(说是架构做的不好再加上笨重的node modules，可能让作者不爽了吧)，开始开发全新架构的Deno



既然是后台技术，那肯定也需要框架和项目管理工具， NodeJS框架及项目管理工具如下：

- Express：NodeJS框架
- Koa：Express简化版
- NPM：项目综合管理工具，类似于Maven
- YARN：NPM的替代方案，类似于Maven和Gradle的关系

 

## 2.7、主流前端框架

Vue.js



> iView

iview是一个强大的基于Vue的UI库， 有很多实用的基础组件比element ui的组件更丰富， 主要服务于PC界面的中后台产品。使用单文件的Vue组件化开发模式基于npm+webpack+babel开发， 支持ES 2015高质量、功能丰富友好的API， 自由灵活地使用空间。
备注：属于前端主流框架，选型时可考虑使用，主要特点是移动端支持较多

> Element UI

Element是饿了么前端开源维护的Vue UI组件库， 组件齐全， 基本涵盖后台所需的所有组件，文档讲解详细， 例子也很丰富。主要用于开发PC端的页面， 是一个质量比较高的Vue UI组件库。
备注：属于前端主流框架，选型时可考虑使用，主要特点是桌面端支持较多

> ICE

飞冰是阿里巴巴团队基于React/Angular/Vue的中后台应用解决方案， 在阿里巴巴内部， 已经有270多个来自几乎所有BU的项目在使用。飞冰包含了一条从设计端到开发端的完整链路，帮助用户快速搭建属于自己的中后台应用。
备注：主要组件还是以React为主， 截止2019年02月17日更新博客前对Vue的支持还不太完善，目前尚处于观望阶段

> VantUI

Vant UI是有赞前端团队基于有赞统一的规范实现的Vue组件库， 提供了-整套UI基础组件和业务组件。通过Vant， 可以快速搭建出风格统一的页面，提升开发效率。

> at-ui

at-ui是一款基于Vue 2.x的前端UI组件库， 主要用于快速开发PC网站产品。它提供了一套n pm+web pack+babel前端开发工作流程， CSS样式独立， 即使采用不同的框架实现都能保持统一的UI风格。

> Cube Ul

cube-ui是滴滴团队开发的基于Vue js实现的精致移动端组件库。支持按需引入和后编译， 轻量灵活；扩展性强，可以方便地基于现有组件实现二次开发。

> Flutter

Flutter是谷歌的移动端UI框架， 可在极短的时间内构建Android和iOS上高质量的原生级应用。Flutter可与现有代码一起工作， 它被世界各地的开发者和组织使用， 并且Flutter是免费和开源的。
备注：Google出品， 主要特点是快速构建原生APP应用程序， 如做混合应用该框架为必选框架

> WeUI

WeUI是一套同微信原生视觉体验一致的基础样式库， 由微信官方设计团队为微信内网页和微信小程序量身设计， 令用户的使用感知更加统一。包含button、cell、dialog、toast、article、icon等各式元素。

# 3、了解前后分离的演变史

## 3.1、后端为主的mvc时代

为了降低开发的复杂度， 以后端为出发点， 比如：Struts、Spring MVC等框架的使用， 就是后端的MVC时代；

以SpringMVC流程为例：

![image-20220626001826219](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220626001826219.png)

- 发起请求到前端控制器(DispatcherServlet)
- 前端控制器请求HandlerMapping查找Handler，可以根据xml配置、注解进行查找
- 处理器映射器HandlerMapping向前端控制器返回Handler
- 前端控制器调用处理器适配器去执行Handler
- 处理器适配器去执行Handler
- Handler执行完成给适配器返回ModelAndView
- 处理器适配器向前端控制器返回ModelAndView，ModelAndView是SpringMvc框架的一个底层对象，包括Model和View
- 前端控制器请求视图解析器去进行视图解析，根据逻辑视图名解析成真正的视图(JSP)
- 视图解析器向前端控制器返回View
- 前端控制器进行视图渲染，视图渲染将模型数据(在ModelAndView对象中)填充到request域
- 前端控制器向用户响应结果



优点：

MVC是一个非常好的协作模式， 能够有效降低代码的耦合度从架构上能够让开发者明白代码应该写在哪里。为了让View更纯粹， 还可以使用Thymeleaf、Freemarker等模板引擎， 使模板里无法写入Java代码， 让前后端分工更加清晰。



缺点：

- 前端开发重度依赖开发环境，开发效率低，这种架构下，前后端协作有两种模式：
  - 第一种是前端写DEMO， 写好后， 让后端去套模板。好处是DEMO可以本地开发， 很高效。不足是还需要后端套模板，有可能套错，套完后还需要前端确定，来回沟通调整的成本比较大；
  - 另一种协作模式是前端负责浏览器端的所有开发和服务器端的View层模板开发。好处是UI相关的代码都是前端去写就好，后端不用太关注，不足就是前端开发重度绑定后端环境，环境成为影响前端开发效率的重要因素。
- 前后端职责纠缠不清：模板引擎功能强大，依旧可以通过拿到的上下文变量来实现各种业务逻辑。这样，只要前端弱势一点，往往就会被后端要求在模板层写出不少业务代码，还有一个很大的灰色地带是Controller， 页面路由等功能本应该是前端最关注的， 但却是由后端来实现。Controller本身与Model往往也会纠缠不清，看了让人咬牙的业务代码经常会出现在Controller层。这些问题不能全归结于程序员的素养， 否则JSP就够了。
- 对前端发挥的局限性：性能优化如果只在前端做空间非常有限，于是我们经常需要后端合作，但由于后端框架限制，我们很难使用[Comet】、【BigPipe】等技术方案来优化性能。



注：在这期间(2005年以前) ， 包括早期的JSP、PHP可以称之为Web 1.0时代。在这里想说一句， 如果你是一名Java初学者， 请你不要再把一些陈旧的技术当回事了， 比如JSP， 因为时代在变、技术在变、什么都在变(引用扎克伯格的一句话：唯一不变的是变化本身)；当我们去给大学做实训时，有些同学会认为我们没有讲什么干货，其实不然，只能说是你认知里的干货对于市场来说早就过时了而已

## 3.2、基本ajax带来的spa时代

时间回到2005年AJAX(Asynchronous JavaScript And XML， 异步JavaScript和XML，老技术新用法)被正式提出并开始使用CDN作为静态资源存储， 于是出现了JavaScript王者归来(在这之前JS都是用来在网页上贴狗皮膏药广告的) 的SPA(Single Page Application) 单页面应用时代。

![image-20220626002434830](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220626002434830.png)

优点：

这种模式下， 前后端的分工非常清晰， 前后端的关键协作点是AJAX接口。看起来是如此美妙， 但回过头来看看的话， 这与JSP时代区别不大。复杂度从服务端的JSP里移到了浏览器的JavaScript，浏览器端变得很复杂。类似Spring MVC， 这个时代开始出现浏览器端的分层架构：

![image-20220626002550881](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220626002550881.png)

缺点：

前后端接口的约定：如果后端的接口一塌糊涂，如果后端的业务模型不够稳定，那么前端开发会很痛苦；不少团队也有类似尝试，通过接口规则、接口平台等方式来做。有了和后端一起沉淀的接口规则，还可以用来模拟数据，使得前后端可以在约定接口后实现高效并行开发。

前端开发的复杂度控制：SPA应用大多以功能交互型为主，JavaScript代码过十万行很正常。大量JS代码的组织，与View层的绑定等，都不是容易的事情。

## 3.3、前端为主的mv*时代

此处的MV*模式如下：

- MVC(同步通信为主) ：Model、View、Controller
- MVP(异步通信为主) ：Model、View、Presenter
- MVVM(异步通信为主)：Model、View、ViewModel为了降低前端开发复杂度，涌现了大量的前端框架，比如：Angular JS、React、Vue.js、Ember JS等， 这些框架总的原则是先按类型分层， 比如Templates、Controllers、Models， 然后再在层内做切分，如下图：

![image-20220626002703787](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220626002703787.png)

优点

- 前后端职责很清晰：前端工作在浏览器端，后端工作在服务端。清晰的分工，可以让开发并行，测试数据的模拟不难， 前端可以本地开发。后端则可以专注于业务逻辑的处理， 输出RESTful等接口。
- 前端开发的复杂度可控：前端代码很重，但合理的分层，让前端代码能各司其职。这一块蛮有意思的，简单如模板特性的选择，就有很多很多讲究。并非越强大越好，限制什么，留下哪些自由，代码应该如何组织，所有这一切设计，得花一本书的厚度去说明。
- 部署相对独立：可以快速改进产品体验

缺点

- 代码不能复用。比如后端依旧需要对数据做各种校验，校验逻辑无法复用浏览器端的代码。如果可以复用，那么后端的数据校验可以相对简单化。
- 全异步， 对SEO不利。往往还需要服务端做同步渲染的降级方案。
- 性能并非最佳，特别是移动互联网环境下。
- SPA不能满足所有需求， 依旧存在大量多页面应用。URL Design需要后端配合， 前端无法完全掌控。

## 3.4、nodejs带来的全栈时代

前端为主的MV*模式解决了很多很多问题， 但如上所述， 依旧存在不少不足之处。随着Node JS的兴起， JavaScript开始有能力运行在服务端。这意味着可以有一种新的研发模式：

![image-20220626004245728](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220626004245728.png)

在这种研发模式下，前后端的职责很清晰。对前端来说，两个UI层各司其职：

- Front-end Ul layer处理浏览器层的展现逻辑。通过CSS渲染样式， 通过JavaScript添加交互功能， HTML的生成也可以放在这层， 具体看应用场景。
- Back-end Ul layer处理路由、模板、数据获取、Cookie等。通过路由， 前端终于可以自主把控URL Design， 这样无论是单页面应用还是多页面应用， 前端都可以自由调控。后端也终于可以摆脱对展现的强关注，转而可以专心于业务逻辑层的开发。

通过Node， WebServer层也是JavaScript代码， 这意味着部分代码可前后复用， 需要SEO的场景可以在服务端同步渲染，由于异步请求太多导致的性能问题也可以通过服务端来缓解。前一种模式的不足，通过这种模式几乎都能完美解决掉。

与JSP模式相比， 全栈模式看起来是一种回归， 也的确是一种向原始开发模式的回归， 不过是一种螺旋上升式的回归。

基于NodeJS的全栈模式， 依旧面临很多挑战：

- 需要前端对服务端编程有更进一步的认识。比如TCP/IP等网络知识的掌握。
- NodeJS层与Java层的高效通信。NodeJS模式下， 都在服务器端， RESTful HTTP通信未必高效， 通过SOAP等方式通信更高效。一切需要在验证中前行。
- 对部署、运维层面的熟练了解，需要更多知识点和实操经验。
- 大量历史遗留问题如何过渡。这可能是最大最大的阻力。



# 4、第一个vue程序

## 4.1、什么是MVVM

MVVM（Model-View-ViewModel）是一种软件架构设计模式。是一种简化用户界面的**事件驱动编程方式**。

MVVM源自经典的MVC（Model-View-Controller）模式，MVVM的核心是ViewModel层，负责转换Model中数据对象让数据变得更容易管理和使用，其作用如下：

- 该层向上与视图层进行双向数据绑定
- 向下与model层通过接口请求进行数据交互

![image-20220626103446738](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220626103446738.png)

MVVM已经相当成熟了。主要运用单不仅仅在网络应用程序开发中。当下流行的MVVM框架有VUE.js和AngularJS等



## 4.2、为什么要使用MVVM

MVVM模式和MVC模式一样，主要目的是分离视图（view）和模型（model），有几个好处：

- 低耦合

  视图（view）可以独立于model变化和修改，一个ViewModel可以绑定到不同的view上，当view变化的时候model可以不变，当model变化的时候view也可以不变

- 可复用

  你可以把一些视图逻辑放在一个ViewModel里面，让很多view重用这段视图逻辑

- 独立开发

  开发成员可以专注于业务逻辑和数据的开发（ViewModel），设计人员可以专注于页面设计

- 可测试

  界面素来是比较难于测试的，而现在测试可以针对ViewModel来写

![image-20220626110954580](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220626110954580.png)



## 4.3、MVVM模式的实现者 -- vue

- Model：模型层，在这里表示JavaScript对象

- View：视图层，在这里表示DOM（html操作的元素）

- ViewModel：连接视图和数据的中间件，VUE.js就是MVVM中的ViewModel层的实现者。

  在MVVM架构中，是不允许数据和视图直接通信的，只能通过ViewModel来通讯，而ViewModel就是定义了一个Observer观察者

- **ViewModel能够观察到数据的变化，并对视图对应的内容进行更新**

- **ViewModel能够监听到视图的变化，并能够通知数据发生改变**



至此，我们就明白了，vue就是一个MVVM的实现者，它的核心就是实现了DOM监听与数据绑定。



## 4.4、第一个vue程序

1. idea安装vue插件

   ![image-20220626110016192](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220626110016192.png)

2. 新建一个空白项目Vue

3. 新增一个文件demo01.html

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   
   <!-- view层 模板 -->
   <div id="app">
       <h2>{{message}}</h2>
   </div>
   
   <!-- 1. 导入vue.js -->
   <script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
   <script>
       var vm = new Vue({
           el:"#app",
           // Model 数据
           data:{
               message:"hello,vue"
           }
       });
   </script>
   </body>
   </html>
   ```

4. 用浏览器打开

   ![image-20220626110200134](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220626110200134.png)

5. 浏览器控制台修改

   ```
   vm.message='zyy'
   ```

   发现页面也修改了

   ![image-20220626110333259](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220626110333259.png)

Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统



![img](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/1363376-20210416213825502-1608429244.png)

八大生命周期钩子函数

| 函数          | 调用时间              |
| ------------- | --------------------- |
| beforeCreate  | vue实例初始化之前调用 |
| created       | vue实例初始化之后调用 |
| beforeMount   | 挂载到dom树之前调用   |
| mounted       | 挂载到dom树之后调用   |
| beforeUpdate  | 数据更新之前调用      |
| updated       | 数据更新之后调用      |
| beforeDestroy | vue实例销毁之前调用   |
| destroyed     | vue实例销毁之后调用   |



# 5、vue基本基本语法

## 5.1、v-bind

除了文本插值，我们还可以像这样来绑定元素 attribute

```html
<div id="app-2">
  <span v-bind:title="message">鼠标悬停几秒钟查看此处动态绑定的提示信息！</span>
  <!-- 可以简写成 -->
  <!--<span :title="message">鼠标悬停几秒钟查看此处动态绑定的提示信息！</span>-->
</div>
```



```javascript
    var vm2 = new Vue({
        el: "#app-2",
        // Model 数据
        data: {
            message: '页面加载于 ' + new Date().toLocaleString()
        }
    });
```



这里我们遇到了一点新东西。你看到的 `v-bind` attribute 被称为**指令**。指令带有前缀 `v-`，以表示它们是 Vue 提供的特殊 attribute。可能你已经猜到了，它们会在渲染的 DOM 上应用特殊的响应式行为。在这里，该指令的意思是：“将这个元素节点的 `title` attribute 和 Vue 实例的 `message` property 保持一致”。

如果你再次打开浏览器的 JavaScript 控制台，输入 `vm2.message = '新消息'`，就会再一次看到这个绑定了 `title` attribute 的 HTML 已经进行了更新。

## 5.2、v-if...v-else-if...v-else

```html
<!DOCTYPE html>
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!-- view层 模板 -->
<div id="app">
    <div v-if="type === 'A'">
        A
    </div>
    <div v-else-if="type === 'B'">
        B
    </div>
    <div v-else-if="type === 'C'">
        C
    </div>
    <div v-else>
        Not A/B/C
    </div>
</div>


<!-- 1. 导入vue.js -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
<script>
    var vm = new Vue({
        el: "#app",
        // Model 数据
        data: {
            type:"A"
        }
    });
</script>

</body>
</html>
```



## 5.3、v-for

```html
<!DOCTYPE html>
<head xmlns:v-for="http://www.w3.org/1999/xhtml">
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!-- view层 模板 -->
<div id="app">
    <li v-for="(item,index) in items">
        {{item.message}}--{{index}}
    </li>
    <!-- 
	<li v-for="item in items">
        {{item.message}}
    </li> 
    -->

</div>


<!-- 1. 导入vue.js -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
<script>
    var vm = new Vue({
        el: "#app",
        // Model 数据
        data: {
            items:[
                {message:"123"},
                {message:"234"},
                {message:"345"}
            ]
        }
    });
</script>

</body>
</html>
```



# 6、vue绑定事件

## 6.1、v-on

```html
<!DOCTYPE html>
<head xmlns:v-on="http://www.w3.org/1999/xhtml">
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!-- view层 模板 -->
<div id="app">
    <button v-on:click="sayHello">点击</button>
    <!-- 可以简写成 -->
    <!--<button @click="sayHello">点击</button>-->
</div>


<!-- 1. 导入vue.js -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
<script>
    var vm = new Vue({
        el: "#app",
        // Model 数据
        data: {
        },
        methods: {//方法必须定义在vue methods对象中
            sayHello: function () {
                alert("hello");
            }
        }
    });
</script>

</body>
</html>
```

# 7、vue双向绑定

数据双向绑定， 即当数据发生变化的时候， 视图也就发生变化， 当视图发生变化的时候，数据也会跟着同步变化。

## 7.1、v-model

```html
<!DOCTYPE html>
<head xmlns:v-model="http://www.w3.org/1999/xhtml">
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!-- view层 模板 -->
<div id="app">
    <input type="text" v-model="message" placeholder="edit me"><span>{{message}}</span>
</div>


<!-- 1. 导入vue.js -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
<script>
    var vm = new Vue({
        el: "#app",
        // Model 数据
        data: {
            "message":""
        }
    });
</script>

</body>
</html>
```

![image-20220630222145213](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220630222145213.png)

```html
<!DOCTYPE html>
<head xmlns:v-model="http://www.w3.org/1999/xhtml">
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!-- view层 模板 -->
<div id="app">
    性别：
    <input type="radio" value="1" name="sex" v-model="zyy">男
    <input type="radio" value="2" name="sex" v-model="zyy">女
    <p>选中了{{zyy}}</p>
</div>


<!-- 1. 导入vue.js -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
<script>
    var vm = new Vue({
        el: "#app",
        // Model 数据
        data: {
            "zyy":""
        },
        methods: {
        }
    });
</script>

</body>
</html>
```

![image-20220627163345658](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220627163345658.png)

 ```html
 <!DOCTYPE html>
 <head xmlns:v-model="http://www.w3.org/1999/xhtml">
     <meta charset="UTF-8">
     <title>Title</title>
 </head>
 <body>
 
 <!-- view层 模板 -->
 <div id="app">
     <select v-model="zyy">
         <option disabled value="">==请选择===</option>
         <option>A</option>
         <option>B</option>
         <option>C</option>
     </select>
     <p>选择了{{zyy}}</p>
 </div>
 
 
 <!-- 1. 导入vue.js -->
 <script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
 <script>
     var vm = new Vue({
         el: "#app",
         // Model 数据
         data: {
             "zyy":""
         },
         methods: {
         }
     });
 </script>
 
 </body>
 </html>
 ```

![image-20220627163315300](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220627163315300.png)

# 8、vue组件讲解

组件是可复用的`Vue`实例， 说白了就是一组可以重复使用的模板

![img](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/20210623154955230.png)

## 8.1、Vue.component

```html
<!DOCTYPE html>
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!-- view层 模板 -->
<div id="app">
    <zyy></zyy>

</div>


<!-- 1. 导入vue.js -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
<script>
    Vue.component("zyy", {
        template: '<li>苹果</li>'
    });
    var vm = new Vue({
        el: "#app"
    });
</script>

</body>
</html>
```

![image-20220627163137229](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220627163137229.png)

```html
<!DOCTYPE html>
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!-- view层 模板 -->
<div id="app">
    <zyy v-for="item in items" v-bind:pro="item"></zyy>

</div>


<!-- 1. 导入vue.js -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
<script>
    Vue.component("zyy", {
        props: ['pro'],
        template: '<li>{{pro}}</li>'
    });
    var vm = new Vue({
        el: "#app",
        data: {
            items: ["java", "linux", "vue"]
        }

    });
</script>

</body>
</html>
```

![image-20220627163221179](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220627163221179.png)

# 9、Axios异步通讯

新建一个文件data.json

```json
{
  "name": "狂神说Java",
  "url": "https://www.baidu.com",
  "page": 1,
  "isNonProfit": true,
  "address": {
    "street": "含光门",
    "city": "陕西西安",
    "country": "中国"
  },
  "links": [
    {
      "name": "bilibili",
      "url": "https://space.bilibili.com/95256449"
    },
    {
      "name": "狂神说Java",
      "url": "https://blog.kuangstudy.com"
    },
    {
      "name": "百度",
      "url": "https://www.baidu.com/"
    }
  ]
}

```

新建一个html

```html
<!DOCTYPE html>
<head xmlns:v-bind="http://www.w3.org/1999/xhtml">
    <meta charset="UTF-8">
    <title>Title</title>
    <!-- 解决闪烁问题 -->
    <style>
        /*属性选择器*/
        [v-clock] {
            display: none;
        }
    </style>
</head>
<body>

<!-- view层 模板 -->
<div id="app" v-clock>
    <p>{{info.name}}</p>
    <p>{{info.address.street}}</p>
    <a v-bind:href="info.url">点我</a>
</div>


<!-- 1. 导入vue.js -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script>
    var vm = new Vue({
        el: "#app",
        data: {},
        data() {
            return {
                info: {}
            }
        },
        mounted() {//钩子函数
            //链式编程
            axios.get('data.json').then(response => (this.info = response.data))
        }
    });
</script>

</body>
</html>
```

![image-20220627222826896](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220627222826896.png)

# 10、计算属性

**什么是计算属性？**

计算属性的重点突出在属性两个字上(属性是名词)，首先它是个属性其次这个属性有计算的能力(计算是动词)，这里的计算就是个函数：简单点说，它就是一个能够将计算结果缓存起来的属性(将行为转化成了静态的属性)，仅此而已；可以想象为缓存!

## 10.1、computed

```html
<!DOCTYPE html>
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!-- view层 模板 -->
<div id="app">
    <p>currentTime1 {{currentTime1()}}</p>
    <p>currentTime2 {{currentTime2}}</p>
</div>


<!-- 1. 导入vue.js -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
<script>
    var vm = new Vue({
        el: "#app",
        data: {
            message:"hello zyy"
        },
        methods: {
            currentTime1: function() {
                return Date.now();
            }
        },
        // computed 计算属性
        // methods、computed 中函数名一般不可重名，重名的话也只会走methods中的函数
        computed: {
            currentTime2: function() {
                this.message;
                return Date.now();
            }

        }
    });
</script>

</body>
</html>
```

![image-20220628170059951](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220628170059951.png)

打开浏览器控制台，验证如下

![image-20220628170023351](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220628170023351.png)

# 11、插槽solt

## 11.1、slot

```html
<!DOCTYPE html>
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!-- view层 模板 -->
<div id="app">
    <!-- 老代码 -->
    <!--<p>课程</p>
    <ul>
        <li>java</li>
        <li>linux</li>
    </ul>-->
    <todo>
        <!-- <todo-head slot="todo-head" v-bind:title="ti"></todo-head> -->
        <!-- v-bind可以简写为 -->
        <todo-head slot="todo-head" :title="ti"></todo-head>
        <todo-item slot="todo-item" v-for="it in its" :item="it"></todo-item>
    </todo>
</div>


<!-- 1. 导入vue.js -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
<script>
    Vue.component("todo", {
        template: '<div>\
                       <slot name="todo-head"></slot>\
                       <ul>\
                           <slot name="todo-item"></slot>\
                       </ul>\
                   </div>'
    });

    Vue.component("todo-head", {
        props: ['title'],
        template: '<p>{{title}}</p>'
    });


    Vue.component("todo-item", {
        props: ['item'],
        template: '<li>{{item}}</li>'
    });
    var vm = new Vue({
        el: "#app",
        data: {
            ti:'必修课程',
            its:['java','linux','spring']
        },
    });
</script>

</body>
</html>
```

![image-20220628180921023](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220628180921023.png)



# 12、自定义事件内容分发

## 12.1、this.$emit

![image-20220630223827933](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220630223827933.png)

```html
<!DOCTYPE html>
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!-- view层 模板 -->
<div id="app">
    <todo>
        <!-- <todo-head slot="todo-head" v-bind:title="ti"></todo-head> -->
        <!-- v-bind可以简写为 -->
        <todo-head slot="todo-head" :title="ti"></todo-head>
        <todo-item slot="todo-item" v-for="(it,index) in its" :item="it" :index="index"
                   v-on:rev="removeItem(index)"></todo-item>
    </todo>
</div>


<!-- 1. 导入vue.js -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
<script>
    Vue.component("todo", {
        template: '<div>\
                       <slot name="todo-head"></slot>\
                       <ul>\
                           <slot name="todo-item"></slot>\
                       </ul>\
                   </div>'
    });

    Vue.component("todo-head", {
        props: ['title'],
        template: '<p>{{title}}</p>'
    });


    Vue.component("todo-item", {
        props: ['item', 'index'],
        // template: '<li>{{item}} <button v-on:click="remove()">删除</button></li>',
        // v-on:click 可以简写 @click
        template: '<li>{{item}} <button @click="remove">删除</button></li>',
        methods: {
            remove: function (index) {
                //this.$emit 自定义事件分发
                this.$emit('rev', index);
            }
        }
    });
    var vm = new Vue({
        el: "#app",
        data: {
            ti: '必修课程',
            its: ['java', 'linux', 'spring']
        },
        methods: {
            removeItem: function (ind) {
                console.info("删除了" + this.its[ind] + "元素！");
                //删除当前元素
                this.its.splice(ind, 1);
            }
        }
    });
</script>

</body>
</html>
```

![image-20220630214454651](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220630214454651.png)

# 13、第一个vue-cli程序

## 13.1、什么是vue-cli

vue-cli 官方提供的一个脚手架,用于快速生成一个 vue 的项目模板;

预先定义好的目录结构及基础代码，就好比咱们在创建 Maven 项目时可以选择创建一个骨架项目，这个骨架项目就是脚手架,我们的开发更加的快速;

主要功能:

- 统一的目录结构
- 本地调试
- 热部署
- 单元测试
- 集成打包上线

## 13.2、需要的环境

- 安装nodejs
- 安装git



**确认nodejs安装成功**

- cmd 下输入 `node -v`,查看是否能够正确打印出版本号即可!
- cmd 下输入 `npm -v`,查看是否能够正确打印出版本号即可!

![image-20220704224321833](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220704224321833.png)

**npm**

```
npm 是JavaScript 世界的包管理工具，并且是Node.js 平台的默认包管理工具。通过npm 可以安装、共享、分发代码，管理项目依赖关系。
```



**安装 Node.js 淘宝镜像加速器（cnpm）**

这样子的话,下载会快很多~

```bash
# -g 就是全局安装
npm install cnpm -g

# 若安装失败，则将源npm源换成淘宝镜像
# 因为npm安装插件是从国外服务器下载，受网络影响大
npm install --registry=https://registry.npm.taobao.org
# 然后再执行
npm install cnpm -g
```

安装位置

```
C:\Users\{管理员}\AppData\Roaming\npm
```

## 13.3、安装vue-cli

```bash
#在命令台输入
cnpm instal1 vue-cli -g
#测试是否安装成功#查看可以基于哪些模板创建vue应用程序，通常我们选择webpack
vue list
```

![image-20220704173741637](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220704173741637.png)



## 13.4、第一个vue-cli应用程序

1. 新建一个空白文件夹VUE

2. 打开cmd命令窗口，然后进去刚才新建的目录

3. 执行命令

   ```bash
   vue init webpack myvue
   ```

   ```
   1.Project name：项目名称 ，默认回车即可
   2.Project description：项目描述，默认回车即可
   3.Author：作者，如果有配置git的作者，自动会读取。默认回车即可
   4.vue build (Use arrow keys) 有下面两个选择，推荐选择第一个
   > Runtime + Compiler:recommended for most users
   > Runtime-only:about 6KB lighter min+gzip,but templates (or any Vue-specific HTML) are ONLY 
   5.Install vue-router? 是否安装vue的路由插件，选择n不安装（后期需要在手动添加）
   6.Use ESLint to lint your code? 是否用ESLint来限制你的代码错误和风格。选择n不安装（后期需要在手动添加）
   7.setup unit tests? 是否需要安装单元测试工具，选择n不安装（后期需要在手动添加）
   8.Setup e2e tests with Nightwatch? 是否安装e2e来进行用户行为模拟测试，选择n不安装（后期需要在手动添加）
   9.Should we run 'npm install' for you after the project has been created? 有下面三个选择，这里选择最后一个
   > yes,use npm 使用npm
   > yes,use yarn  使用yarn
   > no,I will handle that myself 自己操作
   ```

4. 进入刚才创建的项目myvue，安装并运行

   ```bash
   cd myvue
   npm install
   npm run dev
   ```

   ![image-20220704223527847](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220704223527847.png)

   ![image-20220704223538250](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220704223538250.png)

# 14、webpack学习使用

什么是webpack? -- **静态模板打包工具**

```
本质上，webpack 是一个用于现代 JavaScript 应用程序的 静态模块打包工具。当 webpack 处理应用程序时，它会在内部从一个或多个入口点构建一个 依赖图(dependency graph)，然后将你项目中所需的每一个模块组合成一个或多个 bundles，它们均为静态资源，用于展示你的内容。
```

安装webpack

```bash
npm install webpack -g
npm install webpack-cli -g
```

测试安装成功

```bash
webpack -v
webpack-cli -v
```

![image-20220705230909471](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220705230909471.png)



**案例**

1. 新建一个项目（PS:新增一个文件夹）,webpack-study，然后用idea打开

2. 新增一个目录modules

3. 在modules下新建hello.js

   ```js
   exports.sayHi = function () {
       document.write("<h1>zyy12</h1>")
   }
   ```

4. 在modules下新建main.js

   ```js
   var hello = require('./hello');
   hello.sayHi();
   ```

5. 最外层目录新建webpack.config.js

   ```js
   module.exports = {
       entry: './modules/main.js',
       output: {
           filename: './js/bundle.js'
       }
   }
   ```

6. 命令窗口中输入`webpack`

   ![image-20220705233107035](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220705233107035.png)

   生成文件

   ![image-20220705233143557](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220705233143557.png)

7. 最外层目录新建index.html

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   <script src="dist/js/bundle.js"></script>
   
   </body>
   </html>
   ```

8. 用浏览器打开

   ![image-20220705233228096](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220705233228096.png)

热部署效果(修改了代码，无需执行webpack命令，直接页面刷新即可)

```bash
webpack --watch
```





# 15、vue-router路由

官方文档：https://router.vuejs.org/zh/guide/#html

安装

```bash
npm install vue-router --save-dev
# 上面报错的话，可以尝试降低版本
npm install vue-router@3.5.2 --save-dev
```

![image-20220706222559380](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220706222559380.png)

验证

1. 打开之前的第一个vue-cli项目 myvue

2. 清理src/assets/的所有文件，和src/components下的所有文件

3. 新增一个内容页components/Content.vue

   ```vue
   <template>
   
     <h1>内容页</h1>
   </template>
   
   <script>
     export default {
       name: "Content"
     }
   </script>
   
   <style scoped>
   
   </style>
   
   ```

4. 新增一个首页components/Main.vue

   ```vue
   <template>
     <h1>首页</h1>
   </template>
   
   <script>
     export default {
       name: "Main"
     }
   </script>
   
   <style scoped>
   
   </style>
   ```

5. 新增一个目录src/router，并在这个目录下新增一个配置路由文件index.js

   ```js
   import Vue from 'vue'
   import VueRouter from 'vue-router'
   import Content from '../components/Content'
   import Main from '../components/Main'
   
   //安装路由
   Vue.use(VueRouter);
   
   //配置导出路由
   export default new VueRouter({
     routes: [
       {
         //路由路径 有点类似后端的@RequestMapping
         path: '/content',
         name: 'content',
         // 跳转的组件
         component: Content
       },
       {
         //路由路径
         path: '/main',
         name: 'main',
         // 跳转的组件
         component: Main
       }
     ]
   })
   
   ```

6. main.js中添加路由

   ```js
   import Vue from 'vue'
   import App from './App'
   import router from './router' //自助扫描里面的路由配置
   
   Vue.config.productionTip = false
   
   //显示声明使用VueRouter
   // Vue.use(VueRouter);
   
   new Vue({
     el: '#app',
     //配置路由
     router,
     components: {App},
     template: '<App/>'
   })
   
   ```

7. App.vue中使用路由

   ```vue
   <template>
     <div id="app">
       <h1>Vue-Router</h1>
       <router-link to="/main">首页</router-link>
       <router-link to="/content">内容</router-link>
       <router-view></router-view>
     </div>
   </template>
   
   <script>
     export default {
       name: 'App'
     }
   </script>
   
   <style>
     #app {
       font-family: 'Avenir', Helvetica, Arial, sans-serif;
       -webkit-font-smoothing: antialiased;
       -moz-osx-font-smoothing: grayscale;
       text-align: center;
       color: #2c3e50;
       margin-top: 60px;
     }
   </style>
   
   ```

8. 命令行运行`npm run dev`

9. 打开浏览器验证

   ![image-20220706223325230](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220706223325230.png)

   ![image-20220706223335885](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220706223335885.png)



# 16、vue+elementUI

1. 创建一个名为hello-vue的工程

   ```bash
   vue init webpack hello-vue
   ```

   ![image-20220708165225979](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220708165225979.png)

2. 进入工程目录，并安装依赖

   ```bash
   # 进入工程目录
   cd hello-vue
   # 安装vue-routern 报错的都可以尝试用cnpm去安装
   npm install vue-router --save-dev
   # 上面报错的话，可以尝试降低版本
   npm install vue-router@3.5.2 --save-dev
   # 安装element-ui
   npm i element-ui -S
   # 安装依赖
   npm install
   # 安装SASS加载器 可能会报错，可以尝试降低版本
   cnpm install sass-loader node-sass --save-dev
   # 启功测试
   npm run dev
   ```

   npm 命令说明

   ```
   npm install moduleName：安装模块到项目目录下
   
   npm install -g moduleName：-g的意思是将模块安装到全局，具体安装到磁盘哪个位置要看npm config prefix的位置
   
   npm install -save moduleName：–save的意思是将模块安装到项目目录下， 并在package文件的dependencies节点写入依赖，-S为该命令的缩写
   
   npm install -save-dev moduleName:–save-dev的意思是将模块安装到项目目录下，并在package文件的devDependencies节点写入依赖，-D为该命令的缩写
   ```

3. idea 打开创建好的项目，并清理无用的文件，并创建好项目结构，如下图

   ![image-20220708163049441](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220708163049441.png)

   

4. 在views下新增Main.vue

   ```vue
   <template>
     <h1>首页</h1>
   </template>
   
   <script>
     export default {
       name: "Main"
     }
   </script>
   
   <style scoped>
   
   </style>
   ```

5. 在views下新增Login.vue

   ```vue
   <template>
     <div>
       <el-form ref="loginForm" :model="form" :rules="rules" label-width="80px" class="login-box">
         <h3 class="login-title">欢迎登录</h3>
         <el-form-item label="账号" prop="username">
           <el-input type="text" placeholder="请输入账号" v-model="form.username"/>
         </el-form-item>
         <el-form-item label="密码" prop="password">
           <el-input type="password" placeholder="请输入密码" v-model="form.password"/>
         </el-form-item>
         <el-form-item>
           <el-button type="primary" v-on:click="onsubmit('loginForm')">登录</el-button>
         </el-form-item>
       </el-form>
   
       <el-dialog title="温馨提示" :visible.sync="dialogVisible" width="30%" :before-close="handleClose">
         <span>请输入账号和密码</span>
         <span slot="footer" class="dialog-footer">
             <el-button type="primary" @click="dialogVisible = false">确定</el-button>
           </span>
       </el-dialog>
     </div>
   </template>
   
   <script>
     export default {
       name: "Login",
       data() {
         return {
           form: {
             username: '',
             password: ''
           },
           //表单验证，需要在 el-form-item 元素中增加prop属性
           rules: {
             username: [
               {required: true, message: "账号不可为空", trigger: "blur"}
             ],
             password: [
               {required: true, message: "密码不可为空", trigger: "blur"}
             ]
           },
   
           //对话框显示和隐藏
           dialogVisible: false
         }
       },
       methods: {
         handleClose: function () {
           this.dialogVisible = false;
         },
         onsubmit(formName) {
           //为表单绑定验证功能
           this.$refs[formName].validate((valid) => {
             if (valid) {
               //使用vue-router路由到指定界面，该方式称为编程式导航
               this.$router.push('/main');
             } else {
               this.dialogVisible = true;
               return false;
             }
           });
         }
       }
     }
   </script>
   
   <style scoped>
     .login-box {
       border: 1px solid #DCDFE6;
       width: 350px;
       margin: 180px auto;
       padding: 35px 35px 15px 35px;
       border-radius: 5px;
       -webkit-border-radius: 5px;
       -moz-border-radius: 5px;
       box-shadow: 0 0 25px #909399;
     }
   
     .login-title {
       text-align: center;
       margin: 0 auto 40px auto;
       color: #303133;
     }
   </style>
   ```

6. 在router下新增index.js

   ```js
   import Vue from "vue";
   import router from 'vue-router';
   import Main from "../views/Main";
   import Login from "../views/Login";
   
   Vue.use(router);
   
   export default new router({
     routes: [
       {
         path: '/login',
         component: Login
       },
       {
         path: '/main',
         component: Main
       }
     ]
   })
   ```

7. App.vue修改

   ```vue
   <template>
     <div id="app">
       <router-link to="/login">登录</router-link>
       <router-view></router-view>
     </div>
   </template>
   
   <script>
     export default {
       name: 'App',
     }
   </script>
   
   <style>
     #app {
       font-family: 'Avenir', Helvetica, Arial, sans-serif;
       -webkit-font-smoothing: antialiased;
       -moz-osx-font-smoothing: grayscale;
       text-align: center;
       color: #2c3e50;
       margin-top: 60px;
     }
   </style>
   ```

8. main.js修改

   ```js
   import Vue from 'vue'
   import App from './App'
   //导入router
   import router from "./router";
   //导入ElementUI
   import ElementUI from 'element-ui';
   import 'element-ui/lib/theme-chalk/index.css';
   
   Vue.use(router);
   Vue.use(ElementUI);
   
   new Vue({
     el: '#app',
     router,
     render: h => h(App)
   })
   ```

9. 测试`npm run dev`

   ![image-20220708164235634](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220708164235634.png)

   ![image-20220708164248995](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220708164248995.png)

   ![image-20220708164301808](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220708164301808.png)

   

项目注意事项：

1. 版本问题

   ![image-20220708164354438](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220708164354438.png)

   package.json

   ```json
   {
     "name": "hello-vue",
     "version": "1.0.0",
     "description": "A Vue.js project",
     "author": "zyy",
     "private": true,
     "scripts": {
       "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
       "start": "npm run dev",
       "build": "node build/build.js"
     },
     "dependencies": {
       "element-ui": "^2.15.9",
       "vue": "^2.5.2"
     },
     "devDependencies": {
       "autoprefixer": "^7.1.2",
       "babel-core": "^6.22.1",
       "babel-helper-vue-jsx-merge-props": "^2.0.3",
       "babel-loader": "^7.1.1",
       "babel-plugin-syntax-jsx": "^6.18.0",
       "babel-plugin-transform-runtime": "^6.22.0",
       "babel-plugin-transform-vue-jsx": "^3.5.0",
       "babel-preset-env": "^1.3.2",
       "babel-preset-stage-2": "^6.22.0",
       "chalk": "^2.0.1",
       "copy-webpack-plugin": "^4.0.1",
       "css-loader": "^0.28.0",
       "extract-text-webpack-plugin": "^3.0.0",
       "file-loader": "^1.1.4",
       "friendly-errors-webpack-plugin": "^1.6.1",
       "html-webpack-plugin": "^2.30.1",
       "node-notifier": "^5.1.2",
       "node-sass": "^7.0.1",
       "optimize-css-assets-webpack-plugin": "^3.2.0",
       "ora": "^1.2.0",
       "portfinder": "^1.0.13",
       "postcss-import": "^11.0.0",
       "postcss-loader": "^2.0.8",
       "postcss-url": "^7.2.1",
       "rimraf": "^2.6.0",
       "sass-loader": "^13.0.2",
       "semver": "^5.3.0",
       "shelljs": "^0.7.6",
       "uglifyjs-webpack-plugin": "^1.1.1",
       "url-loader": "^0.5.8",
       "vue-loader": "^13.3.0",
       "vue-router": "^3.5.2",
       "vue-style-loader": "^3.0.1",
       "vue-template-compiler": "^2.5.2",
       "webpack": "^3.6.0",
       "webpack-bundle-analyzer": "^2.9.0",
       "webpack-dev-server": "^2.9.1",
       "webpack-merge": "^4.1.0"
     },
     "engines": {
       "node": ">= 6.0.0",
       "npm": ">= 3.0.0"
     },
     "browserslist": [
       "> 1%",
       "last 2 versions",
       "not ie <= 8"
     ]
   }
   ```

2. Login.vue会报错的原因可能是最下面的`<style>`标签加上了`lang="scss"`，删除`lang="scss"`后再试试

   

# 17、嵌套路由

就在上面的项目中继续优化

1. views目录下新建文件夹user，并在下面创建List.vue 和Profile.vue

   List.vue

   ```vue
   <template>
     <h1>用户列表</h1>
   
   </template>
   
   <script>
     export default {
       name: "UserList"
     }
   </script>
   
   <style scoped>
   
   </style>
   ```

   Profile.vue

   ```vue
   <template>
     <h1>个人信息</h1>
   
   </template>
   
   <script>
     export default {
       name: "UserProfile"
     }
   </script>
   
   <style scoped>
   
   </style>
   ```

2. index.js 添加嵌套路由

   ```js
   import Vue from "vue";
   import VueRouter from 'vue-router';
   import Main from "../views/Main";
   import Login from "../views/Login";
   import UserList from "../views/user/List";
   import UserProfile from "../views/user/Profile";
   
   Vue.use(VueRouter);
   
   export default new VueRouter({
     routes: [
       {
         path: '/login',
         component: Login
       },
       {
         path: '/main',
         component: Main,
         // 嵌套路由
         children: [
           {path: '/user/list', component: UserList},
           {path: '/user/profile', component: UserProfile}
         ]
       }
     ]
   })
   ```

3. Main.vue优化

   ```vue
   <template>
     <div>
       <el-container>
         <el-aside width="200px">
           <el-menu :default-openeds="['1']">
             <el-submenu index="1">
               <template slot="title"><i class="el-icon-caret-right"></i>用户管理</template>
               <el-menu-item-group>
                 <el-menu-item index="1-1">
                   <!--插入的地方-->
                   <router-link to="/user/profile">个人信息</router-link>
                 </el-menu-item>
                 <el-menu-item index="1-2">
                   <!--插入的地方-->
                   <router-link to="/user/list">用户列表</router-link>
                 </el-menu-item>
               </el-menu-item-group>
             </el-submenu>
             <el-submenu index="2">
               <template slot="title"><i class="el-icon-caret-right"></i>内容管理</template>
               <el-menu-item-group>
                 <el-menu-item index="2-1">分类管理</el-menu-item>
                 <el-menu-item index="2-2">内容列表</el-menu-item>
               </el-menu-item-group>
             </el-submenu>
           </el-menu>
         </el-aside>
   
         <el-container>
           <el-header style="text-align: right; font-size: 12px">
             <el-dropdown>
               <i class="el-icon-setting" style="margin-right: 15px"></i>
               <el-dropdown-menu slot="dropdown">
                 <el-dropdown-item>个人信息</el-dropdown-item>
                 <el-dropdown-item>退出登录</el-dropdown-item>
               </el-dropdown-menu>
             </el-dropdown>
           </el-header>
           <el-main>
             <!--在这里展示视图-->
             <router-view />
           </el-main>
         </el-container>
       </el-container>
     </div>
   </template>
   <script>
     export default {
       name: "Main"
     }
   </script>
   <style scoped>
     .el-header {
       background-color: #B3C0D1;
       color: #333;
       line-height: 60px;
     }
     .el-aside {
       color: #333;
     }
   </style>
   ```

4. `npm run dev`启动，验证

   ![image-20220708174448505](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220708174448505.png)

   ![image-20220708174501639](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220708174501639.png)

# 18、参数传递及重定向

## 18.1、参数传递

**在上面的demo中继续改造，怎么传递参数呢？**

1. 修改Main.vue

   ```vue
   <!-- ....-->  
     <el-menu-item index="1-1">
       <!-- v-bind绑定参数 name属性的值需要和路由name保持一致 -->
       <router-link v-bind:to="{name:'UserProfile', params:{id:1}}">个人信息</router-link>
     </el-menu-item>
   <!-- ....-->  
   ```

2. 修改路由配置index.js

    ```js
    // ...
        {
          path: '/main',
          component: Main,
          // 嵌套路由
          children: [
            {path: '/user/list', name: 'UserList', component: UserList},
            {path: '/user/profile/:id', name: 'UserProfile', component: UserProfile}
          ]
        }
    // ...
    ```

    

3. 修改Profile.vue

      ```vue
      <template>
        <!-- 注意这里所有元素必须在根节点下，这里根节点就是div -->
        <div>
          <h1>个人信息</h1>
          <!-- 注意这里的route不要写错了 -->
          {{$route.params.id}}
        </div>
      </template>
      
      <script>
        export default {
          name: "UserProfile"
        }
      </script>
      
      <style scoped>
      
      </style>
      ```

4. 验证

      ![image-20220709104418263](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220709104418263.png)

      ![image-20220709104431057](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220709104431057.png)

**上面的基础上再优化一下，用props减少耦合**

1. 修改路由配置`props:true`

      ```js
      // ...
      
          {
            path: '/main',
            component: Main,
            // 嵌套路由
            children: [
              {path: '/user/list', name: 'UserList', component: UserList},
              {path: '/user/profile/:id', name: 'UserProfile', component: UserProfile, props:true}
            ]
          }
      // ...
      ```

2. 修改Profile.vue

      ```vue
      <template>
        <!-- 注意这里所有元素必须在根节点下，这里根节点就是div -->
        <div>
          <h1>个人信息</h1>
          {{id}}
        </div>
      </template>
      
      <script>
        export default {
          props: ['id'],
          name: "UserProfile"
        }
      </script>
      
      <style scoped>
      
      </style>
      ```

3. 验证同上



**再次加深一下印象，登录的时候把用户名显示在主页上**

1. 修改Login.vue 跳转主页面的时候带上用户名

   ```vue
   <!-- ... -->
   <script>
       // ...
         onsubmit(formName) {
           //为表单绑定验证功能
           this.$refs[formName].validate((valid) => {
             if (valid) {
               //使用vue-router路由到指定界面，该方式称为编程式导航
               this.$router.push('/main/'+this.form.username);
             } else {
               this.dialogVisible = true;
               return false;
             }
           });
         }
       // ...
   </script>
   <!-- ... -->
   ```

2. 修改路由配置index.js

   ```js
   // ...
   
       {
         path: '/main/:name',
         component: Main,
         props: true,
         // 嵌套路由
         children: [
           {path: '/user/list', name: 'UserList', component: UserList},
           {path: '/user/profile/:id', name: 'UserProfile', component: UserProfile, props:true}
         ]
       }
   // ...
   ```

3. 修改Main.vue

   ```vue
   <!-- ... -->
           <el-header style="text-align: right; font-size: 12px">
             <el-dropdown>
               <i class="el-icon-setting" style="margin-right: 15px"></i>
               <el-dropdown-menu slot="dropdown">
                 <el-dropdown-item>个人信息</el-dropdown-item>
                 <el-dropdown-item>退出登录</el-dropdown-item>
               </el-dropdown-menu>
             </el-dropdown>
             <span>{{name}}</span>
           </el-header>
   <!-- ... -->
   ```

4. 验证

   ![image-20220709112341289](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220709112341289.png)

## 18.2、重定向

1. 修改index.js  `redirect`

      ```js
      import Vue from "vue";
      import VueRouter from 'vue-router';
      import Main from "../views/Main";
      import Login from "../views/Login";
      import UserList from "../views/user/List";
      import UserProfile from "../views/user/Profile";
      
      Vue.use(VueRouter);
      
      export default new VueRouter({
        routes: [
          {
            path: '/login',
            component: Login
          },
          {
            path: '/main',
            component: Main,
            // 嵌套路由
            children: [
              {path: '/user/list', name: 'UserList', component: UserList},
              {path: '/user/profile/:id', name: 'UserProfile', component: UserProfile, props:true}
            ]
          },
          {
            path: '/goHome',
            redirect: '/main'
          }
        ]
      })
      ```

2. 修改Main.vue

      ```vue
      <!-- ... -->
                    <el-menu-item index="1-2">
                      <router-link to="/user/list">用户列表</router-link>
                    </el-menu-item>
                    <el-menu-item index="1-3">
                      <router-link to="/goHome">回到主页</router-link>
                    </el-menu-item>
      <!-- ... -->
      ```

3. 验证

      ![image-20220709105431950](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220709105431950.png)

# 19、404和路由钩子

## 19.1、404

**地址输入了一个不存在的，怎么办？另外地址中的#怎么可以不显示？**

1. 创建一个views/NotFound.vue

   ```vue
   <template>
     <div><h1>404，你的页面走丢了</h1></div>
   </template>
   
   <script>
     export default {
       name: "NotFound"
     }
   </script>
   
   <style scoped>
   
   </style>
   ```

2. 修改index.js

   ```js
   import Vue from "vue";
   import VueRouter from 'vue-router';
   import Main from "../views/Main";
   import Login from "../views/Login";
   import UserList from "../views/user/List";
   import UserProfile from "../views/user/Profile";
   import NotFound from "../views/NotFound";
   
   Vue.use(VueRouter);
   
   export default new VueRouter({
     mode: 'history',
     routes: [
       {
         path: '/login',
         component: Login
       },
       {
         path: '/main/:name',
         component: Main,
         props: true,
         // 嵌套路由
         children: [
           {path: '/user/list', name: 'UserList', component: UserList},
           {path: '/user/profile/:id', name: 'UserProfile', component: UserProfile, props:true}
         ]
       },
       {
         path: '/goHome',
         redirect: '/main'
       },
       {
         path: '*',
         component: NotFound
       }
     ]
   })
   ```

3. 验证

   ![image-20220709114718621](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220709114718621.png)



**路由模式：**

- hash：路径带 # 符号，如 http://localhost/#/login
- history：路径不带 # 符号，如 http://localhost/login

修改路由配置

```js
// ...
export default new VueRouter({
  mode: 'history',
  routes: []
// ...
```



## 19.2、钩子

**路由导航守卫**

- beforeRouteEnter

  ```
  在渲染该组件的对应路由被验证前调用
  不能获取组件实例 `this` ！
  因为当守卫执行时，组件实例还没被创建！
  ```

  `beforeRouteEnter` 守卫 **不能** 访问 `this`，因为守卫在导航确认前被调用，因此即将登场的新组件还没被创建。

  不过，你可以通过传一个回调给 `next` 来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数：

  ```js
  beforeRouteEnter (to, from, next) {
    next(vm => {
      // 通过 `vm` 访问组件实例
    })
  }
  ```

- beforeRouteUpdate

  ```
  在当前路由改变，但是该组件被复用时调用
  举例来说，对于一个带有动态参数的路径 `/users/:id`，在 `/users/1` 和 `/users/2` 之间跳转的时候，
  由于会渲染同样的组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
  ```

- beforeRouteLeave：在离开路由前执行

  ```
  在导航离开渲染该组件的对应路由时调用
  与 `beforeRouteUpdate` 一样，它可以访问组件实例 `this`
  ```

  

1. Profile.vue中使用钩子函数

   ```vue
   <!-- ... -->
   <script>
     export default {
       props: ['id'],
       name: "UserProfile",
       beforeRouteEnter: (to, from, next) => {
         console.log("进入个人信息页面")
         next();
       },
       beforeRouteLeave: (to, from, next) => {
         console.log("离开个人信息页面")
         next();
       }
     }
   </script>
   <!-- ... -->
   ```

   ```
   钩子函数参数说明
   to: 路由将要跳转的路径信息
   from：路径跳转前的路径信息
   next：路由的控制参数
   - next()：跳入下一个页面
   - next('/path')：改变路由的跳转方向，使其跳到另一个路由
   - next(false)：返回原来的页面
   - next(vm => {})：仅在beforeRouteEnter中也可用，vm是组件实例
   ```

2. 安装axios、vue-axios

   ```bash
   npm install axios vue-axios --save
   ```

3. main.js导入axios并使用

   main.js

   ```js
   import Vue from 'vue'
   import App from './App'
   //导入router
   import router from "./router";
   //导入ElementUI
   import ElementUI from 'element-ui';
   import 'element-ui/lib/theme-chalk/index.css';
   //导入axios
   import Axios from 'axios';
   import VueAxios from 'vue-axios';
   
   Vue.use(router);
   Vue.use(ElementUI);
   Vue.use(VueAxios, Axios);
   
   new Vue({
     el: '#app',
     router,
     render: h => h(App)
   })
   ```

4. 准备数据 static下新建目录mock，并创建文件data.json

   ```json
   {
     "name": "狂神说Java",
     "url": "https://www.baidu.com",
     "page": 1,
     "isNonProfit": true,
     "address": {
       "street": "含光门",
       "city": "陕西西安",
       "country": "中国"
     },
     "links": [
       {
         "name": "bilibili",
         "url": "https://space.bilibili.com/95256449"
       },
       {
         "name": "狂神说Java",
         "url": "https://blog.kuangstudy.com"
       },
       {
         "name": "百度",
         "url": "https://www.baidu.com/"
       }
     ]
   }
   ```

   说明： 只有我们的 static 目录下的文件是可以直接被访问到的，所以我们就把静态文件放入该目录下

5. Profile.vue中的beforeRouteEnter进行异步请求

   ```vue
   <template>
     <!-- 注意这里所有元素必须在根节点下，这里根节点就是div -->
     <div>
       <h1>个人信息</h1>
       <!-- 注意这里的route不要写错了 -->
       {{id}}
     </div>
   </template>
   
   <script>
     export default {
       props: ['id'],
       name: "UserProfile",
       beforeRouteEnter: (to, from, next) => {
         console.log("进入个人信息页面")
         next(vm => {
           //进入路由之前执行getData方法
           vm.getData();
         });
       },
       beforeRouteLeave: (to, from, next) => {
         console.log("离开个人信息页面")
         next();
       },
       //axios
       methods: {
         getData: function () {
           this.axios({
             method: 'get',
             url: 'http://localhost:8080/static/mock/data.json'
           }).then(response => console.log(response.data))
         }
       }
     }
   </script>
   
   <style scoped>
   
   </style>
   ```

6. 测试

   ![image-20220710100732650](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220710100732650.png)
