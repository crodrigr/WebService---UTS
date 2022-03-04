# Creación del la clase entity Region y relación Many to one con Cliente

- [Hibernate - Relations](https://www.adictosaltrabajo.com/2020/04/02/hibernate-onetoone-onetomany-manytoone-y-manytomany/)

### 1. Creamos la clase entity Region

La clase Region se crea dentro del package Entities. 

![image](https://user-images.githubusercontent.com/31961588/156842170-2286714a-3d41-458f-8b75-585a222a341a.png)


#### 1.1 Clase Region

La clase region tiene dos atributos el Id de la región y nombre. Por ejemplo, id:1 nombre: centro america. 

![image](https://user-images.githubusercontent.com/31961588/156842566-c40fcd0f-ff87-4ae3-a9ee-4e0f5bdcaea3.png)



### 2. Codigo de la clase Region.

```Java
package com.webservice.uts.models.entites;

import java.io.Serializable;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name = "regiones")
public class Region implements Serializable {

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long Id;

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
