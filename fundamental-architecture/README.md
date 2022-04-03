## High Level

Spring Security's Servlet support is based on Servlet Filter. In a Spring MVC application the Servlet is an instance of `DispatcherServlet`. At most one Servlet can handle a single HttpServletRequest and HttpServletResponse. However, more than one Filter can be used to:  
- Prevent downstream Filters or the Servlet from being invoked. In this instance the Filter will typically write the HttpServletResponse.  
- Modify the HttpServletRequest or HttpServletResponse used by the downstream Filters and Servlet.

Spring provides a Filter implementation named `DelegatingFilterProxy` that allows _bridging_ between the Servlet container's lifecycle and Spring's ApplicationContext.  
The Servlet container not aware about Spring defined Beans. So, `DelegatingFilterProxy` register the filters via standard Servlet container mechanisms, but delegate all the work to a Spring Bean that implements Filter.

Spring Security's Servlet support is contained within `FilterChainProxy`. `FilterChainProxy` is a special Filter provided by Spring Security that allows delegating to many Filter instances through `SecurityFilterChain`. Since `FilterChainProxy` is a Bean, it is typically wrapped in a `DelegatingFilterProxy`.

![security filter chain](https://github.com/bluething/learn-springsecurity/blob/main/images/securityfilterchain.png?raw=true)

Why Security Filters registered with `FilterChainProxy` instead of `DelegatingFilterProxy`?  
1. If we want to troubleshoot Spring Security support just add a breakpoint in `FilterChainProxy`.  
2. Because FilterChainProxy is central to Spring Security usage it can perform tasks that are not viewed as optional.  
   - Clears out the SecurityContext to avoid memory leaks.  
   - Protect applications against certain types of attacks with Spring Security's `HttpFirewall`.  
3. More flexible. In a Servlet container, Filters are invoked based upon the URL alone. However, FilterChainProxy can determine invocation based upon anything in the HttpServletRequest by leveraging the RequestMatcher interface.

## Authentication

## Authorization

## References

[Spring Security Architecture](https://spring.io/guides/topicals/spring-security-architecture)  
[Architecture](https://docs.spring.io/spring-security/reference/servlet/architecture.html)  
[Servlet Authentication Architecture](https://docs.spring.io/spring-security/reference/servlet/authentication/architecture.html)  
[Authorization Architecture](https://docs.spring.io/spring-security/reference/servlet/authorization/architecture.html)