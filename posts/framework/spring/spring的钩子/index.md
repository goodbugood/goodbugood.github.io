# Spring 的钩子


# Spring 的钩子

Spring 有很多钩子，供我们扩展 Spring 的功能。

> Bean factory implementations should support the standard bean lifecycle interfaces as far as possible. The full set of initialization methods and their standard order is:
>
> BeanNameAware's setBeanName
> BeanClassLoaderAware's setBeanClassLoader
> BeanFactoryAware's setBeanFactory
> EnvironmentAware's setEnvironment
> EmbeddedValueResolverAware's setEmbeddedValueResolver
> ResourceLoaderAware's setResourceLoader (only applicable when running in an application context)
> ApplicationEventPublisherAware's setApplicationEventPublisher (only applicable when running in an application context)
> MessageSourceAware's setMessageSource (only applicable when running in an application context)
> ApplicationContextAware's setApplicationContext (only applicable when running in an application context)
> ServletContextAware's setServletContext (only applicable when running in a web application context)
> postProcessBeforeInitialization methods of BeanPostProcessors
> InitializingBean's afterPropertiesSet
> a custom init-method definition
> postProcessAfterInitialization methods of BeanPostProcessors
> On shutdown of a bean factory, the following lifecycle methods apply:
>
> postProcessBeforeDestruction methods of DestructionAwareBeanPostProcessors
> DisposableBean's destroy
> a custom destroy-method definition

## 参考

- [Interface BeanFactory](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/BeanFactory.html)
