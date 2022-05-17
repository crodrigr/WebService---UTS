
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


## 3. Fuentes

**Producto.ts**

```TypeScript
export class Producto {
    id: number;
    nombre: string;
    precio: number;
  }
```

**Item-factura.ts**

```TypeScript
import { Producto } from './producto';

export class ItemFactura {
  producto: Producto;
  cantidad: number = 1;
  importe: number;

  public calcularImporte(): number {
    return this.cantidad * this.producto.precio;
  }
}
```

**Factura.ts**

```TypeScript
import { ItemFactura } from './item-factura';
import { Cliente } from '../../Cliente/cliente';

export class Factura {
  id: number;
  descripcion: string;
  observacion: string;
  items: Array<ItemFactura> = [];
  cliente: Cliente;
  total: number;
  createAt: string;

  calcularGranTotal(): number {
    this.total =0 ;
    this.items.forEach((item: ItemFactura) => {
      this.total += item.calcularImporte();
    });
    return this.total;
  }


}
```

**factura.service.ts**

```TypeScript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { Factura } from '../models/factura';
import { Producto } from '../models/producto';
import { environment } from 'src/environments/environment';

@Injectable({
  providedIn: 'root'
})
export class FacturaService {

  private urlApi: string ="";

  constructor(private http: HttpClient) {
     this.urlApi = environment.apiUrl+'/api';
   }

   getFactura(id: number): Observable<Factura> {
    return this.http.get<Factura>(`${this.urlApi}/facturas/${id}`);
  }

  delete(id: number): Observable<void> {
    return this.http.delete<void>(`${this.urlApi}/facturas/${id}`);
  }

  filtrarProductos(term: string): Observable<Producto[]> {
    return this.http.get<Producto[]>(`${this.urlApi}/facturas/filtrar-productos/${term}`);
  }

  create(factura: Factura): Observable<Factura> {
    return this.http.post<Factura>(this.urlApi+'/facturas', factura);
  }


}
```


