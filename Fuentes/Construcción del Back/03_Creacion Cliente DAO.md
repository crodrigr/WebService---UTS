# Creación del Cliente DAO. 

### 1. Se crea el package DAO
#### 1.1 
![image](https://user-images.githubusercontent.com/31961588/155825310-adebc33e-cac6-4064-a119-ceeda093b3cb.png)
#### 2.1
![image](https://user-images.githubusercontent.com/31961588/155825356-9d3eb729-5aec-4579-84ca-d3cbfb9d2342.png)

### 2. Se crea la interfaz IClienteDao
#### 2.1
![image](https://user-images.githubusercontent.com/31961588/155825393-9cbcb275-b885-47d7-89ce-387be8d0efdd.png)
#### 2.2
![image](https://user-images.githubusercontent.com/31961588/155825440-9f5e0e17-e211-4e0f-8248-f89b41f9d9c5.png)

### 3. Código fuente

```Java
package com.webservice.uts.models.dao;

import org.springframework.data.repository.CrudRepository;

import com.webservice.uts.models.entites.Cliente;


public interface IClienteDao extends CrudRepository<Cliente,Long> {
	

}
```
