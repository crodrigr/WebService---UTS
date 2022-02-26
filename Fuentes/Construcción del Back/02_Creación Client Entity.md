# Creación de la clase Client Entity

### 1. Creamos el paquete models que tiene la siguiente estructura
#### 1.1 Figura 1. 
![image](https://user-images.githubusercontent.com/31961588/155821016-84d34e64-ebb2-4967-9a69-0efc525eb1cb.png)
<br>
#### 1.2  Figura 2. 
![image](https://user-images.githubusercontent.com/31961588/155821172-974d92c4-0fc0-4757-87d1-c904014bc50b.png)
<br>
#### 1.3  Figura 3. 
![image](https://user-images.githubusercontent.com/31961588/155821192-4f7c4a01-f737-4b9d-af38-d3e8669dc4a0.png)

### 2. Creamos la clase Client
#### 2.1  Figura 4. 
![image](https://user-images.githubusercontent.com/31961588/155821243-5b657ab9-540e-49da-a560-76b540543226.png)
<br>
#### 2.2  Figura 5. 
![image](https://user-images.githubusercontent.com/31961588/155821293-05b5a53d-dafa-4dd7-8db1-23319d68d56d.png)
<br>
#### 2.3  Figura 6. 
![image](https://user-images.githubusercontent.com/31961588/155821551-e4a433f9-cb57-4ec8-9cb5-8afecce5b82c.png)

### 3. Definición de decoradores Entity y Table
#### 3.1  Figura 7. 
![image](https://user-images.githubusercontent.com/31961588/155821719-f46ab665-7896-4eff-91a6-80533fd20fe4.png)

### 4. Definición de atributos.
#### 4.1  Figura 8. 
![image](https://user-images.githubusercontent.com/31961588/155822005-68095039-0999-4ef9-be99-8a5f90aa1b44.png)
#### 4.2  Figura 9.
Se implements la interface Serializable, por lo tanto de debe implementar el serialVersionUID. Se declara los atributos iniciales, los cuales, se van a mapear en la base de datos. 

![image](https://user-images.githubusercontent.com/31961588/155822148-4371173f-857e-4e01-a360-08636ab49f9a.png)

#### 4.4 Figura 10. Generar getters and setters
![image](https://user-images.githubusercontent.com/31961588/155824781-380510ab-e529-406f-bcd4-06a8355858c5.png)

#### 4.5  Figura 11.
![image](https://user-images.githubusercontent.com/31961588/155824808-eeb0d32c-9383-448b-b119-6fc30d46939e.png)

#### 4.6  Figura 12.
![image](https://user-images.githubusercontent.com/31961588/155825002-f03998c8-f60b-40bb-8e7b-1287aa11f966.png)

Si tenemos problemas con **import javax.validation.constraints.NotEmpty;** debemos verificar si esta la dependencia correspondiente. 
#### 4.7  Figura 13.
![image](https://user-images.githubusercontent.com/31961588/155825029-c5023ba6-3937-4365-8896-38c538f4addb.png)

### 5. Código fuente

```Java
package com.webservice.uts.models.entites;

import java.io.Serializable;
import java.util.Date;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;
import javax.persistence.Temporal;
import javax.persistence.TemporalType;
import javax.validation.constraints.Email;
import javax.validation.constraints.NotEmpty;
import javax.validation.constraints.Size;

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


