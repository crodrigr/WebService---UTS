# Crear crud de clientes

### 1. Creamos el directorio cliente donde va ir los componentes del modulo de clientes. 

![image](https://user-images.githubusercontent.com/31961588/166582863-03158336-c664-42d2-be8e-75bfe80d3ebd.png)

### 2. Creamos el componente cliente

Con el comando **ng g component cliente --flat** se crea el componente de cliente que cuenta de cuatro archivos: .html, .ts, css. spec.ts 

![image](https://user-images.githubusercontent.com/31961588/166586712-f4aed45b-6996-41c3-a8ca-7d6b106c6f16.png)

Archivos que se crearon

![image](https://user-images.githubusercontent.com/31961588/166586927-dde0ca3b-eb55-4bc7-8b5a-7904c96c4d4a.png)

### 3. Creamos los modelos de cliente. 

En la parte del front vamos necesitar crear el tipo de dato clientes y regiones. Hay dos maneras: clase o interfaz. Se usa clase cuando vayamos a instanciar objetos a traves del
operador new, e interfaz en caso contrario. En nuestro caso lo vamos hacer con interfaz por que tomamos la data desde el back. 

### 3.1 Creamos la interfaz cliente

![image](https://user-images.githubusercontent.com/31961588/166587617-ac1f668b-f512-4638-9fd6-2f5994d436c5.png)

Definimos los atributos que tiene nuestro cliente, que en esencia son los mimos que se tiene en la clase cliente del back.

![image](https://user-images.githubusercontent.com/31961588/166587740-54923b5e-e6a8-41ca-baca-6bee37af0109.png)


### 3.2 Creamos la interfaz region


![image](https://user-images.githubusercontent.com/31961588/166587812-fc20c7ca-51d6-48f7-a42a-ef89e20dec9f.png)

Definimos los atributos de región que son los mismo atributos que la clase region del back.

![image](https://user-images.githubusercontent.com/31961588/166587888-da6adf25-711e-4fc5-b9b5-f05f3913fbbc.png)

### 3.3 Relacionamos el cliente con region.

Recuerde que un cliente tiene una region, por lo tanto, existe una relación, similar a la del back entre region y cliente. 

En **clients.ts** se hace el import de region y se define el atributo region. 

![image](https://user-images.githubusercontent.com/31961588/166588102-03917829-0192-4aa0-82af-3c44c6d1ade8.png)

**Código de Cliente.ts**

```TypeScript
import {Region} from './region';

export interface Cliente{
    id?: number;
    nombre?: string;
    apellido?: string;
    createAt?: string;
    email?: string;
    region?: Region; 
   

}
```
**Código de Region.ts**
```TypeScript
export interface Region{
    id?: number;
    nombre?: string;
}
```


