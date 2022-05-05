# Crear cliente

# 1. Crear el componente form, el formulario para crear y actulizar. 

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

``
