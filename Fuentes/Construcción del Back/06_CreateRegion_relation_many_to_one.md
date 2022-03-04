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



#### 1.2 Codigo de la clase Region.

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

**Nota:** recuerde que se add un nuevo atributo region de tipo **private** debe colocar los getter and setter para que al consultar el cliente cargue la región. 

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
	
	public Region getRegion() {
		return region;
	}

	public void setRegion(Region region) {
		this.region = region;
	}

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

#### 2.3 Actulizamos el import.sql para la tabla Region y Cliente

```Txt
INSERT INTO regiones (id, nombre) VALUES (1, 'Sudamérica');
INSERT INTO regiones (id, nombre) VALUES (2, 'Centroamérica');
INSERT INTO regiones (id, nombre) VALUES (3, 'Norteamérica');
INSERT INTO regiones (id, nombre) VALUES (4, 'Europa');
INSERT INTO regiones (id, nombre) VALUES (5, 'Asia');
INSERT INTO regiones (id, nombre) VALUES (6, 'Africa');
INSERT INTO regiones (id, nombre) VALUES (7, 'Oceanía');
INSERT INTO regiones (id, nombre) VALUES (8, 'Antártida');

INSERT INTO clientes (region_id, nombre, apellido, email, create_at) VALUES(1, 'Camilo', 'Rodriguez', 'profesor@gmail.com', '2018-01-01');
INSERT INTO clientes (region_id, nombre, apellido, email, create_at) VALUES(2, 'Mr. John', 'Doe', 'john.doe@gmail.com', '2018-01-02');
INSERT INTO clientes (region_id, nombre, apellido, email, create_at) VALUES(4, 'Linus', 'Torvalds', 'linus.torvalds@gmail.com', '2018-01-03');
INSERT INTO clientes (region_id, nombre, apellido, email, create_at) VALUES(4, 'Rasmus', 'Lerdorf', 'rasmus.lerdorf@gmail.com', '2018-01-04');
INSERT INTO clientes (region_id, nombre, apellido, email, create_at) VALUES(4, 'Erich', 'Gamma', 'erich.gamma@gmail.com', '2018-02-01');
INSERT INTO clientes (region_id, nombre, apellido, email, create_at) VALUES(3, 'Richard', 'Helm', 'richard.helm@gmail.com', '2018-02-10');
INSERT INTO clientes (region_id, nombre, apellido, email, create_at) VALUES(3, 'Ralph', 'Johnson', 'ralph.johnson@gmail.com', '2018-02-18');
INSERT INTO clientes (region_id, nombre, apellido, email, create_at) VALUES(3, 'John', 'Vlissides', 'john.vlissides@gmail.com', '2018-02-28');
INSERT INTO clientes (region_id, nombre, apellido, email, create_at) VALUES(3, 'Dr. James', 'Gosling', 'james.gosling@gmail.com', '2018-03-03');
INSERT INTO clientes (region_id, nombre, apellido, email, create_at) VALUES(5, 'Magma', 'Lee', 'magma.lee@gmail.com', '2018-03-04');
INSERT INTO clientes (region_id, nombre, apellido, email, create_at) VALUES(6, 'Tornado', 'Roe', 'tornado.roe@gmail.com', '2018-03-05');
INSERT INTO clientes (region_id, nombre, apellido, email, create_at) VALUES(7, 'Jade', 'Doe', 'jane.doe@gmail.com', '2018-03-06');

```

#### 2.4 Consulta findAll Clientes

![image](https://user-images.githubusercontent.com/31961588/156850949-c3f61fd9-2823-4fb5-8d2f-f3f2482b9494.png)


### 3. Crear un servicio rest para obtener el listado de regiones. 

Este servicio es muy importante por que se va usar en el formulario del creación de cliente para selecionar a que región pertenece.

#### 3.1 En la clase IClienteDao agregamos el método findAllRegiones()

![image](https://user-images.githubusercontent.com/31961588/156851300-5c6c87dc-7255-49b3-9d05-583587651e73.png)

```Java
package mintic2022.unab.edu.co.c4g28.facturador.models.dao;

import java.util.List;

import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.CrudRepository;

import mintic2022.unab.edu.co.c4g28.facturador.models.entites.Cliente;
import mintic2022.unab.edu.co.c4g28.facturador.models.entites.Region;

public interface IClienteDao extends CrudRepository<Cliente,Long> {
	
	@Query("from Region")
	public List<Region> findAllRegiones();

}

```

#### 3.2 En la interface IClienteService add findAllRegiones

![image](https://user-images.githubusercontent.com/31961588/156851934-3935e985-f3ea-4b69-aeb0-c87da0867278.png)

#### 3.3 Código de interface IClienteService

```Java
package com.webservice.uts.models.services;

import java.util.List;

import com.webservice.uts.models.entites.Cliente;
import com.webservice.uts.models.entites.Region;

public interface IClienteService {
	
    public List<Cliente> findAll();
	
	public Cliente findById(Long id);
	
	public Cliente save(Cliente cliente);
	
	public void delete(Cliente cliente);
	
	public List<Region> findAllRegiones();

}
```

#### 3.3 En la clase ClienteServiceImpl add la implementación del findAllRegiones()

![image](https://user-images.githubusercontent.com/31961588/156852226-9517f46d-e126-4713-b707-c6bcbca9efad.png)

#### 3.4 Código de la clase ClienteServiceImpl

```Java
package com.webservice.uts.models.services;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import com.webservice.uts.models.entites.Cliente;
import com.webservice.uts.models.entites.Region;
import com.webservice.uts.models.dao.IClienteDao;

@Service
public class ClienteServiceImpl  implements IClienteService {
	
	@Autowired
	private IClienteDao clienteDao;

	@Override
	@Transactional(readOnly=true)
	public List<Cliente> findAll() {
		return (List<Cliente>) clienteDao.findAll();
		
	}

	@Override
	@Transactional(readOnly=true)
	public Cliente findById(Long id) {
		return  clienteDao.findById(id).orElse(null);
	}

	@Override
	@Transactional
	public Cliente save(Cliente cliente) {
		 return clienteDao.save(cliente);
	}

	@Override
	@Transactional
	public void delete(Cliente cliente) {
		clienteDao.delete(cliente);
		
	}
	
	@Override
	@Transactional(readOnly = true)
	public List<Region> findAllRegiones() {
		return clienteDao.findAllRegiones();
	}

}

```

#### 3.4 Crear el servicio rest para obtener el listado de las regiones

![image](https://user-images.githubusercontent.com/31961588/156852510-8c94f364-ff5c-4851-ab0c-604ad108add7.png)

```Java
package com.webservice.uts.controllers;

import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

import javax.validation.Valid;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.dao.DataAccessException;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseStatus;
import org.springframework.web.bind.annotation.RestController;



import com.webservice.uts.models.services.IClienteService;
import com.webservice.uts.models.entites.Cliente;
import com.webservice.uts.models.entites.Region;



@CrossOrigin(origins = { "http://localhost:4200"})
@RestController
@RequestMapping("/api")
public class ClienteRestController {

	@Autowired
	private IClienteService clienteService;
	
	@GetMapping("/clientes")
	public List<Cliente> index(){
		return clienteService.findAll();	
	}
	
	@GetMapping("/cliente/{id}")
	public Cliente show(@PathVariable Long id){
		return clienteService.findById(id);
	}
	
	@PostMapping("/clientes")
	public ResponseEntity<?> create(@Valid @RequestBody Cliente cliente, BindingResult result){
		
		Cliente clienteNew=null;
		
		Map<String, Object> response=new HashMap<>();
		
		if(result.hasErrors()) {		
			List<String> errors= result.getFieldErrors()
					.stream()
					.map(err -> "El campo " +err.getField() +" "+err.getDefaultMessage())
			        .collect(Collectors.toList());	
		
		response.put("errors",errors);
		 return new ResponseEntity<Map<String,Object>>(response,HttpStatus.BAD_REQUEST);
		
		}
		
		try {
			clienteNew= this.clienteService.save(cliente);
		}catch(DataAccessException e) {
		  response.put("mensaje", "Error al realizar el insert en la base de datos");
		  response.put("error", e.getMessage().concat(": ").concat(e.getMostSpecificCause().getMessage()));	
		  return new ResponseEntity<Map<String,Object>>(response,HttpStatus.INTERNAL_SERVER_ERROR);
		}
		response.put("mensaje","El cliente ha sido creado con éxito!");
		response.put("cliente", clienteNew);
		
		return new ResponseEntity<Map<String,Object>>(response,HttpStatus.CREATED);
		
		
		
		
	}
	
	@PutMapping("/cliente/{id}")	
	public ResponseEntity<?> update(@Valid @RequestBody Cliente cliente,BindingResult result,@PathVariable  Long id){
		
		Cliente currentCliente=this.clienteService.findById(id);
		
		Cliente updateCliente=null;
		
        Map<String, Object> response=new HashMap<>();
		
		if(result.hasErrors()) {		
			List<String> errors= result.getFieldErrors()
					.stream()
					.map(err -> "El campo " +err.getField() +" "+err.getDefaultMessage())
			        .collect(Collectors.toList());	
		
		response.put("errors",errors);
		 return new ResponseEntity<Map<String,Object>>(response,HttpStatus.BAD_REQUEST);
		
		}
		
		if(currentCliente==null){
			response.put("mensaje", "Error: no se puede editar, el cliente ID: ".concat(id.toString())
					.concat(" no existe en la base de datos"));
			return new ResponseEntity<Map<String,Object>>(response,HttpStatus.NOT_FOUND);		   
			
		}
		
		try{
			currentCliente.setNombre(cliente.getNombre());
			currentCliente.setApellido(cliente.getApellido());
			currentCliente.setEmail(cliente.getEmail());
			updateCliente=this.clienteService.save(currentCliente);
			
		}catch(DataAccessException e){
			response.put("mensaje", "Error al actulizar en la base de datos");
			response.put("error", e.getMessage().concat(": ").concat(e.getMostSpecificCause().getMessage()));	
			return new ResponseEntity<Map<String,Object>>(response,HttpStatus.INTERNAL_SERVER_ERROR);
			
		}
		response.put("mensaje","El cliente ha sido actulizado con éxito!");
		response.put("cliente", updateCliente);		
		return new ResponseEntity<Map<String,Object>>(response,HttpStatus.CREATED);	
		
	}
	
	@DeleteMapping("/clientes/{id}")
	@ResponseStatus(HttpStatus.NO_CONTENT)
	public ResponseEntity<?> delete(@PathVariable Long id){
		
		Map<String, Object> response=new HashMap<>();		
		try {
			
			Cliente cliente=this.clienteService.findById(id);
		    this.clienteService.delete(cliente);
		
		}catch(DataAccessException e){
			 response.put("mensaje", "Error al eliminar el cliente en la base de datos");
			 response.put("error", e.getMessage().concat(": ").concat(e.getMostSpecificCause().getMessage()));	
			 return new ResponseEntity<Map<String,Object>>(response,HttpStatus.INTERNAL_SERVER_ERROR);			
		}
		
		 response.put("mensaje", "El cliente eliminado con éxito");		 
		 return new ResponseEntity<Map<String,Object>>(response,HttpStatus.OK);
		
	}
	

	@GetMapping("/clientes/regiones")
	public List<Region> listarRegiones(){
		return clienteService.findAllRegiones();
	}
	
	
	
}
```
#### 3.5 Consulta de regiones. 
![image](https://user-images.githubusercontent.com/31961588/156852735-d0d7af15-5885-4733-a572-baec8c76be6d.png)

