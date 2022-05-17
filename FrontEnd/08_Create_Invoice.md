
# Modulo para crear la factura

## 1. Crear el directorio para los componentes de modulo de factura

### 1.1 Directorio de facturas

![image](https://user-images.githubusercontent.com/31961588/168698817-655be536-dc8e-45f0-8e7b-d337a5886505.png)


## 2. Crear los modelos del módulo de facturas

### 2.1 Se crea el directorio models dentro de facturas. 

**Se crea la clase producto.**

![image](https://user-images.githubusercontent.com/31961588/168699078-b91944a5-4f73-4170-b2e6-b890d4a631ea.png)

**Se crea la clase item-producto**

![image](https://user-images.githubusercontent.com/31961588/168699435-9315a172-c61d-4077-80ec-92319fa80e86.png)


**Se crea la clase factura**

![image](https://user-images.githubusercontent.com/31961588/168699564-00e4af0e-838b-4b48-b821-0c35dcc4f448.png)

**Se crea el directorio service dentro de factura**

![image](https://user-images.githubusercontent.com/31961588/168700317-5684997a-6487-4364-99fc-3436ea9004ff.png)


**Se crea la clase factura.services.ts**

![image](https://user-images.githubusercontent.com/31961588/168706744-52e67ee1-8acc-40c7-8d37-568b75dddec6.png)

**Se define los servicios que se van a llamar al back para modulo de factura**

- **getFactura(id: number):** obetner una factura por su id
- **delete(id: number):** eliminar factura por su id
- **create(factura: Factura):** crear factura
- **filtrarProductos(term: string):** filtrar productos por nombre

![image](https://user-images.githubusercontent.com/31961588/168706822-b50f347d-d6f6-4088-9437-a976ff8b2d72.png)


