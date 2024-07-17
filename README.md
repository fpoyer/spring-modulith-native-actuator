# spring-modulith-native-actuator

I created this project as a minimal self-contained example of a bug with the current versions of spring-native and spring-modulith-actuator

## Reproduce the issue

Run `git clone https://github.com/fpoyer/spring-modulith-native-actuator.git && cd spring-modulith-native-actuator` to clone the project  
Run `./mvnw ./mvnw -Pnative clean spring-boot:build-image` to compile native image  
Run `docker run --rm spring-native-actuator:0.0.1-SNAPSHOT` to run the image  

## [Edit : Solution]
The problem is caused by spring-modulith-observability, which uses archunit that is not (yet) graalvm ready.  
Removing it from the classpath, by changing the dependency from spring-modulith-starter-insight to spring-modulith-actuator solves the situation and the application starts with no error

## Expected
The Application starts with no error

## Actual
Application starts with the following error :
```
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/

 :: Spring Boot ::                (v3.3.1)

2024-07-16T13:37:52.839Z  INFO 1 --- [spring-native-actuator] [           main] [                                                 ] c.e.s.SpringNativeActuatorApplication    : Starting AOT-processed SpringNativeActuatorApplication using Java 21.0.3 with PID 1 (/workspace/com.example.spring_native_actuator.SpringNativeActuatorApplication started by cnb in /workspace)
2024-07-16T13:37:52.839Z  INFO 1 --- [spring-native-actuator] [           main] [                                                 ] c.e.s.SpringNativeActuatorApplication    : No active profile set, falling back to 1 default profile: "default"
2024-07-16T13:37:52.842Z  INFO 1 --- [spring-native-actuator] [cTaskExecutor-1] [                                                 ] com.tngtech.archunit.core.PluginLoader   : Detected Java version 21.0.3
2024-07-16T13:37:52.842Z  WARN 1 --- [spring-native-actuator] [           main] [                                                 ] w.s.c.ServletWebServerApplicationContext : Exception encountered during context initialization - cancelling refresh attempt: org.springframework.context.ApplicationContextException: Unable to start web server
Application run failed
org.springframework.context.ApplicationContextException: Unable to start web server
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.onRefresh(ServletWebServerApplicationContext.java:165)
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:618)
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.refresh(ServletWebServerApplicationContext.java:146)
	at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:754)
	at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:456)
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:335)
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1363)
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1352)
	at com.example.spring_native_actuator.SpringNativeActuatorApplication.main(SpringNativeActuatorApplication.java:10)
	at java.base@21.0.3/java.lang.invoke.LambdaForm$DMH/sa346b79c.invokeStaticInit(LambdaForm$DMH)
Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'org.springframework.boot.autoconfigure.web.servlet.ServletWebServerFactoryConfiguration$EmbeddedTomcat': java.lang.ExceptionInInitializerError
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:607)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:522)
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:337)
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:234)
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:335)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:225)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveNamedBean(DefaultListableBeanFactory.java:1323)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveNamedBean(DefaultListableBeanFactory.java:1284)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveBean(DefaultListableBeanFactory.java:486)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.getBean(DefaultListableBeanFactory.java:341)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.getBean(DefaultListableBeanFactory.java:334)
	at org.springframework.boot.autoconfigure.web.servlet.ServletWebServerFactoryConfiguration__BeanDefinitions$EmbeddedTomcat.lambda$getTomcatServletWebServerFactoryInstanceSupplier$0(ServletWebServerFactoryConfiguration__BeanDefinitions.java:35)
	at org.springframework.util.function.ThrowingBiFunction.apply(ThrowingBiFunction.java:68)
	at org.springframework.util.function.ThrowingBiFunction.apply(ThrowingBiFunction.java:54)
	at org.springframework.beans.factory.aot.BeanInstanceSupplier.lambda$get$2(BeanInstanceSupplier.java:206)
	at org.springframework.util.function.ThrowingSupplier.get(ThrowingSupplier.java:58)
	at org.springframework.util.function.ThrowingSupplier.get(ThrowingSupplier.java:46)
	at org.springframework.beans.factory.aot.BeanInstanceSupplier.invokeBeanSupplier(BeanInstanceSupplier.java:218)
	at org.springframework.beans.factory.aot.BeanInstanceSupplier.get(BeanInstanceSupplier.java:206)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.obtainInstanceFromSupplier(DefaultListableBeanFactory.java:949)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.obtainFromSupplier(AbstractAutowireCapableBeanFactory.java:1219)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1162)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:562)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:522)
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:337)
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:234)
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:335)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:205)
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.getWebServerFactory(ServletWebServerApplicationContext.java:223)
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.createWebServer(ServletWebServerApplicationContext.java:186)
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.onRefresh(ServletWebServerApplicationContext.java:162)
	... 9 more
Caused by: java.lang.RuntimeException: java.lang.ExceptionInInitializerError
	at org.springframework.util.function.ThrowingSupplier.get(ThrowingSupplier.java:64)
	at org.springframework.util.function.ThrowingSupplier.get(ThrowingSupplier.java:46)
	at org.springframework.util.function.SingletonSupplier.get(SingletonSupplier.java:106)
	at org.springframework.modulith.runtime.ApplicationModulesRuntime.isApplicationClass(ApplicationModulesRuntime.java:69)
	at org.springframework.modulith.observability.ModuleTracingBeanPostProcessor.postProcessAfterInitialization(ModuleTracingBeanPostProcessor.java:82)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.applyBeanPostProcessorsAfterInitialization(AbstractAutowireCapableBeanFactory.java:438)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1791)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:600)
	... 39 more
Caused by: java.util.concurrent.ExecutionException: java.lang.ExceptionInInitializerError
	at java.base@21.0.3/java.util.concurrent.FutureTask.report(FutureTask.java:122)
	at java.base@21.0.3/java.util.concurrent.FutureTask.get(FutureTask.java:191)
	at org.springframework.modulith.runtime.autoconfigure.SpringModulithRuntimeAutoConfiguration.lambda$modulesRuntime$1(SpringModulithRuntimeAutoConfiguration.java:71)
	at org.springframework.util.function.ThrowingSupplier.get(ThrowingSupplier.java:58)
	... 46 more
Caused by: java.lang.ExceptionInInitializerError
	at com.tngtech.archunit.core.importer.ClassFileImporter.lambda$importPackages$1(ClassFileImporter.java:212)
	at java.base@21.0.3/java.util.stream.ReferencePipeline$7$1.accept(ReferencePipeline.java:273)
	at java.base@21.0.3/java.util.AbstractList$RandomAccessSpliterator.forEachRemaining(AbstractList.java:722)
	at java.base@21.0.3/java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:509)
	at java.base@21.0.3/java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:499)
	at java.base@21.0.3/java.util.stream.ReduceOps$ReduceOp.evaluateSequential(ReduceOps.java:921)
	at java.base@21.0.3/java.util.stream.AbstractPipeline.evaluate(AbstractPipeline.java:234)
	at java.base@21.0.3/java.util.stream.ReferencePipeline.collect(ReferencePipeline.java:682)
	at com.tngtech.archunit.core.importer.ClassFileImporter.importPackages(ClassFileImporter.java:213)
	at org.springframework.modulith.core.ApplicationModules.<init>(ApplicationModules.java:131)
	at org.springframework.modulith.core.ApplicationModules.<init>(ApplicationModules.java:104)
	at org.springframework.modulith.core.ApplicationModules.lambda$of$17(ApplicationModules.java:607)
	at java.base@21.0.3/java.util.concurrent.ConcurrentHashMap.computeIfAbsent(ConcurrentHashMap.java:1708)
	at org.springframework.modulith.core.ApplicationModules.of(ApplicationModules.java:603)
	at org.springframework.modulith.core.ApplicationModules.of(ApplicationModules.java:223)
	at org.springframework.modulith.core.ApplicationModules.of(ApplicationModules.java:205)
	at org.springframework.modulith.runtime.autoconfigure.SpringModulithRuntimeAutoConfiguration$ApplicationModulesBootstrap.initializeApplicationModules(SpringModulithRuntimeAutoConfiguration.java:157)
	at org.springframework.modulith.runtime.autoconfigure.SpringModulithRuntimeAutoConfiguration.lambda$modulesRuntime$0(SpringModulithRuntimeAutoConfiguration.java:70)
	at java.base@21.0.3/java.util.concurrent.FutureTask.run(FutureTask.java:317)
	at java.base@21.0.3/java.lang.Thread.runWith(Thread.java:1596)
	at java.base@21.0.3/java.lang.Thread.run(Thread.java:1583)
	at org.graalvm.nativeimage.builder/com.oracle.svm.core.thread.PlatformThreads.threadStartRoutine(PlatformThreads.java:902)
	at org.graalvm.nativeimage.builder/com.oracle.svm.core.thread.PlatformThreads.threadStartRoutine(PlatformThreads.java:878)
Caused by: com.tngtech.archunit.core.PluginLoader$PluginLoadingFailedException: Couldn't load plugin of type com.tngtech.archunit.core.importer.ModuleImportPlugin
	at com.tngtech.archunit.core.PluginLoader$PluginSupplier.create(PluginLoader.java:88)
	at com.tngtech.archunit.core.PluginLoader$PluginSupplier.get(PluginLoader.java:73)
	at com.tngtech.archunit.thirdparty.com.google.common.base.Suppliers$NonSerializableMemoizingSupplier.get(Suppliers.java:186)
	at com.tngtech.archunit.core.PluginLoader.load(PluginLoader.java:50)
	at com.tngtech.archunit.core.importer.ImportPlugin$Loader.loadForCurrentPlatform(ImportPlugin.java:40)
	at com.tngtech.archunit.core.importer.Locations.<clinit>(Locations.java:43)
	... 23 more
Caused by: java.lang.ClassNotFoundException: com.tngtech.archunit.core.importer.ModuleImportPlugin
	at org.graalvm.nativeimage.builder/com.oracle.svm.core.hub.ClassForNameSupport.forName(ClassForNameSupport.java:122)
	at org.graalvm.nativeimage.builder/com.oracle.svm.core.hub.ClassForNameSupport.forName(ClassForNameSupport.java:86)
	at java.base@21.0.3/java.lang.Class.forName(DynamicHub.java:1359)
	at java.base@21.0.3/java.lang.Class.forName(DynamicHub.java:1322)
	at java.base@21.0.3/java.lang.Class.forName(DynamicHub.java:1315)
	at com.tngtech.archunit.core.PluginLoader$PluginSupplier.create(PluginLoader.java:84)
	... 28 more

```
