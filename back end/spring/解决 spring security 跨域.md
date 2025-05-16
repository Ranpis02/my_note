```java
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                 .anyRequest()
                 .permitAll()
                 .and()
                 .formLogin()
                 .permitAll()
                 .and()
                 .httpBasic()
                 .and()
                 //支持跨域访问
                 .cors()
                 .and()
                 .csrf()
                 .disable();
       }
}
```

