#### BUG
* !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
* 虽然这个bug解决起来很快.但是.我只能说..好难受的bug.
* Controller中,无论是不是根路径,都需要指定一个"/",不然可能会出现F12后.告诉你静态文件类型不对的错误.

#### 奇淫巧技
* ResponseEntity. 该类可以无需在方法上加@ResponseBody注解,然后返回实体和状态码

* 直接在resource下放上名为favicon.ico,即可改变网站图标

* @AutoConfigureMockMvc

#### Thymeleaf和SpringBoot集成
1. 引入依赖
> compile('org.springframework.boot:spring-boot-starter-thymeleaf')
2. 修改默认thymeleaf版本,在buildscript{...}增加
>
	//自定义thymeleaf和它的方言的版本.因为默认版本比较低
	ext['thymeleaf.version'] = '3.0.3.RELEASE'
	ext['thymeleaf-layout-dialect.version'] = '2.2.0'
>
3. 可直接在项目根目录输入gradlew bootRun 启动项目
4. yml中如下设置:
>
    spring:
      thymeleaf:
        encoding: UTF-8
        #热部署静态文件
        cache: false
        #使用html5标准,默认就是
        mode: HTML5
>

#### Thymeleaf用户管理页面搭建
* 页面需要导入的命名空间:  
>
    <html xmlns:th="http://www.thymeleaf.org"
        xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout">    
>

#### H2内嵌数据库
1. 导入依赖
>
    	runtime group: 'com.h2database', name: 'h2', version: '1.4.196'
>
2. 无需配置dataSource即可运行程序
3. (貌似不需要)在中配置:
>
    spring:
      h2:
        console:
          enabled: true
>
4. 可查看/h2-console,查看其内存数据库中的数据






#### 前端框架
* Tether 1.4.0 http://tether.io  处理绝对布局
* Bootstrap v4.0.0-alpha.6 
* Jquery 3.1.1 
* Font Awesome 4.7.0  图标库
* NProgress 0.2.0 进度条
* Thinker-md markdown编辑器
* Jquery Tags Input 1.3.6 标签
* jQuery tagEditor 
* Bootstrap Tags Input
* Bootstrap Chosen 1.0.3 下拉控件
* toastr 2.1.1 信息提示插件
* jquery image cropping 图片裁剪 https://github.com/fengyuanchen/cropperjs
* cropbox 头像裁剪 https://github.com/hongkhanh/cropbox


#### IDEA GRADLE springBoot 热部署
1. compile group: 'org.springframework.boot', name: 'spring-boot-devtools', version: '1.4.3.RELEASE'
2. build.gradle
>
    bootRun {   
          addResources = true
    }
>
3. 使用bootRun运行(命令行是 gradlew bootRun)
4. idea的设置中的Complier中的自动编译勾上
5. ctrl + shift + alt + /,选择registry... , 勾上 when.app.runing

#### SpringSecurity 和 Thymeleaf 
1. 导入依赖
>
    	compile('org.springframework.boot:spring-boot-starter-security')
    	compile('org.thymeleaf.extras:thymeleaf-extras-springsecurity4:3.0.2.RELEASE')
>

2. 配置类
3. 在页面导入命名空间
> xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity4"



#### 总结
这个教程买得贼亏,就照着写了前面的一点,几乎没有什么出彩的地方.
后面那老师自己都是狂粘贴.我全部快进看了.一无是处.








