# 如何开发出优秀的APICloud应用

> APICloud定制平台项目实施规范
>
> APICloud应用优化策略Top30
>
> 如何开发出运行体验良好、高性能的App
>
> 如何开发出客户满意、能够顺利交付的App

### 1. 引擎或模块问题：遇到应用层无法解决的问题，如果能确定需要引擎和模块支持的，不要自己想办法绕过去，要第一时间在开发者社区提交问题，或找APICloud项目经理提出。

**!!!注意!!!:** 在开发者社区中，会有版主和APICloud技术支持对您的问题进行验证和解答。

**!!!注意!!!:** 定制平台项目问题提出后2天之内没有解决的，可以直接找APICloud项目总监投诉。


### 2. 开发工具：推荐使用Sublime Text＋APICloud插件，调试工具使用自定义Loader，真机同步使用WiFi真机同步，日志输出使用WiFi日志输出。

> *推荐视频：[Sublime使用教程Window&Mac](http://www.apicloud.com/video_list)*
  
> *推荐文档：[Sublime插件使用说明](http://docs.apicloud.com/Dev-Tools/sublime-apicloud-plugin)*

### 3. 前端框架：尽量不要使用jQuery、AngularJS、BootStrap等重型的框架，摆脱对$的依赖，培养自己动手的习惯，但是可以根据功能需求在特定页面中使用功能独立的Mobile First框架
 
> 默认样式设置、DOM操作和字符串处理推荐使用APICloud前端框架(api.js和api.css)

> 移动端UI框架推荐使用AUI

### 4. 屏幕适配：要正确设置viewport，建议使用720*1280尺寸的UI图，优先考虑绝对计量类的单位 px，应先在UI效果图中（如720x1280尺寸图）量出元素的宽或高对应的 px 值，再除以屏幕倍率（如分辨率为720x1280设备的屏幕倍率通常为 2) 来得到书写样式时的确切数值。

**!!!注意!!!：** APICloud项目验收时会根据设计提供的UI图尺寸（如720x1280），在对应屏幕分辨率的手机设备(如720x1280）中安装运行，将运行后的页面与UI效果图一一进行对比。
 
**!!!注意!!!：** H5界面的实现要与UI设计完全一致，精细到0.5px。

**!!!注意!!!：** openFrame/FrameGroup等时，应使用auto结合margin布局，以动态适应变化无常的android设备屏幕。

> *推荐文档：[屏幕适配原理及实现](http://docs.apicloud.com/Dev-Guide/screen-adapt-guide)*

### 5. UI布局：要求使用APICloud五大组件（Widget、Layout、Window、Frame、UIModules）进行APP的UI架构设计。

**!!!注意!!!：** SPA的模式不适合APP开发，DIV＋JS的窗口切换影响用户体验。APICloud的UI结构设计可以从整体上解决H5在Interaction、Animation和Render方面的性能问题。

> *推荐文档：[培训讲义：APICloud界面布局和APP架构设计](http://docs.apicloud.com/Seven/Day1)*

### 6. 窗口切换：避免出现任何卡顿、闪屏、白屏等情况；动画效果流畅，不能出现丢帧的情况。

**!!!注意!!!：** 要理解并控制窗口好切与界面渲染之间的关系，要适时更新UI，如果Window或Frame中所加载的静态页面内容过多，建议等动画执行完毕再进行页面的加载和渲染。无论是Android还是iOS系统，在进行窗口切换的时候，如果窗体本身正在进行渲染（Window或Frame所加载的网页没有渲染完毕），则会影响切换动画运行的流畅性，出现卡顿或丢帧的情况。

**!!!注意!!!：** 建议在打开Window或Frame的时候，如果所加载的静态网页不能过大，内容不要太多，不能快速的渲染完毕。为了不影响窗体切换动画的执行，可以在切换动画执行完毕后再进行动态数据的加载和界面的刷新。

### 7. 窗口切换动画：如果没有特别要求尽量使用平台默认的动画效果，即api.openWin时不指定动画类型，使用默认值。

**!!!注意!!!：** 无论是在Android还是iOS上，APICloud引擎会从整体上保证默认的窗口动画类型是性能最好的。

**!!!注意!!!：** 三星、小米等大屏Android6.0及以上手机，可以尝试在云编译的时候选择使用[Android引擎渲染优化版本](http://community.apicloud.com/bbs/forum.php?mod=viewthread&tid=49684&extra=page%3D1)

**!!!注意!!!：** 如果窗体所加载的静态网页内容比较多(如：初始的Dom树很大或图片很多)，在Android平台上openWindow的时候可以尝试使用movein或fade的[动画类型](http://docs.apicloud.com/Client-API/api#b6)

### 8. 窗口关闭处理：开发过程中根据需要处理Android的keyback事件和iOS的回滑手势。

**!!!注意!!!**：Android上要在Window中才能监听到keyback事件，Frame中无法监听到keyback事件；在iOS7以上的系统上可以在openWin的时候通过设置[slidBackEnabled参数](http://docs.apicloud.com/Client-API/api#33)来实现是否支持回滑手势关闭窗口的功能。

**!!!注意!!!**：在后台关闭页面时，应注意在关闭方法中添加animation:{type:"none"}，来防止切换动画的出现影响用户体验；

### 9. 窗体背景图片：避免使用H5来实现body级别的背景图片，可以使用Window或Frame的bgColor参数以原生的方式来高效实现

**!!!注意!!!：** 不建议通过给body元素指定background的方式来实现body级别的背景图片，特别是高清的大背景图片用H5方式实现会严重影响渲染性能。

### 10. 导航切换：切换底部导航或顶部分类菜单的时候，要求切换体验平滑，切换过程不能出现白屏、闪屏等现象

**!!!注意!!!**：建议使用FrameGroup来实现Frame的切换，要按需合理配置预加载的Frame数量，每个Frame要有明显的刷新机制，不能每次切换都进行刷新和重绘。

**!!!注意!!!**：如果使用模块来实现底部导航栏推荐使用NVTabBar模块。

### 11. 列表滚动：滚动效果要平滑流畅，不能使用iscroll等JS的方式来实现滚动

**!!!注意!!!**：建议使用Window+Frame的UI结构，以Native的方式来实现列表页面的滚动。

**!!!注意!!!**：在iOS上要支持点击状态栏能自动回到顶部的效果，可以通过在openWin或openFrame的时候配置[scrollToTop参数](http://docs.apicloud.com/Client-API/api#33)来实现；此效果在FrameGroup中使用的时候要注意确保只有当前显示的Frame的scrollToTop属性为true，其它Frame的scrollToTop属性为false。

### 12. 界面之间参数传递：可以使用pageParam来实现，但要避免使用过大的pageParam

**!!!注意!!!：** 界面切换的时候如果pageParam过大，则JSON解析就会比较耗时，影响界面切换的执行和动画运行体验。

**!!!注意!!!：** 不要使用使用URL+?的形式进行参数的传递，此方式在Android上存在兼容问题。

### 13. 交互响应：点击事件必须处理click事件的300ms延迟问题，优化点击响应速度，建议通过为可点击的元素增加tapmode属性来优化点击速度。

**!!!注意!!!**：引擎对具有tapmode属性的元素点击事件的优化处理会在apiready事件触发之前，根据当前的dom树自动进行优化。在apiready之后加载的数据使用要显式的调用api.parseTapmode方法来进行主动的tapmode处理，例如在上拉加载更多数据后，要调用一下api.parseTapmode方法.

**!!!注意!!!**：要按UE设计确定可点击区域的大小，可以适当扩大点击区域来保障点击反应的灵敏。

**!!!注意!!!**：api.parseTapmode调用会有性能成本，不需要的情况下不要随便调用。

**!!!注意!!!**：要按照需求明确所有按钮点击时的交互效果，为tapmode属性设置正确的样式值，对于没有交互效果的点击实现，可以不为tapmode属性指定任何样式，但是为了优化点击速度，必须要给元素增加tapmode属性。

### 14. 下拉刷新效果：建议不要使用APICloud默认的下拉刷新效果（灰色箭头），要使用模块来实现UE/UI所设计的下拉刷新效果。

**!!!注意!!!**：如果UE/UI所设计的下拉刷新效果，使用目前APICloud平台模块无法实现，要第一时间跟项目经理提出，由APICloud进行模块封装来实现。

### 15. 网络通信方式：必须使用api.ajax，并且设置合适的超时时间，并进行超时和请求失败的异常情况。

**!!!注意!!!**：JQuery的ajax在开启全包加密的时候会有问题，不建议使用。

### 16. 网络请求状态处理：APP要判断当前的网络状态，请求过程要按UI设计有明显的状态提示；网络超时或网络请求失败的时候要进行相关处理并有错误提示。

> [api对象](http://docs.apicloud.com/Client-API/api)和[dialogBox模块](http://docs.apicloud.com/Client-API/UI-Layout/dialogBox)下面封装了常用的提示对话框方法。

### 17. 数据缓存：要对GET请求进行数据的缓存处理，在用户没用网络的情况下，仍然能够看到APP的静态界面布局以及上次已经缓存的服务器端数据。

**!!!注意!!!**：可以在api.ajax方法中设置[cache参数](http://docs.apicloud.com/Client-API/api#3)为true来开启缓存；也可以使用api.writeFile和api.readFile方法，在获取数据后自己实现简单的数据缓存，或使用fs和db模块来缓存数据。

### 18. 图片缓存：必须手动进行图片的缓存处理，需要调用[api.imageCache](http://docs.apicloud.com/Client-API/api#78)方法实现。

**!!!注意!!!**：Webview默认的缓存机制存在缺陷，在跨窗口时表现不好，并且存在对所缓存图片的尺寸限制等问题，所有APICloud应用的图片缓存不能依赖Webview默认的缓存机制，必须手动实现。

### 19. 图片处理：要减少由图片造成的内存占用，减少图片缩放等耗性能的操作，服务器端要根据产品设计提供合适尺寸的大图、小图、缩略图等

**!!!注意!!!**：APICloud应用所占用的内存大小由所加载的网页大小决定，通常图片过多过大会造成整个应用的内存占用过大，另外在浏览器中进行图片的缩放处理成本也很高。

**!!!注意!!!**：列表中的头像等缩略图，宽高应控制在250-300px之间，小于这个范围大屏手机容易失真，大于这个范围消耗更多内存和性能。

### 20. 状态栏效果：Android和iOS上都要求实现沉浸式状态栏效果的适配

**!!!注意!!!**：可以通过在config.xml中开启[沉浸式效果](http://docs.apicloud.com/Dev-Guide/app-config-manual#10)]配置项，然后在Window或Frame的apiready事件后，调用[$api.fixStatusBar()](http://docs.apicloud.com/Front-end-Framework/framework-dev-guide#45)方法来实现。如果由于各种原因造成apiready执行太晚，当Header高度变化时会产生页面跳动的现象，也可以根据需求自己来实现，在合适的时机（如onload事件中）判断平台类型后，手动调整Header的高度，Android的状态栏高度是25px，iOS是20px。

**!!!注意!!!**：要根据当前界面的背景颜色，通过调用[api.setStatusBarStyle方法](http://docs.apicloud.com/Client-API/api#47)来设置当前状态栏的风格或背景色。

### 21. 键盘处理：在打开带有输入框的Window或Frame的是，默认要自动让输入框自动获得焦点。

**!!!注意!!!**：在config.xml中有关于键盘显示方式，弹出方式和第三方键盘使用的各种配置，要根据需要正确配置。

**!!!注意!!!**：由于在Android上input元素的focus事件存在兼容性问题，要完成输入框自动获取焦点的功能，建议使用扩展模块[UIInput模块](http://docs.apicloud.com/Client-API/UI-Layout/UIInput)。

**!!!注意!!!**：在打开Window的时候，如果自动弹出键盘，弹出键盘的行为影响切换动画执行的流畅性，出现卡顿或丢帧的情况。建议可以对键盘弹出的行为设置适当的延迟，例如在apiready中设置延迟200ms后再让UIInut元素获得焦点。

**!!!注意!!!**：可以在同一个界面中（如登陆界面）创建多个UIInput模块的实例，来实现多个输入框。

**!!!注意!!!**：输入框位于设备屏幕下半部份的应用场景，config.xml中的的键盘弹出模式参数[softInputMode](http://docs.apicloud.com/Dev-Guide/app-config-manual#14-0)务必设置为resize模式，或者使用[UIInput](http://docs.apicloud.com/Client-API/UI-Layout/UIInput)相关模块。

**!!!注意!!!**：为了让应用看起来更接近原生，尽量配置config.xml中的[softInputBarEnabled](http://docs.apicloud.com/Dev-Guide/app-config-manual#14-11)参数来隐藏iOS键盘上面的工具条。也可以在openWin或openFrame的时候通过[softInputBarEnabled参数](http://docs.apicloud.com/Client-API/api#33)来单独指定。

### 22. 配置外部字体：可以根据项目的需要引入外部字体，但是要控制外部字体文件的大小，字体文件不宜过大。

**!!!注意!!!**：Android上默认有3种字体：sans, serif, monospace，在开发人员不指定的情况下，默认为sans，这3种字体在开发过程中都是通过字体名进行引用，系统会自动对应到内置字体文件。但是，对于外部的字体文件，Android上无法实现通过引擎配置后成为内置的字体文件，只能通过@font-face的方式在每个页面中重复加载，每一个要使用外部字体的Window或Frame都要引入一遍，如果字体体积过大会占用大量内存，并且影响页面的加载速度。

**!!!注意!!!**：iOS可以在config.xml文件中进行[外部字体文件的配置](http://docs.apicloud.com/Dev-Guide/app-config-manual#14-1)，配置完成后就可以像系统内置字体一样在页面中指定了，无需在每个Window或Frame中通过@font-face的方式引入。

### 23. JavaScript模版：建议使用doT模版等轻量级的模版。

**!!!注意!!!**：要优先选择使用Mobile First的模版，体量小，生成的文本效率高。

> [doT模版文档](https://github.com/apicloudcom/apicloud-js-module)

### 24. 支付业务：支付宝，微信等密钥必须存放在服务器端，不应暴露在APP代码中。

**!!!注意!!!**：支付订单金额应由服务器产生，服务器一定要对支付宝、微信服务器回调的支付结果做最终校验。

**!!!注意!!!**：alipay模块要调用payOrder方法来进行支付，自己处理订单信息以及签名过程；不要使用config接口和pay接口把订单信息以及签名过程交予模块内部处理（官方提供此种支付方式只是为了方便开发者调试）。

### 25. 对于文件、数据库、偏好设置等操作推荐使用同步接口(方法名增加Sync后缀)来简化代码的实现，解决异步callback层次过深的问题。

> [fs对象的同步方法](http://docs.apicloud.com/Client-API/Func-Ext/fs)
> 
> [db对象的同步方法](http://docs.apicloud.com/Client-API/Func-Ext/db)
> 
> [偏好设置操作的同步方法](http://docs.apicloud.com/Client-API/api#21)

> 对于异步callback嵌套的问题，也可以通过调用api.sendEvent方法来解耦，通过事件机制来实现。

### 26. 网页代码组织：尽量将同一个界面的HTML、CSS和JS代码写在一个html文件中，提高页面加载速度；公用的CSS、JS尽量少和小，不要在html页面中随意加载无用的CSS或JS文件；尽量减少页面中的link或script标签的使用

**!!!注意!!!：** 在浏览器中，外部文件的引入和加载过程是同步操作，影响整个页面的执行效率。

### 27. 应用代码组成：要遵循APICloud Widget包结构，结构清晰规范。

> *推荐文档：[APICloud Widget包结构](http://docs.apicloud.com/Dev-Guide/widget-package-structure-manual)*

### 28.文件命名规范：要有统一规范，如首页Windowhome文件命名为home.html，首页Frame文件命名为home_frame.html，所有文件名(网页和资源文件)避免使用中文命名、也不要包含大写字母。

**!!!注意!!!**：原生系统内部资源文件管理不支持中文名和大写字母，使用中文或大写的资源文件在真实设备运行中会出现各种问题。

**!!!注意!!!**：例如在自定义Loader中运行没有问题，但云编译的包就有问题，出现页面无法加载或资源找不到等问题，通常就是使用了中文或大写的文件命名。因为官方Loader或自定义Loader的Widget是存放在SDCard中，而云编译后的安装包Widget是存在应用的沙箱中，沙箱中是要采用的原生系统的内部资源文件管理机制。

### 29. 安全机制：要从代码、数据存储、网络通信等方面保证APP的内容和数据的安全。

**!!!注意!!!**：开发过程中每次云编译的无论测试包还是正式包都建议选择全包加密，因为在APICloud定制平台上，客户可以全程监控项目的实施过程，可以查看代码提交纪录，但是没有获取代码的权限；客户可以查看云编译纪录，如果编译的安装包没有使用全包加密则客户可能通过解压安装包轻松获取APP的H5源码，从而影响后续项目款的按时支付。

**!!!注意!!!**： config.xml中的[access配置项](http://docs.apicloud.com/Dev-Guide/app-config-manual#3-1)可以配置在哪些类型的页面里面可以访问APICloud的扩展API方法，可访问域的设置以及越狱限制等。

**!!!注意!!!**： config.xml中的[checkSslTrusted配置项](http://docs.apicloud.com/Dev-Guide/app-config-manual#14-12)配置是否检查https证书是受信任的。

**!!!注意!!!**： config.xml中的[appCertificateVerify配置项](http://docs.apicloud.com/Dev-Guide/app-config-manual#14-12)配置是否校验应用证书。若配置为true，应用被重签名后将无法再使用。

**!!!注意!!!**： 对重要参数变量进行必要的加密处理，对重要的常量数据应放入[key.xml](http://docs.apicloud.com/Dev-Guide/key.xm-config-instruction)中，使用[api.loadSecureValue方法](http://docs.apicloud.com/Client-API/api#64)进行数据读取；

### 30. 安装包大小：云编译生成的安装包的大小由4部分内容组成：引擎、模块、网页文件和资源文件。引擎的大小是固定的（Android约为400K，iOS约为1.2M），应该控制减少模块、网页文件和资源文件的大小，删除无用的模块和文件。

**!!!注意!!!**：编译正式版本的时候，要检查一下控制台选定的模块是否都在实际代码中使用到了。一些开发者在开发过程中会不断引入一些“预计使用”或"测试使用"的模块，但是在最终的代码中没有使用，这部分模块要云编译的时候去掉，无用的模块不仅仅会增大安装包的体积，还有可能引起于其它模块的冲突或编译选项，造成编译失败。

**!!!注意!!!**：在config.xml文件中配置的模块，在控制台无法删除，因为config中feature配置项的forceBind属性默认true，是强制绑定的，可以通过在配置[forceBind属性](http://docs.apicloud.com/Dev-Guide/app-config-manual#15)来修过。

**!!!注意!!!**：在编译正式版本的时候，要删除Widget包中的icon和launch目录下的图片以减小安装包体积。














