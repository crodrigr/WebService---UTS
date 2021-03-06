# Crear controlador de cliente

### 1. Crear package de controllers

![image](https://user-images.githubusercontent.com/31961588/155827338-c2a2922c-5c1d-4a65-ade6-550bb4d4bbc5.png)

### 2. Crear la clase  ClienteRestController

![image](https://user-images.githubusercontent.com/31961588/155827353-b4f1ae49-9b97-4cd1-87c9-db0fbc26f92a.png)

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
		response.put("mensaje","El cliente ha sido creado con ??xito!");
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
		response.put("mensaje","El cliente ha sido actulizado con ??xito!");
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
		
		 response.put("mensaje", "El cliente eliminado con ??xito");		 
		 return new ResponseEntity<Map<String,Object>>(response,HttpStatus.OK);
		
	}
	
	
}
```

### 3. Creamos el archivo import.sql
![image](https://user-images.githubusercontent.com/31961588/155827609-6ecc8d23-a23d-48c5-86d2-8523caa92506.png)

```Sql
INSERT INTO clientes (nombre, apellido, email, create_at) VALUES('Camilo', 'Guzm??n', 'profesor@gmail.com', '2018-01-01');
INSERT INTO clientes (nombre, apellido, email, create_at) VALUES('Mr. John', 'Doe', 'john.doe@gmail.com', '2018-01-02');
INSERT INTO clientes (nombre, apellido, email, create_at) VALUES('Linus', 'Torvalds', 'linus.torvalds@gmail.com', '2018-01-03');
INSERT INTO clientes (nombre, apellido, email, create_at) VALUES('Rasmus', 'Lerdorf', 'rasmus.lerdorf@gmail.com', '2018-01-04');
INSERT INTO clientes (nombre, apellido, email, create_at) VALUES('Erich', 'Gamma', 'erich.gamma@gmail.com', '2018-02-01');
INSERT INTO clientes (nombre, apellido, email, create_at) VALUES('Richard', 'Helm', 'richard.helm@gmail.com', '2018-02-10');
INSERT INTO clientes (nombre, apellido, email, create_at) VALUES('Ralph', 'Johnson', 'ralph.johnson@gmail.com', '2018-02-18');
INSERT INTO clientes (nombre, apellido, email, create_at) VALUES('John', 'Vlissides', 'john.vlissides@gmail.com', '2018-02-28');
INSERT INTO clientes (nombre, apellido, email, create_at) VALUES('Dr. James', 'Gosling', 'james.gosling@gmail.com', '2018-03-03');
INSERT INTO clientes (nombre, apellido, email, create_at) VALUES('Magma', 'Lee', 'magma.lee@gmail.com', '2018-03-04');
INSERT INTO clientes (nombre, apellido, email, create_at) VALUES('Tornado', 'Roe', 'tornado.roe@gmail.com', '2018-03-05');
INSERT INTO clientes (nombre, apellido, email, create_at) VALUES('Jade', 'Doe', 'jane.doe@gmail.com', '2018-03-06');


```

### 4. Se configura los parametros de conexion a la base de datos.

![image](https://user-images.githubusercontent.com/31961588/155827662-5805859f-2cc7-4bb9-bc41-5b85fe027990.png)

```Txt
spring.datasource.url=jdbc:postgresql://localhost:5432/db_c4_g28_facturador_test
spring.datasource.username=postgres
spring.datasource.password=admin
spring.datasource.driver-class-name=org.postgresql.Driver
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.ddl-auto=create-drop
logging.level.org.hibernate.SQL=debug

server.port=${PORT:8080}

```

### 5. Test consulta de clientes

![image](https://user-images.githubusercontent.com/31961588/155827843-3fd4e8af-efa5-4816-bd9c-1387bc274bdc.png)

