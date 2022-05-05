# Crear list clientes

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

### 4. Crear servicio de cliente

Un servicio en angular es una clase ofrece los servicios serán invocados al backed. Una clase de service aplica inyección de dependencia. Para crear una clase de servicio se hace a través del comando **ng g service cliente** . Crea dos archivos cliente.service.ts y cliente.service.spec.ts, este ultimo no lo vamos a usar por que es para programar pruebas automáticas. 

![image](https://user-images.githubusercontent.com/31961588/166608399-252d5768-76f6-40e8-aaf2-9c92369717c4.png)

Se observa que tiene decorador @Injectable, por lo tanto, usa inyección de dependencias, esto nos permite usarla en cualquier componente. 

![image](https://user-images.githubusercontent.com/31961588/166608638-0c88ae2a-e3ee-423b-873f-0a3ead5e1666.png)

### 4.1 Configuración path del back del Enviroment 

Esta es una variable de entorno vamos a configurar con la path del backend. 

![image](https://user-images.githubusercontent.com/31961588/166608812-77c55f5f-91a0-483e-bd16-5cc6b51d005d.png)

## 4.2 HttpClient

El httpClient me permite hacer peticiones http: get, post, delete, push, en este caso se van hacer a nuestro backend. 

![image](https://user-images.githubusercontent.com/31961588/166609142-5b31f255-530a-4c23-a64e-eed8b15960de.png)


HttpRequest,HttpEvent y HttpHeaders se importa para el manejo de los headers y request. 

## 4.3 Definimos el metodo constructor

![image](https://user-images.githubusercontent.com/31961588/166609660-31539a53-266e-4eaa-8a03-f2bbc8aa1516.png)


## 4.4 Método getClientes

Definimos el metodo getClientes, el cual, va hacer una petición get al back para traer todos los clientes que tenemos registrados en nuestra base de datos. 

![image](https://user-images.githubusercontent.com/31961588/166610288-b60a0fb2-add1-4dfb-9942-7ad81e75094c.png)

**Código de cliente.service.ts**

```TypeScript
import { Injectable } from '@angular/core';
import { HttpClient, HttpRequest, HttpEvent,HttpHeaders } from '@angular/common/http';
import { environment } from 'src/environments/environment';
import { Observable, throwError } from 'rxjs';
import { Cliente } from './cliente';

@Injectable({
  providedIn: 'root'
})
export class ClienteService {

  private urlApi: string ="";

  constructor(private http: HttpClient){
     this.urlApi = environment.apiUrl+'/api';
   }

  getClientes(): Observable<Cliente[]> {
    return this.http.get<Cliente[]>(this.urlApi + '/clientes');
  }
}
```

## 5. Crear una table que muestre el listado de clientes

### 5.1 cliente.component.ts

El cliente.component.ts se define la lógica del componente, en este caso, los métodos que vamos a usar para llamar al backend para traer todos los registros de nuestros clientes y mostrarlos en nuestra tabla de clientes. 

![image](https://user-images.githubusercontent.com/31961588/166611612-030a1146-efd5-4c32-b990-6d5162a4778e.png)


**Código cliente.component.ts**

```TypeScript
import { Component, OnInit } from '@angular/core';
import { ClienteService } from './cliente.service';
import { Cliente } from './cliente';

@Component({
  selector: 'app-cliente',
  templateUrl: './cliente.component.html',
  styleUrls: ['./cliente.component.css']
})
export class ClienteComponent implements OnInit {

  title = 'Cliente';     
  clientes: Cliente[]=[];

  constructor(private clienteService: ClienteService) { }

  ngOnInit(): void {
    this.getClientes();
  }

  getClientes(): void{    
    this.clienteService.getClientes().subscribe(response => {
       this.clientes = response; 
    });


  }

}
```

### 5.2 cliente.component.html

![image](https://user-images.githubusercontent.com/31961588/166612680-976a55c6-058d-4070-b1ae-f065c942ed50.png)

![image](https://user-images.githubusercontent.com/31961588/166612745-442691b0-7bbb-4a0e-8dee-a42405e4ed6b.png)

**Código cliente.componente.html**

```Html
<div class="card border-primary mb3">
    <div class="card-header">Clientes</div>
    <div class="card-body text-primary">
       <h5 class="card-title">Listado de clientes</h5>
      <div class="my-2 text-left">       
      </div>
      <div *ngIf="clientes?.length==0" class="alert alert-info">
       No hay registros en la base de datos!
      </div>
      <table class="table table-border table-hover text-primary">
        <thead>
           <tr>
              <th></th>
              <th>#</th>
              <th>Nombre</th>
              <th>Apellido</th>
              <th>Email</th>
              <th>Fecha</th>
              <th>Crear factura</th>
              <th>Actualizar</th>
              <th>Eliminar</th>
           </tr>
        </thead>
        <tbody>      
            <tr *ngFor="let cliente of clientes">
            <td><button type="button" class="btn btn-info" >Ver</button></td>
            <td>{{cliente.id}}</td>
            <td>{{cliente.nombre}}</td>
            <td>{{cliente.apellido}}</td>
            <td>{{cliente.email}}</td>
            <td>{{cliente.createAt}}</td>
            <td><button type="button" class="btn btn-secondary"  >Crear factura</button></td>
            <td><button type="button" class="btn btn-success" >Actualizar</button></td>
            <td><button type="button" class="btn btn-danger" >Elminar</button></td>
           </tr>
        </tbody>
      </table>
      
    </div>
    </div>
    

```

### 5.3 Configurar routing y app.module

![image](https://user-images.githubusercontent.com/31961588/166614536-0468932a-998f-4824-8834-a6c9897b6603.png)

**Código app.module.ts**

```TypeScript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HttpClient } from '@angular/common/http';
import { HttpClientModule } from '@angular/common/http';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { HeaderComponent } from './header/header.component';
import { ClienteComponent } from './Cliente/cliente.component';

@NgModule({
  declarations: [
    AppComponent,
    HeaderComponent,
    ClienteComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    HttpClientModule 
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```


![image](https://user-images.githubusercontent.com/31961588/166614577-313848e4-3b2f-4bdb-ab42-db3235a8d715.png)

**Código del app-routing.module.ts**

```TypeScript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { ClienteComponent } from './Cliente/cliente.component';

const routes: Routes = [ 
   { path: 'clientes', component: ClienteComponent }
   ];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

**Cargando el listado del cliente**

![image](https://user-images.githubusercontent.com/31961588/166614790-35746e68-648c-48f2-8ee6-cd2b536a62c9.png)
