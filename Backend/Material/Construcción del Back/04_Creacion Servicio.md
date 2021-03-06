# Creación de la interfaz de servicio e implementación de la misma para la entity Cliente. 

### 1. Creación del paquete services
#### 1.1 Figura 1.
![image](https://user-images.githubusercontent.com/31961588/155825601-7951c0b0-1a76-4942-af6d-83ae69113bc5.png)
#### 1.2 Figura 2.
![image](https://user-images.githubusercontent.com/31961588/155825990-a617c507-c1d6-42e0-8036-6e4568e572ef.png)
#### 1.3 Figura 3.
![image](https://user-images.githubusercontent.com/31961588/155826009-482817e1-38f9-4141-ab03-66f8dd8df302.png)
#### 1.4 Figura 4.
![image](https://user-images.githubusercontent.com/31961588/155826045-ae17245b-1da3-419e-913a-58adbdec8403.png)
#### 1.5 Figura 5.
![image](https://user-images.githubusercontent.com/31961588/155826084-70cc13e2-8f79-440d-ba77-8b4a16bad0ea.png)

### 2. Se crea la implementación del IClienteService
#### 2.1 Figura 6.
![image](https://user-images.githubusercontent.com/31961588/155826130-edb4b0e1-9422-4ebd-8b2e-6890eca7eb30.png)
#### 2.2 Figura 7.
![image](https://user-images.githubusercontent.com/31961588/155827092-54be7fcc-2dbb-4db0-8f35-c5ccd7644621.png)


```Java
package com.webservice.uts.models.services;

import java.util.List;

import com.webservice.uts.models.entites.Cliente;

public interface IClienteService {
	
    public List<Cliente> findAll();
	
	public Cliente findById(Long id);
	
	public Cliente save(Cliente cliente);
	
	public void delete(Cliente cliente);
}
```

```Java
package com.webservice.uts.models.services;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import com.webservice.uts.models.entites.Cliente;

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

}
```
