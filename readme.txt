����Ŀ���޺���ͬѧdemo�Ĳ���   ����ͬѧ����- http://z77z.oschina.io 

1.����Ŀ�����ƣ�

1. �û���    �û���ɫ��ϵ��   ��ɫ��     ��ɫȨ�޹�ϵ��  Ȩ�ޱ�

 ��һ�ױ����Ը���shiro��ʵ�ֲ�ͬ�û����Բ�ͬģ�����Ȩ�޵Ŀ���

2.�û���     �û��û����ϵ��   �û����  �û����ɫ��ϵ�� �û���

��һ�ױ����Ը����Զ���ע�⣬ʵ�ֲ�ͬ�û�����ҵ��㲻ͬ������Ȩ�޿���

2.��Ŀʵ�ֵĹ��ܣ�

       2.1  ��¼Ȩ�޿���

 �������ļ��ܣ��û���Ϣ��¼��֤�����ύ֮�󣬸����û�Ψһ��ʾ����ȡ�û���Ϣ�����û����ͱ���������ƴ�ӣ���������+�������Ͻ���MD5���ܣ��Ƚ����ݿ��б�������룬�Ƿ�͵õ��ļ�������һ�£�

         �������Դ��������ƣ�shiro�������֤�������MyShiroRealm�����doGetAuthenticationInfo()��ȡ���ݿ����û���Ϣ���������������ִ�д����ﵽĿ�ģ���Redis����洢�û����Դ�������¼�ɹ���0���ﵽ������ʧ�ܱ���

         ��֤��У�飺����GIF��֤�������ɶ�̬��֤�룬�����ڵ�ǰ�Ựsession�У���¼ʱ���ڵ�¼������У����֤��ͨ����Żᣬ����shiro��subject.login;

         ��ס�ҵĹ��ܣ�����һ��nameΪrememberMe��cookie���ͻ���;����shiro��remeberMe������;

      2.2Ȩ�޿��ƣ�

2.2.1. ��̬Ȩ�޿��ƺ�Redis����Ȩ��   ��Ȩ�����ݴ�������ݿ��У�shiro���ü���Ȩ�����ݵ�ʱ�򣬴����ݿ��ж�ȡ��ǰ̨����Ȩ�޸���֮������ShiroFilterFactoryBeanȥ������ص����ã��������������ӷ���Ȩ��;�ڽ��з���Ȩ���жϵ�ʱ�򣬻�ȡȨ�����ݿ��Դ�redis�����ȡȨ�����ݣ���ΪȨ�����ݲ������Ķ���������Ȩ�ޣ����ٺ����ݿ�Ľ���;

2.2.2�Զ���������                               

   shiro֧���Զ��������������AccessControllerFilter,��д����ķ�����һ��Ȩ���жϣ������Ƿ������Ȩ�޼������ʣ�һ�����÷��ʾܾ����Ƿ������������������; isAccessAllowed;һ��onAccessDeneied;

 

b.1.1�����û��������������߳��û�����cacheManager���棬ά��һ��cache,����ÿ���û���Ӧ��sessionId����;û�м��룬���жϸ����Ƿ�ﵽ���ޣ��ﵽ��־ ��ǰ�ػ���session�����Ƿ��߳�״̬�������������ͷ��ز�ͬ�����ajax���󷵻ض�Ӧ��json����ҳ������ֱ�Ӵﵽû��Ȩ��ҳ������ر����õ��߳�ҳ�棻

b.1.2 �Զ����ɫ��Ȩ���������޶���ͬ���͵�URL������Ҫ��ЩȨ�޲�����Ȩ�޿��Է��ʣ�

1.���������ݿ���ά��һ���ʼ��Ȩ�����ݣ� ���ݿ������ݸ�ʽ��

/system/manage/**=perms["user:sysConfig"]

/org/manage/**=perms["user:organization"]

/contract/manage/**=perms["user:contract"]

/salary/manage/**=perms["user:salary"]

 

      2.2.3��Ӧ���������ص�shiro��filterChainDefinitionMap��

      2.2.4 ��д��isAccessAllowed�����ж�subject���Ƿ�ӵ��Ȩ�޿��Է��ʣ�

              2.2.5.��д  onAccessDenied���������ʱ��ܾ�֮����δ���ǰ���󣬷���json����ʾ��������תû�з���Ȩ��ҳ�棻

        2.2.6  ʹ���Զ���ע�⣬��service�����뷽������Ȩ�޵��жϣ�ͨ��aop����+�����õ��������Ƿ�����֤Ȩ�޵ı�ʶ���Ӷ��жϷ����Ƿ���Ҫ��Ȩ

      2.2.7: ��������ɾ�Ĳ飬�Լ��໥֮���ϵ���ά��


3. ��Ҫ��λ��æ�ƽ�����

 springBoot+shiro  ʹ��shiroע����Ч������˵��ȫ��Ч��

        ����ע����������£�

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




�ҵ�application.properties  AOP ����:  spring.aop.proxy-target-class=true��

�ҵ�ҵ���service û�в��ýӿڣ�ȫ����ֱ��ʵ���ࣻ

��ʱ����1���ҵ�Controller�����淽����  shiroע����Ч������service�����淽����  shiroע����Ч���Զ���ע��aop����ʧ�ܣ�


���ҽ���

@Bean

@ConditionalOnMissingBean

@DependsOn("lifecycleBeanPostProcessor")

    public DefaultAdvisorAutoProxyCreator defaultAdvisorAutoProxyCreator() {

        DefaultAdvisorAutoProxyCreator daap = new DefaultAdvisorAutoProxyCreator();

        daap.setProxyTargetClass(true);

        return daap;

    }

        �ⲿ������ȥ��֮���Զ���ע��AOP����ɹ���Controller�෽����shiroע����Ч��Service�㷽����shiroע����Ȼ��Ч��


       �����ж���������ڣ�DefaultAdvisorAutoProxyCreator     һ����ʹ����������ã�controllerע���service bean  ����һ��aop���� Ҳ����cglib����  ����jdk����

ȥ���������֮��controllerע��service  ��һ��aop���� ��һ��cglib����  ����jdk���� ;

       �������⣺service �㷽����shiroע����Ч ���ƽ⣡����



 ��Դ����·�� ��Ҫ����redis  ���ݿ�����ö���application.properties���� 

  http://download.csdn.NET/detail/u012581573/9805572