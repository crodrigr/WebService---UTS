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

### 1.3 Crear un método que nos traiga el listado de todas las regiones para seleccionar en el formulario de cliente

#### 1.3.1 Crear servicio getRegiones en el cliente.service.ts

**Importamos la interfaz region en cliente.service.ts**

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

Se crea en el cliente.service.ts el método create que hace una petición post al back pasandole el objeto cliente. Se importa map,catchError , y tap


![image](https://user-images.githubusercontent.com/31961588/166978585-50de134c-4fb8-49e2-9bfe-f7f428572b9c.png)


Método create cliente en el form.component.ts

![image](https://user-images.githubusercontent.com/31961588/166979219-d41c5495-cdfe-4b10-b99c-12812c817aae.png)

En el **form.component.ts** 

![image](https://user-images.githubusercontent.com/31961588/167016136-4d7b99b1-affb-4e73-aad2-4f1924eb5927.png)

## 1.4 Formulario html crear cliente

![image](https://user-images.githubusercontent.com/31961588/167025560-27827ddb-5b64-4681-812d-8bb84c57cf08.png)

![image](https://user-images.githubusercontent.com/31961588/167052942-3d66395e-1e76-4be4-9792-e897fe9f8bb0.png)


## 1.5 Colocar botón crear en la tabla clientes

En **cliente.component.html**

![image](https://user-images.githubusercontent.com/31961588/167031005-12cb0976-821b-4857-949d-1bb793e0aeea.png)

```Html
 <div class="my-2 text-left">
         <button type="button" routerLink='/clientes/form' class="btn btn-success">Crear</button>       
      </div>
```

Configuramos la ruta en **app-routing.module.ts**

![image](https://user-images.githubusercontent.com/31961588/167031216-63895837-e2fb-4e9c-8e6f-200f87446a42.png)

```TypeScript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { ClienteComponent } from './Cliente/cliente.component';
import { FormComponent } from './Cliente/form.component';

const routes: Routes = [ 
   { path: 'clientes', component: ClienteComponent },
   { path: 'clientes/form', component: FormComponent }
   ];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

# 2 Código final de form.component.ts, form.componente.html cliente.service.ts

### 2.1 form.component.ts
```TypeScript

import { Component, OnInit } from '@angular/core';
import { Cliente } from 'src/app/Cliente/cliente';
import { Region }  from 'src/app/Cliente/region';
import { ClienteService} from 'src/app/Cliente/cliente.service';
import { Router, ActivatedRoute } from '@angular/router';

@Component({
  selector: 'app-form',
  templateUrl: './form.component.html',
  styleUrls: ['./form.component.css']
})
export class FormComponent implements OnInit {

  titulo: string="Crear Cliente";
  
  cliente: Cliente={};
  regiones: Region[]=[];  
  errores: string[]=[];

  constructor(private clienteService: ClienteService,
              private router: Router,
              private activatedRouter: ActivatedRoute) { }

  ngOnInit(): void {
    this.getRegiones();
  }

  getRegiones(): void{
    this.clienteService.getRegiones().subscribe(respuesta=>{
      this.regiones=respuesta;
    }
    )
  }

  create(): void{
    this.clienteService.create(this.cliente).subscribe({
       next: (cliente: Cliente)=>{
          this.router.navigate(['/clientes']);          
       },
       error: (err)=>{
        this.errores = err.error.errors as string[];
        console.error('Código del error desde el backend: ' + err.status);
        console.error(err.error.errors);  
       } 
    });    
  }
  compararRegion(o1: Region, o2: Region): boolean {
    if (o1 === undefined && o2 === undefined) {
      return true;
    }
    return o1 === null || o2 === null || o1 === undefined || o2 === undefined ? false : o1.id === o2.id;
  }
}

```

### 2.2 form.componente.html
```Html
<ul class="alert alert-danger" *ngIf="errores.length > 0">
    <li *ngFor="let err of errores">
      {{ err }}
    </li>
  </ul>
  <div class="card bg-dark text-white my-2"   style="width: 50%; margin: auto auto;"> 
     <div class="card-header">{{titulo}}</div>
     <div class="card-body">
      <form #clienteForm="ngForm">
        <div class="form-group row">
          <label for="nombre" class="col-form-label colm-sm-2">Nombre</label>
           <div class="col-sm-6">            
               <input type="text" class="form-control" [(ngModel)]="cliente.nombre" id="nombre" name="nombre" style="display:inline; width:300px;" required>
            </div>
        </div>
        <div class="form-group row">
          <label for="apellido" class="col-form-label colm-sm-2">Apellido</label>
           <div class="col-sm-6"> 
             <input type="text" class="form-control" [(ngModel)]="cliente.apellido" name="apellido" style="display:inline; width:300px;" required>            
           </div>
        </div>     
        <div class="form-group row">
          <label for="email" class="col-form-label colm-sm-2">Email</label>
           <div class="col-sm-6">            
            <input type="email" class="form-control" [(ngModel)]="cliente.email" name="email" style="display:inline; width:300px;" required>          
           </div>
        </div>
        <div class="form-group row">
          <label for="createAt" class="col-form-label colm-sm-2">Fecha</label>
           <div class="col-sm-6">            
               <input type="date" class="form-control" [(ngModel)]="cliente.createAt" name="createAt" style="display:inline; width:300px;">
           </div>
        </div>
  
        <div class="form-group row">
          <label for="region" class="col-form-label colm-sm-2">Región</label>
           <div class="col-sm-6">            
               <select [compareWith]="compararRegion" class="form-control" [(ngModel)]="cliente.region" name="region" style="width: 300px;">
                 <option [ngValue]="undefined">---Seleccionar una región----</option>
                 <option *ngFor="let region of regiones" [ngValue]="region">{{region.nombre}}</option>              
               </select>
           </div>
        </div>
       
        
        <div class="form-group my-3 row">
          <div class="col-sm-6">
            <button class="btn btn-primary " role="button" (click)='create()' [disabled]="!clienteForm.form.valid">Crear</button>
            <button class="btn btn-danger m-2"  role="button" routerLink='/clientes'>Cancelar</button>
          </div>
        </div>
      </form>
  
     </div>  
  </div>
  

  

```

### 2.3 cliente.service.ts
```TypeScript
import { Injectable } from '@angular/core';
import { HttpClient, HttpRequest, HttpEvent,HttpHeaders } from '@angular/common/http';
import { environment } from 'src/environments/environment';
import { Observable, throwError } from 'rxjs';
import { map, catchError, tap } from 'rxjs/operators';
import { Cliente } from './cliente';
import { Region } from './region'

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

  getRegiones(): Observable<Region[]> {
    return this.http.get<Region[]>(this.urlApi + '/clientes/regiones');
  }

  create(cliente: Cliente): Observable<Cliente>{
    return this.http.post<Cliente>(`${this.urlApi}/clientes`, cliente).pipe(
       map((response: any)=> response.cliente as Cliente),
       catchError(e=>{
         if(e.status==400){
           return throwError(()=>e);
         }
         if(e.errors.mensaje){
           console.log(e.errors.mensaje);
         } 
         return throwError(()=>e);
       })
    );
  }

}

```
