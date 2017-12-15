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

#### ElasticSearch
* 近实时 索引建立后,每隔x秒写入,一般设置为1s左右
* 分片 将索引分片存储,并建立副本

1. 引入依赖
>
    	compile('org.springframework.boot:spring-boot-starter-data-elasticsearch')
    	
    	ext['elasticsearch.version']='6.0.0' 版本和服务器安装的版本一致
>

2. 新建实体类
>
    /**
     * author:ZhengXing
     * datetime:2017-12-04 21:15
     * es文档类
     */
    @Document(indexName = "blog",type = "blog")
    @Data
    @NoArgsConstructor(access = AccessLevel.PROTECTED)//防止直接使用
    public class EsBlog implements Serializable{
        @Id
        private String id;//string的id
        private String title;
        private String summary;//摘要
        private String content;
    
        public EsBlog(String title, String summary, String content) {
            this.title = title;
            this.summary = summary;
            this.content = content;
        }
    }
>

3. 数据操作类:
和spring data jpa类似,都是被springData包装过的
>
    /**
     * author:ZhengXing
     * datetime:2017-12-04 21:22
     * es博客 文档
     */
    public interface EsBlogRepository extends ElasticsearchRepository<EsBlog,String> {
    
        /**
         * 查询不重复的,标题包含,或摘要包含,或正文包含该字符的分页的数据
         *
         * @return
         */
        Page<EsBlog> findDistinctEsBlogByTitleContainingOrSummaryContainingOrContentContaining(
                String title, String summary, String content, Pageable pageable
        );
    }
>

#### Elasticsearch 安装
1. 解压
2. 创建用户
>
    useradd zx  创建名为zx的用户
    passwd zx 给zx设置密码
    su zx 切换到zx用户
    
    切换到root
    chown zx /zx/elasticsearch-6.0.0 -R 给zx权限
>

3. elasticsearch.yml 增加 network.host: 0.0.0.0

4.  max file descriptors [65535] for elasticsearch process is too low, increase to at least [65536]
root用户下 
vim /etc/security/limits.conf 
在末尾增加
>
    * soft nofile 65536
    * hard nofile 65536
    * soft nproc 2048
    * hard nproc 4096
>

5. max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
vim /etc/sysctl.conf 
sysctl -p


5.启动 sh ./bin/elasticsearch

6. 访问http://106.14.7.29:9200 即可看到信息

7. 停止   
JPS 看进程号  
kill  -9  进程号  

* 需要注意的是spring boot stater data elasticsearch 中整合的是2.4.6版本的elasticsearch.需要安装最低支持版本的才能操作.  
如果需要使用最新版,就无法使用spring data框架


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

