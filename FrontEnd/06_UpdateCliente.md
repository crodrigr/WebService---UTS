# Actualizar cliente

## 1. Método getCliente por Id en cliente.service.ts

Es necesario tener un método que traiga la información del cliente desde el back para llenar el formulario con los datos del cliente y sobre ellos modificar los
campos necesarios. Recuerde que se debe crear un servicio en **cliente.service.ts** que trae un **observable** y este método se va llamar desde el **form.component.ts**

Antes se declara el router para usarlo en el método getCliente, esto va permitir reenviar al componente cliente una vez trae el cliente del back

![image](https://user-images.githubusercontent.com/31961588/167054250-5f66079f-073e-4c6e-a9ce-a797799ee9ab.png)

Se define el método getCliente.

![image](https://user-images.githubusercontent.com/31961588/167054156-536f0f32-75fe-4bf0-9610-4d4d3d183901.png)

## 2. Método Update cliente en cliente.service.ts

![image](https://user-images.githubusercontent.com/31961588/167055487-067ab694-a453-44c7-956a-d347d1157d6f.png)


## 3. Método Update cliente en form.component.ts

![image](https://user-images.githubusercontent.com/31961588/167056018-629c315d-a082-4993-b516-2afbe4e90ba2.png)

## 4. Creamos el botón de editar cliente y configuramos el path de este servicio. 

### 4.1 configuración de app-routing.module.ts

![image](https://user-images.githubusercontent.com/31961588/167056922-85916b46-6952-400f-afa3-7aa46c5209a9.png)


### 4.2 crear botón editar cliente en cliente.component.html

![image](https://user-images.githubusercontent.com/31961588/167057207-0b2b88e4-b4a1-4bae-9065-6937c4e30d9d.png)



