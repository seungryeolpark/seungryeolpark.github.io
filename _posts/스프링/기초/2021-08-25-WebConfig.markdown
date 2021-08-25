---
title: WebConfig
categories: springBase
---

### WebConfig
**예시**
```java
@Configuration
@RequiredArgsConstructor
public class WebConfig implements WebMvcConfigurer {

    @Value("${file.save}")
    private String uploadImagePath;

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {

        registry.addResourceHandler("/js/**")
                .addResourceLocations("classpath:/static/js/");
        registry.addResourceHandler("/css/**")
                .addResourceLocations("classpath:/static/css/");
        registry.addResourceHandler("/img/**")
                .addResourceLocations("classpath:/static/img/");
        registry.addResourceHandler("/fonts/**")
                .addResourceLocations("classpath:/static/fonts/");
        registry.addResourceHandler("/data/**")
                .addResourceLocations("classpath:/static/data/");
        registry.addResourceHandler("/")
                .addResourceLocations("classpath:/static/index.html");

        // 업로드 이미지용 외부 폴더 추가
        registry.addResourceHandler("/upload/**")
                .addResourceLocations("file:///" + uploadImagePath)
                .setCachePeriod(3600)
                .resourceChain(true)
                .addResolver(new PathResourceResolver())
                .addResolver(new GzipResourceResolver());
    }
```

+ setCachePeriod() : 캐쉬 유효 기간을 초 단위로 지정한다.
+ resourceChain() : 캐쉬 사용 유무
+ PathResourceResolver() : 요청 경로와 일치하는 주어진 위치에서 리소스를 찾으려고 시도하는 간단한 ResourceResolver 입니다.
+ GzipResourceResolver() : 요청 경로와 일치하는 주어진 위치에서 리소스를 gzip으로 압축하여 보내주는 ResourceResolver 입니다.
