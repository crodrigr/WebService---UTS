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
