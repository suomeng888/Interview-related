#                    Spring拦截器整理

##     拦截器解读

​	依赖于web框架，在SpringMVC中就是依赖于SpringMVC框架。在实现上基于Java的反射机制，属于面向切面编程（AOP）的一种运用。由于拦截器是基于web框架的调用，因此可以使用Spring的依赖注入（DI）进行一些业务操作，同时一个拦截器实例在一个controller生命周期之内可以多次调用。但是缺点是只能对controller请求进行拦截，对其他的一些比如直接访问静态资源的请求则没办法进行拦截处理。它提供了一种机制可以使开发者在一个Action执行的前后执行一段代码。

   ##     code

Java代码：

`public class HandlerInterceptorAdapter implements HandlerInterceptor {   
 public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) 
​            throws Exception{
​    }   
​    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView)
​            throws Exception{
​    }   
​    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)
​            throws Exception{
​    }
}` 

xml配置：

` <!--配置拦截器, 多个拦截器,顺序执行 -->
​    <mvc:interceptors>
​        <mvc:interceptor>
​            <!-- 匹配的是url路径， 如果不配置或/**,将拦截所有的Controller -->
​            <mvc:mapping path="/ps/**" />
​            <!--<mvc:mapping path="/user/**" />-->
​            <!--<mvc:mapping path="/test/**" />-->
​            <bean class="com.weixin.InterceptorAdapter.HandlerInterceptorAdapter"></bean>
​        </mvc:interceptor>
​        <!-- 当设置多个拦截器时，先按顺序调用preHandle方法，然后逆序调用每个拦截器的postHandle和afterCompletion方法 -->
​    </mvc:interceptors>`

用法：

`//继承spring的拦截接口,实现以下三个方法
public interface HandlerInterceptor {
​    // 在业务处理器处理请求之前被调用
​    boolean preHandle(HttpServletRequest var1, HttpServletResponse var2, Object var3) throws Exception;
​    // 在业务处理器处理请求完成之后，生成视图之前执行
​    void postHandle(HttpServletRequest var1, HttpServletResponse var2, Object var3, ModelAndView var4) throws Exception;
​    // 在DispatcherServlet完全处理完请求之后被调用，可用于清理资源,日志打印
​    void afterCompletion(HttpServletRequest var1, HttpServletResponse var2, Object var3, Exception var4) throws Exception;
}`

## 与过滤器的区别：

​	过滤器依赖于servlet容器。在实现上基于函数回调，可以对几乎所有请求进行过滤，但是缺点是*一个过滤器实例只能在容器初始化时调用一次*。使用过滤器的目的是用来做一些过滤操作，获取我们想要获取的数据，比如：在过滤器中修改字符编码；在过滤器中修改HttpServletRequest的一些参数，包括：过滤低俗文字、危险字符等。

## 多个过滤器与拦截器的执行顺序：

 ![执行顺序图](E:\Interview-related\document\执行顺序.png)