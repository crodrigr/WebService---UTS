# Creación de Usuario y Role

## 1. Crear la clase role entity

#### 1.1 Creación de la clase role


![image](https://user-images.githubusercontent.com/31961588/161340093-18d2fe3a-2a3c-4cb3-86a8-a2f829a38576.png)

.

![image](https://user-images.githubusercontent.com/31961588/161340176-7e39204c-26d9-44a5-908d-e7bccb44d8ce.png)



#### 1.2 Código de la clase role

```Java
package com.webservice.uts.models.entites;

import java.io.Serializable;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GenerationType;
import javax.persistence.Table;
import javax.persistence.Id;
import javax.persistence.GeneratedValue;

@Entity
@Table(name="roles")
public class Role implements Serializable {	

	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	private Long Id;
	
	@Column(unique=true, length=20)
	private String nombre;
	
	
	public Long getId() {
		return Id;
	}
	public void setId(Long id) {
		Id = id;
	}
	public String getNombre() {
		return nombre;
	}
	public void setNombre(String nombre) {
		this.nombre = nombre;
	}
	
	/**
	 * 
	 */
	private static final long serialVersionUID = 1L;
	

}


```

## 2. Creación de la clase usuario entity

#### 2.1 Creación de la clase usuario

![image](https://user-images.githubusercontent.com/31961588/161340517-6c3d5de2-f444-45cd-a730-cdbe4fdaf1bd.png)

.

![image](https://user-images.githubusercontent.com/31961588/161340616-5ac43876-8316-45d8-b96b-aee8f9d3883d.png)


#### 2.1 Código de la clase usuario

```Java
package com.webservice.uts.models.entites;


import java.util.List;

import javax.persistence.CascadeType;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.FetchType;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Table;
import javax.persistence.Id;
import javax.persistence.JoinTable;
import javax.persistence.ManyToMany;
import javax.persistence.JoinColumn;
import javax.persistence.UniqueConstraint;

@Entity
@Table(name = "usuarios")
public class Usuario {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)    
	private Long Id;

	@Column(unique=true, length=20)
    private String username;

	@Column(length=60)
	private String password;
	
	private Boolean enabled;

	private String nombre;

	private String apellido;

	@Column(unique=true)
	private String email;

	@ManyToMany(fetch = FetchType.LAZY, cascade = CascadeType.ALL)
	@JoinTable(name="usuarios_roles", joinColumns = @JoinColumn(name="usuario_id"),
	inverseJoinColumns=@JoinColumn(name="role_id"),
	uniqueConstraints = {@UniqueConstraint(columnNames= {"usuario_id","role_id"})})
	private List<Role> roles;
	
	

	public Boolean getEnabled() {
		return enabled;
	}

	public void setEnabled(Boolean enabled) {
		this.enabled = enabled;
	}

	public Long getId() {
		return Id;
	}

	public void setId(Long id) {
		Id = id;
	}

	public String getUsername() {
		return username;
	}

	public void setUsername(String username) {
		this.username = username;
	}

	public String getPassword() {
		return password;
	}

	public void setPassword(String password) {
		this.password = password;
	}

	public String getNombre() {
		return nombre;
	}

	public void setNombre(String nombre) {
		this.nombre = nombre;
	}

	public String getApellido() {
		return apellido;
	}

	public void setApellido(String apellido) {
		this.apellido = apellido;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public List<Role> getRoles() {
		return roles;
	}

	public void setRoles(List<Role> roles) {
		this.roles = roles;
	}

}


```
