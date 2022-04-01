# Implementación del modulo de seguridad en spring boot con JWT

#### 1. Agregar las dependencias de spring security al proyecto. 

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

#### 1.3 Actualizar dependencias del proyecto

![image](https://user-images.githubusercontent.com/31961588/161341932-7ab1e35c-4345-4509-ba73-f7cb335c8aab.png)


### 2. Creamos la Interfaz IUserService

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

#### 2.3 Creamos la clase UsuarioServiceImp

![image](https://user-images.githubusercontent.com/31961588/161342717-f9415d7e-b2a4-434e-afa0-19ed081e4ab3.png)

#### 2.4 Código fuente

```Java
package mintic2022.unab.edu.co.c4g28.facturador.models.services;

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

import mintic2022.unab.edu.co.c4g28.facturador.models.dao.IUsuarioDao;
import mintic2022.unab.edu.co.c4g28.facturador.models.entites.Cliente;
import mintic2022.unab.edu.co.c4g28.facturador.models.entites.Usuario;

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
