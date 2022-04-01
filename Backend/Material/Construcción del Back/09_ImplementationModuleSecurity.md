# Implementación del modulo de seguridad en spring boot con JWT

### 1. Dependencias spring security. 

![image](https://user-images.githubusercontent.com/31961588/161341759-2af77e1a-454e-4768-9dec-4b48fa2497f6.png)

#### 1.2 pom.xml

```Xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.6.4</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.webservice.uts</groupId>
	<artifactId>api-invoice-uts</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>api-invoice-uts</name>
	<description>Demo project for Spring Boot</description>
	<properties>
		<java.version>11</java.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>org.postgresql</groupId>
			<artifactId>postgresql</artifactId>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
		<dependency> 
		    <groupId>org.springframework.boot</groupId> 
		    <artifactId>spring-boot-starter-validation</artifactId> 
        </dependency>
        
        <!-- https://mvnrepository.com/artifact/org.springframework.security.oauth/spring-security-oauth2 -->        
        <dependency>
			<groupId>org.springframework.security.oauth</groupId>
			<artifactId>spring-security-oauth2</artifactId>
			<version>2.3.4.RELEASE</version>
		</dependency>
			<!-- https://mvnrepository.com/artifact/org.springframework.security/spring-security-jwt -->
			<dependency>
			<groupId>org.springframework.security</groupId>
			<artifactId>spring-security-jwt</artifactId>
			<version>1.0.9.RELEASE</version>
		</dependency>
		
		<dependency>
			<groupId>javax.xml.bind</groupId>
			<artifactId>jaxb-api</artifactId>
		</dependency>
		
		<dependency>
			<groupId>org.glassfish.jaxb</groupId>
			<artifactId>jaxb-runtime</artifactId>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>

```

#### 1.3 Actualizar dependencias

![image](https://user-images.githubusercontent.com/31961588/161341932-7ab1e35c-4345-4509-ba73-f7cb335c8aab.png)


### 2. Interfaz IUserService

![image](https://user-images.githubusercontent.com/31961588/161342379-7e04c05f-e8ad-49ca-8c3f-cabb421d7c27.png)

#### 2.1 Código fuente

```Java
package com.webservice.uts.models.services;

import com.webservice.uts.models.entites.Usuario;

public interface IUsuarioService {
	
	public Usuario findByUsername(String username);
	
	public void delete(Usuario Usuario);

}

```

### 3 Dao IUsuarioDao

![image](https://user-images.githubusercontent.com/31961588/161343331-c6097685-5490-4074-8382-9eb7c1601c15.png)

#### 3.1 Código fuente

```Java
package com.webservice.uts.models.dao;



import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.CrudRepository;

import com.webservice.uts.models.entites.Usuario;


public interface IUsuarioDao extends CrudRepository<Usuario,Long> {
	
	public Usuario findByUsername(String username);
	
	@Query("select u from Usuario u where u.username=?1")	
	public Usuario findByUsername2(String username);

}

```


###  4 Implementación UsuarioServiceImp

![image](https://user-images.githubusercontent.com/31961588/161343512-e085a593-7d8c-408e-b2f2-1dab218e287e.png)

#### 4.1 Código fuente

```Java
package com.webservice.uts.models.services;

import java.util.List;
import java.util.stream.Collectors;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import com.webservice.uts.models.dao.IUsuarioDao;
import com.webservice.uts.models.entites.Cliente;
import com.webservice.uts.models.entites.Usuario;


@Service
public class UsuarioServiceImpl implements IUsuarioService, UserDetailsService {
	
	private Logger logger = LoggerFactory.getLogger(UsuarioServiceImpl.class);

	@Autowired
	private IUsuarioDao usuarioDao;
	
	@Override
	@Transactional(readOnly=true)
	public Usuario findByUsername(String username) {
		return usuarioDao.findByUsername(username);
	}
	@Override
	@Transactional
	public void delete(Usuario usuario) {
		usuarioDao.delete(usuario);
		
	}


	@Override
	@Transactional(readOnly=true)
	public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
		
		Usuario usuario=usuarioDao.findByUsername(username);
		
		if(usuario==null){
		  logger.error("Error en el login: no existe el usuario "+username+" en el sistema!");
		  throw new UsernameNotFoundException("Error en el login: no existe el usuario " +username+ " en el sistema!");
		}
		
		List<GrantedAuthority> authorities = usuario.getRoles()
				.stream()
				.map(role-> new SimpleGrantedAuthority(role.getNombre()))
				.peek(authority-> logger.info("Role: "+authority.getAuthority()))
				.collect(Collectors.toList());
		
		return new User(usuario.getUsername(),usuario.getPassword(),usuario.getEnabled(),true,true,true,authorities);
		
		
	}

}


```

### 5. Paquete auth

Creación de paquete que contiente las clases de autenticación y autorización con jwt

![image](https://user-images.githubusercontent.com/31961588/161344587-aca3fd0e-0eb5-4669-823c-88a10b269b20.png)


### 6. SpringSecurityConfig

Se crea la clase SpringSecurityConfig

![image](https://user-images.githubusercontent.com/31961588/161344773-fe8c33fc-d4c7-445e-af77-fd4796677478.png)


#### 6.1 Código 

```Java
package com.webservice.uts.auth;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;

@EnableGlobalMethodSecurity(securedEnabled=true)
@Configuration
public class SpringSecurityConfig extends WebSecurityConfigurerAdapter {
	
	@Autowired
	private UserDetailsService usuarioService;
	
	@Bean
	public BCryptPasswordEncoder  passwordEncoder(){
		return new BCryptPasswordEncoder();
	}

	@Override
	@Autowired 
	protected void configure(AuthenticationManagerBuilder auth) throws Exception {
		auth.userDetailsService(this.usuarioService).passwordEncoder(passwordEncoder());
	}

	@Bean("authenticationManager")	
	@Override
	public AuthenticationManager authenticationManagerBean() throws Exception {
		return super.authenticationManagerBean();
	}

	@Override
	protected void configure(HttpSecurity http) throws Exception {
		 http.authorizeRequests()
		.anyRequest().authenticated()
		.and()
		.csrf().disable()
		.sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);
	}
	
	
	

}


```

### 7. AuthorizationServerConfig

Antes de crea la clase de AuthorizationServerConfig vamos primero la clase Application del proyecto a crear un BCryptPasswordEncoder para encriptar el password

![image](https://user-images.githubusercontent.com/31961588/161345358-c6705274-fae3-4fb4-88b7-09f9cbe2e66d.png)

#### 7.1 Código ApiInvoiceUtsApplication

```Java
package com.webservice.uts;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;

@SpringBootApplication
public class ApiInvoiceUtsApplication implements CommandLineRunner {
	
	@Autowired
	private BCryptPasswordEncoder passwordEncoder;

	public static void main(String[] args) {
		SpringApplication.run(ApiInvoiceUtsApplication.class, args);
	}

	@Override
	public void run(String... args) throws Exception {
		String password = "12345";
		
		for (int i = 0; i < 4; i++) {
			String passwordBcrypt = passwordEncoder.encode(password);
			System.out.println(passwordBcrypt);
		}
		
	}

}
```
### 8 Crea InfoAdicionalToken

![image](https://user-images.githubusercontent.com/31961588/161350350-a4f492cc-037a-49f6-a103-8b4cd83b7175.png)

**Codigo InfoAdicionalToken**

```Java
package com.webservice.uts.auth;

import java.util.HashMap;
import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.oauth2.common.DefaultOAuth2AccessToken;
import org.springframework.security.oauth2.common.OAuth2AccessToken;
import org.springframework.security.oauth2.provider.OAuth2Authentication;
import org.springframework.security.oauth2.provider.token.TokenEnhancer;
import org.springframework.stereotype.Component;

import com.webservice.uts.models.entites.Usuario;
import com.webservice.uts.models.services.IUsuarioService;



@Component
public class InfoAdicionalToken implements TokenEnhancer {
	
	@Autowired
	private IUsuarioService usuarioService;

	@Override
	public OAuth2AccessToken enhance(OAuth2AccessToken accessToken, OAuth2Authentication authentication) {
		
		Usuario usuario=usuarioService.findByUsername(authentication.getName());
		
		Map<String,Object> info = new HashMap<>();
		info.put("info_adicional", "Hola que tal! : ".concat(authentication.getName()));
		info.put("nombre",usuario.getNombre());
		info.put("apellido",usuario.getApellido());
		info.put("email",usuario.getEmail());

		((DefaultOAuth2AccessToken) accessToken).setAdditionalInformation(info);		
			
		return accessToken;
	}
	
	
	

}


```

### 9  Crea JwtConfig

![image](https://user-images.githubusercontent.com/31961588/161350651-196f77a1-5888-4c6c-8896-d7f9c91ba380.png)


**Codigo JwtConfig**

```Java
public class JwtConfig {
	
	public static final String LLAVE_SECRETA="alguna.clave.secreta.322929292";

}
```

### 10 Creación ResourceServerConfig

![image](https://user-images.githubusercontent.com/31961588/161350941-23a205c4-2212-4f8b-873e-2ef573af5668.png)

```Java
import java.util.Arrays;

import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.Ordered;
import org.springframework.http.HttpMethod;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.oauth2.config.annotation.web.configuration.EnableResourceServer;
import org.springframework.security.oauth2.config.annotation.web.configuration.ResourceServerConfigurerAdapter;
import org.springframework.web.cors.CorsConfiguration;
import org.springframework.web.cors.CorsConfigurationSource;
import org.springframework.web.cors.UrlBasedCorsConfigurationSource;
import org.springframework.web.filter.CorsFilter;


@Configuration
@EnableResourceServer
public class ResourceServerConfig extends ResourceServerConfigurerAdapter {

		
	@Override
	public void configure(HttpSecurity http) throws Exception {
		http.authorizeRequests().antMatchers(HttpMethod.GET,"/api/clientes").permitAll()
		/*.and().cors().configurationSource(corsConfigurationSource());*/
		.anyRequest().authenticated()
		.and().cors().configurationSource(corsConfigurationSource());
		
	}
	
	/*@Bean
	public CorsConfigurationSource corsConfigurationSource(){
		CorsConfiguration config= new CorsConfiguration();
		config.setAllowedOrigins(Arrays.asList("http://localhost:4200","*"));
		config.setAllowedMethods(Arrays.asList("GET","POST","PUT","DELETE","OPTIONS"));
		config.setAllowCredentials(true);
		config.setAllowedHeaders(Arrays.asList("Content-Type","Authorization"));
		UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
		source.registerCorsConfiguration("/**", config);
		return source;
	}	*/
	@Bean
	public CorsConfigurationSource corsConfigurationSource() {
		CorsConfiguration config = new CorsConfiguration();
		config.setAllowedOrigins(Arrays.asList("http://localhost:4200"));
		config.setAllowedMethods(Arrays.asList("GET", "POST", "PUT", "DELETE", "OPTIONS"));
		config.setAllowCredentials(true);
		config.setAllowedHeaders(Arrays.asList("Content-Type", "Authorization"));		
		UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
		source.registerCorsConfiguration("/**", config);
		return source;
	}
	
	
	@Bean
	public FilterRegistrationBean<CorsFilter> corsFilter(){
		FilterRegistrationBean<CorsFilter> bean = new FilterRegistrationBean<CorsFilter>(new CorsFilter(corsConfigurationSource()));
		bean.setOrder(Ordered.HIGHEST_PRECEDENCE);
		return bean;
	}
	
	
}


```


### 11 Creación de la clase AuthorizationServerConfig

![image](https://user-images.githubusercontent.com/31961588/161345557-3ff568aa-0bcd-4246-84da-55f089664da2.png)

**Código fuente**

```Java
package com.webservice.uts.auth;


import java.util.Arrays;

import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.Ordered;
import org.springframework.http.HttpMethod;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.oauth2.config.annotation.web.configuration.EnableResourceServer;
import org.springframework.security.oauth2.config.annotation.web.configuration.ResourceServerConfigurerAdapter;
import org.springframework.web.cors.CorsConfiguration;
import org.springframework.web.cors.CorsConfigurationSource;
import org.springframework.web.cors.UrlBasedCorsConfigurationSource;
import org.springframework.web.filter.CorsFilter;


@Configuration
@EnableResourceServer
public class ResourceServerConfig extends ResourceServerConfigurerAdapter {

		
	@Override
	public void configure(HttpSecurity http) throws Exception {
		http.authorizeRequests().antMatchers(HttpMethod.GET,"/api/clientes").permitAll()
		/*.and().cors().configurationSource(corsConfigurationSource());*/
		.anyRequest().authenticated()
		.and().cors().configurationSource(corsConfigurationSource());
		
	}
	
	/*@Bean
	public CorsConfigurationSource corsConfigurationSource(){
		CorsConfiguration config= new CorsConfiguration();
		config.setAllowedOrigins(Arrays.asList("http://localhost:4200","*"));
		config.setAllowedMethods(Arrays.asList("GET","POST","PUT","DELETE","OPTIONS"));
		config.setAllowCredentials(true);
		config.setAllowedHeaders(Arrays.asList("Content-Type","Authorization"));
		UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
		source.registerCorsConfiguration("/**", config);
		return source;
	}	*/
	@Bean
	public CorsConfigurationSource corsConfigurationSource() {
		CorsConfiguration config = new CorsConfiguration();
		config.setAllowedOrigins(Arrays.asList("http://localhost:4200"));
		config.setAllowedMethods(Arrays.asList("GET", "POST", "PUT", "DELETE", "OPTIONS"));
		config.setAllowCredentials(true);
		config.setAllowedHeaders(Arrays.asList("Content-Type", "Authorization"));		
		UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
		source.registerCorsConfiguration("/**", config);
		return source;
	}
	
	
	@Bean
	public FilterRegistrationBean<CorsFilter> corsFilter(){
		FilterRegistrationBean<CorsFilter> bean = new FilterRegistrationBean<CorsFilter>(new CorsFilter(corsConfigurationSource()));
		bean.setOrder(Ordered.HIGHEST_PRECEDENCE);
		return bean;
	}
	
	
}


```







