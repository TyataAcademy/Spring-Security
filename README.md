# Spring-Security
Spring Security with Spring MVC

Spring Security is one of the AOP feature.

## Setting up Spring Security:
- Maven dependence update in pom.xml for Spring Security jars (spring-security-web, spring-security-config) and AOP(spring-aop).

          <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-config</artifactId>
            <version>4.0.1.RELEASE</version>
          </dependency>
	  
	  <dependency>
		<groupId>org.springframework.security</groupId>	
		<artifactId>spring-security-web</artifactId>	
		<version>4.0.1.RELEASE</version>	
  	  </dependency>
	
	  <dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-aop</artifactId>
	    <version>4.2.5.RELEASE</version>
	  </dependency>        			


- create Spring Security Configuration class that inherites from WebSecurityConfigurerAdapter class.

 package com.tyataacademy.springmvc.security;

    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;


    @Configuration
    @EnableWebSecurity
    public class SecurityConfiguration extends WebSecurityConfigurerAdapter {

	@Autowired
	public void configureGlobalSecurity(AuthenticationManagerBuilder auth)
			throws Exception {
		auth.inMemoryAuthentication().withUser("tyata").password("tyata")
				.roles("USER", "ADMIN");
	}

	@Override
	protected void configure(HttpSecurity http) throws Exception {
		http.authorizeRequests().antMatchers("/login").permitAll()
				.antMatchers("/", "/*todo*/**").access("hasRole('USER')").and()
				.formLogin();
	}
    }



- update web.xml (dispatcher servlet) with filters. We configure this for application to be able to intercept every request before it enter the controller. Then Request first goes to Spring Security. If no user is logged in, then user will automatically sent to login page.

      <filter>
	  <filter-name>springSecurityFilterChain</filter-name>
	  <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
      </filter>
 
      <filter-mapping>
	  <filter-name>springSecurityFilterChain</filter-name>
	  <url-pattern>/*</url-pattern>
      </filter-mapping>     



## While Testing:
There are two things I would like to include here as Spring Security applies in our application which behaves differently and securely that it used to be with our Spring MVC app with simple log in page.
- 1. Spring Security Provides build in log in page where user will be able to input user id and password. 
- 2. Once user logged in successfully through log in page(provide by Spring Security) then use will see welcome page. Please note the url once get to welcome page. If user completely close the browser then again try the saved url on new page user will be redirected to the log in page.
- 3. Once user logged in successfully through log in page(provide by Spring Security) then use will see welcome page. Please note the url once get to welcome page. If the session time out happened and try the same saved url on browser then spring security feature will redirect it to log in page.

