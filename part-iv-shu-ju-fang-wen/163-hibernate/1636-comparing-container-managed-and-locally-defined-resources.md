### 16.3.6 Comparing container-managed and locally defined resources

You can switch between a container-managed JNDI`SessionFactory`and a locally defined one, without having to change a single line of application code. Whether to keep resource definitions in the container or locally within the application is mainly a matter of the transaction strategy that you use. Compared to a Spring-defined local`SessionFactory`, a manually registered JNDI`SessionFactory`does not provide any benefits. Deploying a`SessionFactory`through Hibernate’s JCA connector provides the added value of participating in the Java EE server’s management infrastructure, but does not add actual value beyond that.

Spring’s transaction support is not bound to a container. Configured with any strategy other than JTA, transaction support also works in a stand-alone or test environment. Especially in the typical case of single-database transactions, Spring’s single-resource local transaction support is a lightweight and powerful alternative to JTA. When you use local EJB stateless session beans to drive transactions, you depend both on an EJB container and JTA, even if you access only a single database, and only use stateless session beans to provide declarative transactions through container-managed transactions. Also, direct use of JTA programmatically requires a Java EE environment as well. JTA does not involve only container dependencies in terms of JTA itself and of JNDI`DataSource`instances. For non-Spring, JTA-driven Hibernate transactions, you have to use the Hibernate JCA connector, or extra Hibernate transaction code with the`TransactionManagerLookup`configured for proper JVM-level caching.

Spring-driven transactions can work as well with a locally defined Hibernate`SessionFactory`as they do with a local JDBC`DataSource`if they are accessing a single database. Thus you only have to use Spring’s JTA transaction strategy when you have distributed transaction requirements. A JCA connector requires container-specific deployment steps, and obviously JCA support in the first place. This configuration requires more work than deploying a simple web application with local resource definitions and Spring-driven transactions. Also, you often need the Enterprise Edition of your container if you are using, for example, WebLogic Express, which does not provide JCA. A Spring application with local resources and transactions spanning one single database works in any Java EE web container \(without JTA, JCA, or EJB\) such as Tomcat, Resin, or even plain Jetty. Additionally, you can easily reuse such a middle tier in desktop applications or test suites.

All things considered, if you do not use EJBs, stick with local`SessionFactory`setup and Spring’s`HibernateTransactionManager`or`JtaTransactionManager`. You get all of the benefits, including proper transactional JVM-level caching and distributed transactions, without the inconvenience of container deployment. JNDI registration of a Hibernate`SessionFactory`through the JCA connector only adds value when used in conjunction with EJBs.
