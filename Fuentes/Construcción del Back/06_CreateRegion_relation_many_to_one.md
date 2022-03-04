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

#### 2.1 En la clase cliente se add el atributo region de la Clase Region con su tipo de relación ManyToOne.

Por otra parte, recuede que de la clase Region no colocamos ningún atributo de tipo Cliente por que no es nuestro interes obtener el listado de clientes por región, es por esto que es una relacion bidireccional.

![image](https://user-images.githubusercontent.com/31961588/156848380-1c7c81c8-3065-45d6-9564-71cbcafa3500.png)

#### 2.2 Código de la clase cliente

```Java
package com.webservice.uts.models.entites;

import java.io.Serializable;
import java.util.Date;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.FetchType;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.ManyToOne;
import javax.persistence.Table;
import javax.persistence.Temporal;
import javax.persistence.TemporalType;
import javax.validation.constraints.Email;
import javax.validation.constraints.NotEmpty;
import javax.validation.constraints.NotNull;
import javax.validation.constraints.Size;

import com.fasterxml.jackson.annotation.JsonIgnoreProperties;

import mintic2022.unab.edu.co.c4g28.facturador.models.entites.Region;

@Entity
@Table(name = "clientes")
public class Cliente implements Serializable {
  
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)	
	private Long Id;
	
	@NotEmpty(message="no puede estar vacio")
	@Size(min=3, max=30, message="el tamaño debe estar entre 3 y 30")	
	@Column(nullable=false)
	private String nombre;
	private String apellido;
	
	@Email(message="no es una dirección de correo bien formada")
	@Column(nullable=false, unique=true)
	private String email;
	
	@Column(name="create_at")
	@Temporal(TemporalType.DATE)
	private Date createAt; 
	
	@NotNull(message="la región no puede ser vacia")
	@ManyToOne(fetch=FetchType.LAZY)
	@JoinColumn(name="region_id")
	@JsonIgnoreProperties({"hibernateLazyInitializer","handler"})
	private Region region;
	
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

	public Date getCreateAt() {
		return createAt;
	}

	public void setCreateAt(Date createAt) {
		this.createAt = createAt;
	}
	
	private static final long serialVersionUID = 1L;
	
	
	
}

```

