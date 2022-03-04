# Creación del la clase entity Region y relación Many to one con Cliente

- [Hibernate - Relations](https://www.adictosaltrabajo.com/2020/04/02/hibernate-onetoone-onetomany-manytoone-y-manytomany/)
- [Working with Relationships in Spring Data REST](https://www.baeldung.com/spring-data-rest-relationships)
- [Documentación oficial de spring boot](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#legal)

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

### 2 Relación entre región y cliente. 

Una región tiene uno o muchos clientes y un cliente pertenece a una única región. Por parte de la región no nos interesa obtener los clientes por región. Por lo tanto, se convierte e una relación unidireccional desde cliente que si nos interesa saber a que región pertenece.  

![image](https://user-images.githubusercontent.com/31961588/156847559-791a4e02-125d-402e-925a-27cd0be338e9.png)
