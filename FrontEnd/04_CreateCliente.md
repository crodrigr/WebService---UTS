# Crear cliente

# 1. Crear el componente form, el formulario para crear y actulizar. 

Antes de iniciar con la creación de formulario de creación de cliente se debe importar el FormsModule (ngModel) o ReactiveFormsModule en el app.module de su proyecto

![image](https://user-images.githubusercontent.com/31961588/167023791-455618b3-a761-41dc-9cda-badf03dd532d.png)


### 1.1 Crear el componente form crear cliente. 

![image](https://user-images.githubusercontent.com/31961588/166971616-971546c2-3023-4f2e-9680-408f3b9f85fa.png)


### 1.2 El componente form.component.ts importamos la librerias que se necesita para el create cliente

![image](https://user-images.githubusercontent.com/31961588/166972576-48537fa7-88e4-4c00-a329-e6c1918f8fa7.png)

### 1.3 Defininción de los atributos y constructor de la clase form.component.ts

![image](https://user-images.githubusercontent.com/31961588/166973780-cc4c8a6e-a679-4a94-ab6d-65098cd3847d.png)

### 1.3 Creación un método que nos traiga el listado de todas las regiones para seleccionar en el formulario de cliente

#### 1.3.1 Crear servicio getRegiones en el cliente.service.ts

**Importamos la interfaz region en cliente.service.ts

![image](https://user-images.githubusercontent.com/31961588/166974654-6ae4b98b-f6bb-4df0-a4a6-f863616df33b.png)

```Typescript
import { Region } from './region';

```

Definimos el getRegiones el cliente.service.ts, dicho servicio es get, el cual, llama al contrallador clientes y invoca el endpoint /cliente/regiones, el cual, 
listado de todas la regiones que tenemos en nuestra base datos. 

![image](https://user-images.githubusercontent.com/31961588/166975145-b758168a-1ab7-4d97-8605-571fdf74c53d.png)

```TypeScript
  getRegiones(): Observable<Region[]> {
    return this.http.get<Region[]>(this.urlApi + '/clientes/regiones');
  }
```

### 1.3.2 Método getRegiones en el form.component.ts

En el componente del form, definimos un metodo que, traiga el back todas la regiones y las guardes en la lista regiones, este método se debe ejecutar cada vez 
que se carga el componente form.

![image](https://user-images.githubusercontent.com/31961588/166976252-7518cc50-d197-4400-b7eb-9036c10bee4a.png)

### 1.3.4 Método crearCliente en el form.component.ts

Se crea en el cliente.service.ts el método create que hacer una petición post al back pasandole el objeto cliente. Se importa map,catchError , y tap


![image](https://user-images.githubusercontent.com/31961588/166978585-50de134c-4fb8-49e2-9bfe-f7f428572b9c.png)


Método create cliente en el form.component.ts

![image](https://user-images.githubusercontent.com/31961588/166979219-d41c5495-cdfe-4b10-b99c-12812c817aae.png)

En el **form.component.ts** 

![image](https://user-images.githubusercontent.com/31961588/167016136-4d7b99b1-affb-4e73-aad2-4f1924eb5927.png)

## 1.4 Formulario html crear cliente

![image](https://user-images.githubusercontent.com/31961588/167025560-27827ddb-5b64-4681-812d-8bb84c57cf08.png)

![image](https://user-images.githubusercontent.com/31961588/167025607-e0e92e9a-4b44-4e2e-92f6-a217ab18c83d.png)


