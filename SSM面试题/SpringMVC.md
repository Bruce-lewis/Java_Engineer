Spring MVC 是一个基于 Java 的 Web 框架，属于 Spring 框架的一部分，主要用于构建基于 MVC（Model-View-Controller）设计模式的 Web 应用程序。以下是一些重要概念和高频面试题：

### 重要概念

1. **DispatcherServlet**：
   - Spring MVC 的核心组件，负责请求的分发和处理。

2. **Controller**：
   - 处理用户请求的组件，通常用 `@Controller` 注解标识。

3. **ModelAndView**：
   - 封装了模型数据和视图信息的对象。

4. **View Resolver**：
   - 负责将逻辑视图名解析为实际的视图（如 JSP 文件）。

5. **Handler Mapping**：
   - 将请求 URL 映射到相应的处理器（Controller）。

6. **Request Mapping**：
   - 使用 `@RequestMapping` 注解定义请求 URL 和处理方法的映射关系。

7. **Validation**：
   - Spring MVC 提供了数据校验的支持，可以使用 `@Valid` 注解进行参数验证。

8. **Interceptor**：
   - 用于在请求处理前后执行一些逻辑，比如日志记录、权限检查等。

9. **RESTful 风格**：
   - Spring MVC 支持 RESTful API 的开发，可以使用 `@RestController` 和 `@RequestMapping` 等注解。

10. **数据绑定**：
    - Spring MVC 支持将请求参数自动绑定到 Java 对象。

### 高频面试题

1. **Spring MVC 的工作流程是怎样的？**
   - 请求到达 `DispatcherServlet` → 通过 `Handler Mapping` 找到相应的 `Controller` → 执行 `Controller` 方法 → 返回 `ModelAndView` → 通过 `View Resolver` 解析视图 → 渲染视图并返回响应。

2. **什么是 `@Controller` 和 `@RestController` 的区别？**
   - `@Controller` 用于普通的控制器，返回视图；`@RestController` 是一个组合注解，包含 `@Controller` 和 `@ResponseBody`，用于返回 JSON 或 XML 数据。

3. **如何处理异常？**
   - 可以使用 `@ExceptionHandler` 注解来处理特定的异常，或者使用 `@ControllerAdvice` 来全局处理异常。

4. **如何实现请求参数的校验？**
   - 使用 `@Valid` 或 `@Validated` 注解结合 JSR-303/JSR-380 注解（如 `@NotNull`, `@Size` 等）进行参数校验。

5. **Spring MVC 中的拦截器是什么？**
   - 拦截器是用于在请求处理前后执行自定义逻辑的组件，可以用于日志记录、权限检查等。

6. **如何实现文件上传？**
   - 使用 `MultipartFile` 接收上传的文件，并通过 `@RequestParam` 注解进行绑定。

7. **Spring MVC 如何支持 RESTful API？**
   - 通过使用 `@RestController` 和 HTTP 方法（GET、POST、PUT、DELETE）结合 `@RequestMapping` 注解来实现。

8. **什么是视图解析器？如何配置？**
   - 视图解析器用于将逻辑视图名解析为具体的视图实现（如 JSP）。可以通过 XML 配置或 Java 配置类进行配置。

9. **Spring MVC 中的 `@RequestMapping` 如何使用？**
   - `@RequestMapping` 可以用于类和方法上，定义请求的 URL、HTTP 方法、请求参数等。

10. **如何实现国际化（i18n）？**
    - 通过配置 `MessageSource` 和使用 `@RequestMapping` 中的 `locale` 参数来支持国际化。

这些是 Spring MVC 的一些重要概念和高频面试题，掌握这些内容将有助于在面试中表现出色。