本项目是邹海清同学demo的补充   ，邹同学博客- http://z77z.oschina.io 

1.本项目库表设计：

1. 用户表    用户角色关系表   角色表     角色权限关系表  权限表

 这一套表，用以辅助shiro，实现不同用户，对不同模块访问权限的控制

2.用户表     用户用户组关系表   用户组表  用户组角色关系表 用户表

这一套表，用以辅助自定义注解，实现不同用户，对业务层不同方法的权限控制

2.项目实现的功能：

       2.1  登录权限控制

 密码明文加密：用户信息登录认证，表单提交之后，根据用户唯一标示，获取用户信息，将用户名和保存的随机盐拼接，将表单密码+随机盐组合进行MD5加密，比较数据库中保存的密码，是否和得到的加密密码一致；

         密码重试次数的限制：shiro的身份验证，会调用MyShiroRealm里面的doGetAuthenticationInfo()获取数据库中用户信息，限制这个方法的执行次数达到目的；在Redis里面存储用户重试次数，登录成功清0，达到次数，失败报错！

         验证码校验：利用GIF验证码插件生成动态验证码，保存在当前会话session中，登录时，在登录方法中校验验证码通过后才会，继续shiro的subject.login;

         记住我的功能：保存一个name为rememberMe的cookie到客户端;利用shiro的remeberMe管理器;

      2.2权限控制：

2.2.1. 动态权限控制和Redis缓存权限   将权限数据存放在数据库中，shiro配置加载权限数据的时候，从数据库中读取，前台进行权限更新之后，利用ShiroFilterFactoryBean去清除加载的配置，再重新设置链接访问权限;在进行访问权限判断的时候，获取权限数据可以从redis里面获取权限数据，因为权限数据不经常改动，缓存了权限，减少和数据库的交互;

2.2.2自定义拦截器                               

   shiro支持自定义访问拦截器：AccessControllerFilter,重写里面的方法，一个权限判断，返回是否可以有权限继续访问，一个设置访问拒绝后，是否继续处理其他拦截器; isAccessAllowed;一个onAccessDeneied;

 

b.1.1在线用户人数管理，超过踢出用户；在cacheManager里面，维护一个cache,缓存每个用户对应的sessionId队列;没有加入，有判断个数是否达到上限，达到标志 当前回话中session，的是否踢出状态，根据请求类型返回不同结果，ajax请求返回对应的json串，页面请求，直接达到没有权限页面或者特别设置的踢出页面；

b.1.2 自定义角色鉴权过滤器，限定不同类型的URL请求，需要哪些权限才能有权限可以访问；

1.首先在数据库中维护一组初始化权限数据， 数据库中数据格式：

/system/manage/**=perms["user:sysConfig"]

/org/manage/**=perms["user:organization"]

/contract/manage/**=perms["user:contract"]

/salary/manage/**=perms["user:salary"]

 

      2.2.3在应用启动加载到shiro的filterChainDefinitionMap；

      2.2.4 重写，isAccessAllowed方法判断subject，是否拥有权限可以访问；

              2.2.5.重写  onAccessDenied方法，访问被拒绝之后，如何处理当前请求，返回json串提示；还是跳转没有访问权限页面；

        2.2.6  使用自定义注解，在service层切入方法访问权限的判断，通过aop切入+反射拿到方法上是否有验证权限的标识，从而判断方法是否需要鉴权

      2.2.7: 各个表增删改查，以及相互之间关系表的维护


3. 需要诸位帮忙破解问题

 springBoot+shiro  使用shiro注解无效，不能说完全无效；

        开启注解的配置如下：

  @Bean   

         public static  LifecycleBeanPostProcessor lifecycleBeanPostProcessor() {  

    LifecycleBeanPostProcessor lifecycleBeanPostProcessor = new LifecycleBeanPostProcessor();  

    System.out.println("LifecycleBeanPostProcessor:"+lifecycleBeanPostProcessor);

                         return lifecycleBeanPostProcessor;    

          }

        

    @Bean

@ConditionalOnMissingBean

@DependsOn("lifecycleBeanPostProcessor")

    public DefaultAdvisorAutoProxyCreator defaultAdvisorAutoProxyCreator() {

        DefaultAdvisorAutoProxyCreator daap = new DefaultAdvisorAutoProxyCreator();

        daap.setProxyTargetClass(true);

        return daap;

    }



@Bean

    public AuthorizationAttributeSourceAdvisor authorizationAttributeSourceAdvisor() {

        AuthorizationAttributeSourceAdvisor aasa = new AuthorizationAttributeSourceAdvisor();

        aasa.setSecurityManager(securityManager());

        return aasa;

        }




我的application.properties  AOP 配置:  spring.aop.proxy-target-class=true；

我的业务层service 没有采用接口，全部是直接实现类；

此时现象1：我的Controller类里面方法的  shiro注解有效，但是service层里面方法上  shiro注解无效，自定义注解aop切入失败；


当我将：

@Bean

@ConditionalOnMissingBean

@DependsOn("lifecycleBeanPostProcessor")

    public DefaultAdvisorAutoProxyCreator defaultAdvisorAutoProxyCreator() {

        DefaultAdvisorAutoProxyCreator daap = new DefaultAdvisorAutoProxyCreator();

        daap.setProxyTargetClass(true);

        return daap;

    }

        这部分配置去掉之后，自定义注解AOP切入成功，Controller类方法上shiro注解有效，Service层方法上shiro注解依然无效；


       初步判断问题可能在：DefaultAdvisorAutoProxyCreator     一旦我使用了这个配置，controller注入的service bean  不是一个aop代理 也不是cglib代理  不是jdk代理

去掉这个配置之后，controller注入service  是一个aop代理 是一个cglib代理  不是jdk代理 ;

       最终问题：service 层方法上shiro注解无效 求破解！！！



 资源下载路径 需要启动redis  数据库等配置都在application.properties里面 

  http://download.csdn.NET/detail/u012581573/9805572