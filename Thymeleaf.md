#### Thymeleaf

* 两种标签
    * \<span th:text="...">  需要在html标签中引入命名空间 <html xmlns:th="http://www.thymeleaf.org">
    * \<span data-th-text="..."> html5的自定义属性,无需引入命名空间  
    
* 变量表达式 ${...}
    * \<span th:text="${book.author.name}">
    
* 消息表达式 #{...}  也称为文本外部化/国际化/或i18n    
    * \<th th:text="#{header.address.city}">
    
* 选择表达式 *{...}   
与变量表达式的区别在于,它是在当前选择的对象,而不是整个上下文变量映射上执行的.
>
    <!--取book中的title-->
    <div th:object="${book}">
        <span th:text="*{title}">...</span>
    </div>
>
    
* 链接表达式 @{...}  
    * 链接表达式可以是相对的,在这种情况下,应用程序上下文将不会URL的前缀  
        * \<a th:href="@{../document/report}">...</a>
    * 也可以是服务器相对(同样,没有应用程序上下文前缀)(~表示服务器名或ip)  
        * \<a th:href="@{~/document/report}">...</a>
    * 和协议相对(和绝对url一样,但浏览器将在显示的页面使用对应的http或https协议)
        * \<a th:href="@{//static.xxx.com/document/report}">...</a>
    * 绝对的
        * \<a th:href="@{http://static.xxx.com/document/report}">...</a>

* 分段表达式 th:insert或th:replace  
有一个代码片段,名为copy
>
    <div th:fragment="copy">
        &copy; 2017 <a href="https://xxx.com">xxx.com</a>
    </div>
>
如下代码,可以将上面的代码片段拷贝
>
    <div th:insert="~{footer :: copy}"> </div>
>

* 字面量
    * 文字
        * \<p> xxxxda <span th:text="'xx'">tempalte file</span> </p>
    * 数字
        * \<span th:text="2013"></span>
        * \<span th:text="2013 + 3"></span>
    * 布尔
        * \<div th:if="${user.isAdmin()} == false">
    * null 
        * \<div th:if="${user.isAdmin()} == null">    
    * 算术操作 + - * / %
        * \<div th:with="isEven=(${prodStat.count} % 2 == 0) ">
    * 比较 > < >= <= (gt lt ge le,标签可能和<> <= 存在冲突之类的)
        * \<div th:if="${user.age  le 7}">  
    * 等价 == != (eq ne)
    * 条件运算符(三目运算符)
        * \<tr th:class="${row.isEven} ? 'even' : 'odd'"> 
    * 无操作 _ 
        * \<span th:class="${row.isEven} ? 'even' : _">xxx</span> 
        
* 设置属性值
    * 设置任意属性值 th:attr
        * \<form action="xxx" th:attr="action=@{/subscribe}">
        * \<input value="xxx" th:attr="value=#{user.name}">
    * 设置指定属性值 例如 th:value th:action th:class th:background 
        * \<form action="xxx" th:action="@{/subscribe}">
    * 固定值布尔属性 th:checked th:disabled  th:hidden
        * \<input type="checkbox" name="xxx" th:checked="${user.active}">  会显示为 <input type="checkbox" name="xxx" checked > ,表示已选中
* 迭代器
    * \<li th:each="book : ${books}" th:text="${book.title}">遍历每个book</li>
    * 状态变量: index(索引) count(类似索引,但从1开始) size(总数) current(当前变量) even/odd(奇数/偶数) first(第一个) last(最后一个) 
        * \<tr th:each="a,iterStat : ${c}" th:class="${iterStat.odd}? 'odd'"> 如果当前是奇数索引.设置class
* 条件语句
    * th:if 
        * \<a th:if="${not #lists.isEmpty(user.list)}">view</a> 该集合不为空或长度0,才显示
    * th:unless 相当于if取反
        * \<a th:if="${#lists.isEmpty(user.list)}">view</a> 该集合不为空或长度0,才显示
    * switch 只会执行第一个为true的case
    >
        <div th:switch="${user.role}">
            <p th:case="'admin'">xxxx</p>        
            <p th:case="${role.maneger}">xxxx</p>        
            <p th:case="*">xxxx</p>        
        </div>
    >
              
* 模版布局  
    * 使用
    定义模版片段
    >
        <div th:fragment="copy">xxxxxxx<div>
    >
    引用片段  
    footer表示页面名称
    >
        <div th:insert="~{footer :: copy}"></div>
    >
    
    如果不使用th:fragment,也可引用.例如如下
    >
        <div id="copy">xxx</div>
    >
    引用,加个#即可
    >
        <div th:insert="~{footer :: #copy}"></div>
    >
    
    * th:insert th:replace th:include 三者区别
        * th:insert 简单地插入片段作为正文的主标签
        * th:replace 用片段替换主标签(也就是引用片段的那个主标签被替换了)
        * th:include 类似th:insert,但不插入片段,只插入片段的内容(3.X版本后,不推荐使用)

* 属性优先级 可自行查询对应优先级
    * 在同一个标签中写入多个th:*
        * <li th:each="item : ${items}" th:text="${item.descript}">xxx</li>  
        此处each优先级高于 text,所以会先执行each,再执行text

* 注释  
    * 标准html注释 
    >
        <!--我是注释-->
    >  
    * 解释器级别的注释块 在看html原型界面时会显示,在解析时被删除
    >
        <!--/*--> 我是注释<!--*/-->
    >
    * 原型注释块 在原型中不可见,在解析时会被模版执行
    >
        <!--/*/ 我是注释 /*/-->
    >    

* 内联        
    * 使用
    >
        [[...]] 或 [(...)] 分别对应 th:text 和 th:utext 
        utext不对特殊符号转义(html的转义)
        
        <p>xxx is [(${msg})]</P>
    >
    js内联
    >
        <script th:inline="javascript">
            var username = /*[[${session.name}]]*/ "sxxxxxxx";
        </script>
    >
    css内联
    >
        <style th:inline="css">
            .[[$classname]]{
                text-aligin: [[${aligin}]]
            }
        </style>
    >
    * 禁用内联
    >
        <p th:inline="none">xxx is [[1,2,3]]</P>
    >

* 表达式基本对象 模版初始化时就注入了的直接可以使用的
>
    #ctx:上下文对象,是org.thymeleaf.context.IContext或
    org.thymeleaf.context.IWebContext的实现
    
    可用它获取session等
    ${#ctx.request}
    ${#ctx.response}
    ${#ctx.session}
    ${#ctx.servletContext}
>

>
    #locale:直接访问与java.util.Locale关联的当前的请求
>

>
    request/response等属性
    param:检索请求参数
    session:检索session属性
    application:检索application/servlet上下文属性
>

>
    Web上下文对象
    #request:直接访问HttpServletRequest对象
    #session
    #servletContext
    
    例如
    ${#request.getAttribute('xxx')}
>

* 表达式工具对象
    * 执行信息
    >
        #execInfo:表达式对象提供有关在Thymeleaf标准表达式内正在处理的模版的有用信息
    >
    * 消息
    >
        #mseeages:用于在表达式中获取外部化消息的实用方法,与'#{...}'语法获得的方式相同
    >
    * URI/URL
    >
        #uris:在Thymeleaf标准表达式中执行URI/URL操作(尤其是转义/取消转义)的实用程序对象
    >
    * 转换
    >
        #cionversions:允许在模版任何位置执行转换服务
    >
    * 日期
    >
        #dates:java.util.Date对象的实用方法
    >
    * 日历
    >
        #calendars:类似于#dates,对应java.util.Calendar对象
    >
    * 数字
    >
        #numbers
    >
    * 字符串
    >
        #strings
    >
    * 对象
    >
        #objects
    >
    * 布尔
    >
        #bools
    >    
    * 数组
    >
        #arrays
    >  
    * 链表
    >
        #lists
        #sets
        #maps
        #aggregates:聚合,统计总数,求平均值
    >  
    * ID
    >
        #ids:处理重复的id属性,例如作为一个迭代器的结果
    >    
    