# Create Your Camel RestDSL Route

## Download the project with Git
Go to a terminal that has git available and do the following:
```
  $ git clone <TODO insert url here>
```
Using your favorite editor open up <TODO file path to Application file or Route file>
  
## Review the Code
Take note of the Application class and the ServletRegistrationBean.  This is necessary to use the Camel Servlet component to write your APIs using the Camel RestDSL. 
```java
    @Bean
    public ServletRegistrationBean camelServletRegistrationBean() {
        ServletRegistrationBean registration = new ServletRegistrationBean(new CamelHttpTransportServlet(),"/camel/*");
        registration.setName("CamelServlet");
        return registration;
    }
```
  
## Write Your Camel Route
You will need to write your Camel route inside the configure() method.  The following route can be used, but you can also feel free to write your own.  Directions will go off of this route.
```java
        restConfiguration()
        	.component("servlet")
    		  .bindingMode(RestBindingMode.json);
          
        rest().get("/hello")
        	.route().routeId("get-hello")
    		  .to("direct:hello");
          
        from("direct:hello")
        	.routeId("log-hello")
        	.log(LoggingLevel.INFO, "Hello World, <Your Name>")
        	.transform().simple("Hello World, <Your Name>");
```

## Run and Test Your Camel Route Using Standalone Spring Boot
To initially test your Camel route you can run it using standalone spring-boot.  This will ensure everything compiles and that your Rest API is working as expected. To do this go to your terminal, browse to your project folder, and run the following:
```
mvn spring-boot:run
```
Leaving your route running in a separate terminal or using a browser try and hit your API.  If using a terminal run the following command:
```
curl http://localhost:8080/camel/hello
```
You should get a 200 OK response and a text response of "Hello World, <Your Name>"