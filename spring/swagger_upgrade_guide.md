# Swagger2와 Swagger3 버전 업그레이드
 
## annotaion 변경 
* `@ApiParam` -> `@Parameter` 
* `@ApiOperation` -> `@Operation` 
* `@Api` -> `@Tag` 
* `@ApiImplicitParams` -> `@Parameters` 
* `@ApiImplicitParam` -> `@Parameter` 
* `@ApiIgnore` -> `@Parameter(hidden = true) or @Operation(hidden = true) or @Hidden` 
* `@ApiModel` -> `@Schema` 
* `@ApiModelProperty` -> `@Schema` 
 
## SwaggerConfiguration 변경 
###  multiple Docket 사용한 경우 
* Before: multiple Docket beans  
```java 
   @Bean 
    public Docket publicApi() { 
        return new Docket(DocumentationType.SWAGGER_2) 
                .select() 
                .apis(RequestHandlerSelectors.basePackage("org.github.springshop.web.public")) 
                .paths(PathSelectors.regex("/public.*")) 
                .build() 
                .groupName("springshop-public") 
                .apiInfo(apiInfo()); 
    } 
 
    @Bean 
    public Docket adminApi() { 
        return new Docket(DocumentationType.SWAGGER_2) 
                .select() 
                .apis(RequestHandlerSelectors.basePackage("org.github.springshop.web.admin")) 
                .paths(PathSelectors.regex("/admin.*")) 
                .build() 
                .groupName("springshop-admin") 
                .apiInfo(apiInfo()); 
    } 
``` 
* After: GroupedOpenApi 
```java 
 @Bean 
    public GroupedOpenApi publicApi() { 
        return GroupedOpenApi.builder() 
                .setGroup("springshop-public") 
                .pathsToMatch("/public/**") 
                .build(); 
    } 
 
    @Bean 
    public GroupedOpenApi adminApi() { 
        return GroupedOpenApi.builder() 
                .setGroup("springshop-admin") 
                .pathsToMatch("/admin/**") 
                .build(); 
    } 
``` 
 
### Docket 한개만 쓴 경우 
* remove application.properties configuration 
```yaml 
# (in application.properties) 
springdoc.packagesToScan=package1, package2 
springdoc.pathsToMatch=/v1, /api/balance/** 
``` 
* Add bean of `OpenAPI` type 
```java 
@Bean 
    public OpenAPI springShopOpenAPI() { 
        return new OpenAPI() 
                .info(new Info().title("SpringShop API") 
                .description("Spring shop sample application") 
                .version("v0.0.1") 
                .license(new License().name("Apache 2.0").url("http://springdoc.org"))) 
                .externalDocs(new ExternalDocumentation() 
                .description("SpringShop Wiki Documentation") 
                .url("https://springshop.wiki.github.org/docs")); 
    } 
``` 
 
## Reference 
* [migrating-from-springfox-swagger2-to-springdoc-openapi](https://stackoverflow.com/questions/59291371/migrating-from-springfox-swagger2-to-springdoc-openapi) 
* [A Visual Guide to What's New in Swagger 3.0](https://blog.readme.com/an-example-filled-guide-to-swagger-3-2/)