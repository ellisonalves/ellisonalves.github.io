---
layout: post
title: Disabling TRACE request with spring
date: 2023-01-01 22:01
tags: [Spring Boot]
categories: [Spring]
mermaid: true
---

# Introduction
Recently I got a request for disabling TRACE method calls in the application for security reasons because an attacker could have access to some information that would allow them to find a breach.

# Solution
Considering the application is using spring boot, there are some steps to follow.

## Change application.properties
The following line should be added in the `application.properties` file:

```
spring.mvc.dispatch-trace-request=true
```

This change is required to tell spring that the TRACE requests should go now through the FrameworkServlet doService method just as other methods. So we will be able to intercept it.

## Configuration Spring MVC
In order to configure Spring MVC, we just need to create a configuration class extending `WebMvcConfigurer`. Here you can see an example in kotlin.

```kotlin
@Configuration
class WebConfig : WebMvcConfigurer {

    override fun addInterceptors(registry: InterceptorRegistry) {
        registry.addInterceptor(TraceMethodInterceptor()).addPathPatterns("/**")
    }

    private class TraceMethodInterceptor : HandlerInterceptor {
        override fun preHandle(
            request: HttpServletRequest,
            response: HttpServletResponse,
            handler: Any,
        ): Boolean {
            if (HttpMethod.TRACE.matches(request.method)) {
                response.sendError(HttpServletResponse.SC_METHOD_NOT_ALLOWED)
                return false
            }
            return true
        }
    }
}
```

In the example we are doing the following:
1. Creating a new interceptor called `TraceMethodInterceptor` which will always responds as METHOD_NOT_ALLOWED when any TRACE request is executed againist our application
2. Adding the interceptor in spring
3. Adding the pattern that will be intercepted with `addPathPatterns("/**")`

# Conclusion
That's should be enough for disabling incoming TRACE requests in our application. I also created a repository in [github](https://github.com/ellisonalves/my-spring-things/tree/master/disable-http-trace) so you can have all the details of the application such as tests for verifying that the implementation works.

Hope it helps! :)
