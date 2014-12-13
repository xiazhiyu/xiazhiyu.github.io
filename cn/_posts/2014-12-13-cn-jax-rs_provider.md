---
layout: post_cn
title: "JAX-RS中的提供器"
date: 2014-12-13 23:32:36
categories: 开发
tag: JAVA JAX-RS
---

关于JAX-RS的中文资料似乎很少。
这实际上是去年，前年左右学习过的，做一下整理。

提供器（Provider）有点类似于拦截器的概念。当服务器端接到一个request请求时，会在接收到request和response是触发事件。

首先创建类，继承ContainerRequestFilter,ContainerResponseFilter。

  {% highlight java %}
@LogRecorder
@Provider
public class LogRecorderInterceptor implements ContainerRequestFilter,ContainerResponseFilter{

    @Override
    public void filter(ContainerRequestContext requestContext) throws IOException {
      System.out.println("接受request");  
    }

    @Override
    public void filter(ContainerRequestContext requestContext, ContainerResponseContext responseContext) {
        System.out.println("发出response");      
    }
}
  {% endhighlight %}

使用@NameBinding为该类创建一个注解接口LogRecorder

  {% highlight java %}
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import javax.ws.rs.NameBinding;

/**
 *
 * @author xiazhiyu
 */
@NameBinding
@Target({ ElementType.TYPE, ElementType.METHOD })
@Retention(value = RetentionPolicy.RUNTIME)
public @interface LogRecorder {
    
}
  {% endhighlight %}

对resource类或者方法进行注解
  
  {% highlight java %}
  @LogRecorder
  @POST
  @Path("auth")
  @Consumes(MediaType.APPLICATION_JSON)
  @Produces(MediaType.APPLICATION_JSON)
  public Response loginREST(User user) {
    /*
    login logic
    */
  }
  {% endhighlight %}

在jax-rs 2.0 中，你还可以通过接口DynamicFeature按照package进行配置。
  
  {% highlight java %}
@Provider
public class MyDynamicFeature implements DynamicFeature {

    @Override
    public void configure(final ResourceInfo resourceInfo,
                          final FeatureContext context) {

        final String resourcePackage = resourceInfo.getResourceClass()
                .getPackage().getName();
        final Method resourceMethod = resourceInfo.getResourceMethod();

        if ("my.package.admin".equals(resourcePackage)
                && resourceMethod.getAnnotation(GET.class) != null) {
            context.register(LogRecorderInterceptor.class);
        }
    }
}
  {% endhighlight %}

在这种情况下所有在my.package.admin包中类的方法，都会执行LogRecorderInterceptor中的filter方法。
  
