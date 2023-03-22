### CORS ?

---

- CORS : Cross - Origin Resource Sharing 의 약자로, 웹 브라우저에서 실행되는 스크립트에서 다른 출처 (도메인, 프로토콜, 포트) 의 자원에 접근할 수 있도록 허용하는 메커니즘
- 기본적으로 웹 브라우저에서는 보안상의 이유로 다른 출처의 자원에 대한 접근이 제한됩니다.
- 브라우저는 서버로부터 받은 응답의 헤더를 확인하고, 자원에 대한 접근을 허용할지 여부를 결정하게 됩니다.
- 헤더
    - Access-Control-Allow-Origin
        - 헤더는 다른 출처의 자원에 대한 접근을 허용하는 출처(도메인)을 설정합니다.
        - 이 헤더는 * 와 같이 모든 출처를 허용하는 경우와 특정 출처를 허용하는 경우 2가지로 나뉩니다.
    - Access-Control-Allow-Methods
        - 기본적으로 브라우저에서는 GET, POST 메서드만 허용하며, 이외에 메서드를 사용하려면 이 헤더에 명시해야 합니다.
    - Access-Control-Allow-Headers
        - 이 헤더는 클라이언트에서 요청하는 헤더를 허용하는지 여부를 설정합니다.
        - 이 헤더를 설정하지 않으면 기본적으로 브라우저에서는 Accept, Accept-Language, Content-Language, Content-Type 등 일부 헤더만 허용합니다.

```java
@Bean
    CorsConfigurationSource corsConfigurationSource() {
        CorsConfiguration configuration = new CorsConfiguration();
        configuration.setAllowedOrigins(Arrays.asList("*"));
        configuration.setAllowedMethods(Arrays.asList("GET", "POST", "PATCH", "DELETE"));
        configuration.setAllowedHeaders(Arrays.asList("*"));
        configuration.setExposedHeaders(Arrays.asList("Refresh", "Authorization"));
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", configuration);
        return source;
    }
```

```java
public class CorsConfig implements WebMvcConfigurer {
    
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        WebMvcConfigurer.super.addCorsMappings(registry);
    }
}
```

- addCorsMapipngs() 메소드를 오버라이드하여, CorsRegistry 객체를 사용하여 CORS 관련 헤더를 설정하고 있습니다.
    - allowOrigins(”*”) : 모든 출처를 허용합니다. 특정 출처를 허용하려면 해당 출처를 지정하면 됩니다.
    - allowedMethods(”GET”, “POST”, “PUT”, “DELETE”) : 허용되는 HTTP 메소드를 지정합니다.
    - allowedHeaders(”*”) : 모든 헤더를 허용합니다.
    - allowCredentials(true) : 자격증명을 허용합니다.
    - exposedHeaders(”Content-Disposition”) : 브라우저에서 접근 가능한 헤더를 지정합니다.
    - maxAge(3600) : 캐시 시간을 설정합니다.

### WebMvcConfigurer

---

- **`configureContentNegotiation()`** : 콘텐츠 협상 구성을 설정합니다.
- **`configureAsyncSupport()`** : 비동기 서블릿 요청 및 응답 구성을 설정합니다.
- **`configureDefaultServletHandling()`** : 기본 서블릿 핸들러를 구성합니다.
- **`addFormatters()`** : 포맷터를 추가합니다.
- **`addInterceptors()`** : 인터셉터를 추가합니다.
    - 중요
- **`addResourceHandlers()`** : 리소스 핸들러를 추가합니다.
- **`addViewControllers()`** : 뷰 컨트롤러를 추가합니다.
- **`configureViewResolvers()`** : 뷰 리졸버를 설정합니다.
- **`addArgumentResolvers()`** : 인자 리졸버를 추가합니다.
- **`addReturnValueHandlers()`** : 반환 값 핸들러를 추가합니다.
- **`configureMessageConverters()`** : 메시지 컨버터를 설정합니다.
- **`extendMessageConverters()`** : 메시지 컨버터를 확장합니다.
- **`configureHandlerExceptionResolvers()`** : 핸들러 예외 리졸버를 설정합니다.
- **`extendHandlerExceptionResolvers()`** : 핸들러 예외 리졸버를 확장합니다.
- **`addCorsMappings()`** : CORS 관련 설정을 추가합니다.