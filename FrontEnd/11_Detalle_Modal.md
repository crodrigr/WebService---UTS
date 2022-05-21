# Detalle información de clientes y sus facturas. 

## 1. Crear el directorio detalle

Este directorio se crea dentro del directorio cliente.

![image](https://user-images.githubusercontent.com/31961588/168926089-df79fabe-104d-4135-a33b-bf7dc4b8b790.png)

## 2. Crear el componente detalle

Se crea el componente detalle dentro del directorio detalle. La idea es tener un componente detalle que cargue la información del cliente y sus facturas. 
Para esto se va abril desde un modal del listado de clientes.

![image](https://user-images.githubusercontent.com/31961588/168926533-c368b916-0800-4816-8087-379080c83e98.png)

## 3. Crear un servicio para abril el modal

![image](https://user-images.githubusercontent.com/31961588/168926822-ecffe398-0fd5-4249-8fee-bfaa8adb846e.png)

## 4. Definimos los servicios abril y cerrar modal

![image](https://user-images.githubusercontent.com/31961588/168930744-5ca39b3f-533f-4240-bf43-97cae6336cce.png)

## 5. Relación entre cliente y factura 

![image](https://user-images.githubusercontent.com/31961588/168931721-67136c57-cf1f-4b33-ae51-e6c2470c7e62.png)


## 6. Definimos los métodos de detalle.component.ts

**importar las clases necesarias**

![image](https://user-images.githubusercontent.com/31961588/168930998-3fafc926-eec2-4e87-9178-517946bba30d.png)

![image](https://user-images.githubusercontent.com/31961588/168931075-701a9ca9-e99c-4b75-a2ae-2306304e1418.png)

![image](https://user-images.githubusercontent.com/31961588/168931356-988ba7f4-dd08-45d0-b822-fd53486b085a.png)

## 7. Definims detalle.component.html

![image](https://user-images.githubusercontent.com/31961588/168932205-292c20a2-8891-42be-9a6b-e1454cbef7a7.png)

![image](https://user-images.githubusercontent.com/31961588/168932542-be5f3e0b-9eb7-428f-986e-e0fc4f7ce6e5.png)

![image](https://user-images.githubusercontent.com/31961588/168932612-083f9688-9277-4b12-a000-f492a83424e5.png)

## 8. Ajuste al cliente.component.ts para detalle

![image](https://user-images.githubusercontent.com/31961588/168936717-7be3f406-b4ec-47e6-b03d-32ecbbd5de2d.png)

![image](https://user-images.githubusercontent.com/31961588/168936802-e806c49c-ef65-45df-95de-5d9b2edabfc6.png)

![image](https://user-images.githubusercontent.com/31961588/168936856-6ba7be35-d7c6-404d-bc0b-75962358800b.png)

![image](https://user-images.githubusercontent.com/31961588/168936967-b72bad51-f24b-4aa6-9b62-ebfcaead1e2e.png)

![image](https://user-images.githubusercontent.com/31961588/168937141-1c25904b-b22e-42ea-92ca-a3e3981648cf.png)


## 9. Fuentes

**modal.service.ts**
```Typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class ModalService {

  modal: boolean = false;

  constructor() { }

  abrirModal() {
    this.modal = true;
  }

  cerrarModal() {
    this.modal = false;
  }
  
}
```

**detalle.component.ts**

```Typescript
import { Component, OnInit,Input, Output } from '@angular/core';
import { Cliente } from '../cliente';
import { ClienteService } from '../cliente.service';
import { ModalService } from './modal.service';
import swal from 'sweetalert2';
import { HttpEventType } from '@angular/common/http';
import { Factura } from '../../Facturas/models/factura';
import { FacturaService } from '../../Facturas/services/factura.service';

@Component({
  selector: 'detalle-cliente',
  templateUrl: './detalle.component.html',
  styleUrls: ['./detalle.component.css']
})
export class DetalleComponent implements OnInit {

  @Input() cliente: Cliente;
  

  titulo: string = "Detalle del cliente";

  constructor(private clienteService: ClienteService,    
    public modalService: ModalService,
    private facturaService: FacturaService
    ) { }

  ngOnInit(): void {
  }

  cerrarModal() {
    this.modalService.cerrarModal();   
  }

  delete(factura: Factura): void {
    swal.fire({    
      title: 'Está seguro?',
      text: `¿Seguro que desea eliminar la factura: ${factura.descripcion}?`,
      icon: 'warning',
      showCancelButton: true,
      confirmButtonColor: '#3085d6',
      cancelButtonColor: '#d33',
      confirmButtonText: 'Si, eliminar!'
    }).then((result) => {
      if (result.value) {

        this.facturaService.delete(factura.id).subscribe(
          () => {
            this.cliente.facturas = this.cliente.facturas.filter(f => f !== factura)
            swal.fire(
              'Factura Eliminada!',
              `Factura ${factura.descripcion} eliminada con éxito.`,
              'success'
            )
          }
        )

      }
    });
  }
}

```

**detalle.component.html**

```Typescript
<div class="abrir-modal animacion fadeIn" *ngIf="modalService.modal">
  <div class="modal" tabindex="-1" role="dialog" style="display:block;">
    <div class="modal-dialog modal-lg" role="document">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title">{{ titulo }}</h5>
          <button (click)="cerrarModal()" type="button" class="close" data-dismiss="modal" aria-label="Close">
            <span aria-hidden="true">&times;</span>
          </button>
        </div>
        <div class="modal-body">
          <div class="container">
            <div class="row">
  
              <div class="col-sm">
                <ul *ngIf="cliente" class="list-group text-dark mb-3">
                  <li class="list-group-item active">{{cliente.nombre}}</li>
                  <li class="list-group-item">{{cliente.apellido}}</li>
                  <li class="list-group-item">{{cliente.email}}</li>
                  <li class="list-group-item">{{cliente.createAt | date:"fullDate"}}</li>
                  <li class="list-group-item">{{cliente.region.nombre}}</li>
                </ul>    
              </div>    
            </div> 
            
            <!-- listado de facturas -->
            <div class="row">
              <button type="button" class="btn btn-success btn-sm" [routerLink]="['/facturas/form', cliente.id]"  >crear factura</button>
            </div>
            <div class="row">
  
              <div class="alert alert-info my-4" *ngIf="cliente.facturas.length == 0">
                No hay facturas asignadas para el cliente {{cliente.nombre}} {{cliente.apellido}}
              </div>
  
              <table class="table table-bordered table-striped my-4" *ngIf="cliente.facturas.length > 0">
                <thead>
                  <tr>
                    <th>folio</th>
                    <th>descripción</th>
                    <th>fecha</th>
                    <th>total</th>
                    <th>detalle</th>
                    <th>eliminar</th>
                  </tr>
                </thead>
                <tbody>
                  <tr *ngFor="let factura of cliente.facturas">
                    <td>{{factura.id}}</td>
                    <td>{{factura.descripcion}}</td>
                    <td>{{factura.createAt}}</td>
                    <td>{{factura.total |currency:'COP':"symbol"}}</td>
                    <td><button class="btn btn-primary btn-sm" type="button" [routerLink]="['/facturas', factura.id]" >ver</button></td>
                      <td><button class="btn btn-danger btn-sm" type="button" (click)="delete(factura)">eliminar</button></td>
                  </tr>
                </tbody>
              </table>  

            <!-- listado de facturas -->
            
          </div>
        </div>
        <div class="modal-footer">
          <button (click)="cerrarModal()" type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
        </div>
      </div>
    </div>
  </div>
  </div>
  

```
**cliente.component.ts**

```Typescript
import { Component, OnInit } from '@angular/core';
import { ClienteService } from './cliente.service';
import { Cliente } from './cliente';
import { Router } from '@angular/router';
import { ModalService } from '../Cliente/Detalle/modal.service';
import swal from 'sweetalert2';

@Component({
  selector: 'app-cliente',
  templateUrl: './cliente.component.html',
  styleUrls: ['./cliente.component.css']
})
export class ClienteComponent implements OnInit {

  title = 'Cliente';     
  clientes: Cliente[]=[];

  clienteSeleccionado: Cliente;

  constructor(private clienteService: ClienteService,
    private router: Router,
    private modalService: ModalService){

}

  ngOnInit(): void {
    this.getClientes();
  }

  getClientes(): void{    
    this.clienteService.getClientes().subscribe(response => {
       this.clientes = response; 
    });


  }

  delete(cliente: Cliente): void {    
    swal.fire({
      title: 'Está seguro?',
      text: `¿Seguro que desea eliminar al cliente ${cliente.nombre} ${cliente.apellido}?`,
      icon: 'warning',
      showCancelButton: true,
      confirmButtonColor: '#3085d6',
      cancelButtonColor: '#d33',
      confirmButtonText: 'Si, eliminar!'
    }).then((result) => {
      if (result.isConfirmed) {
        this.clienteService.delete(cliente.id).subscribe({
               next: () => {
                this.clientes = this.clientes.filter(cli => cli !== cliente)
                        swal.fire(
                          'Cliente Eliminado!',
                          `Cliente ${cliente.nombre} eliminado con éxito.`,
                          'success'
                        )
               }
        })
      }
    })
 }

 abrirModal(cliente: Cliente) {
  this.clienteSeleccionado = cliente;
   this.modalService.abrirModal();
}


}

```

**cliente.component.html**
```Typescript
<detalle-cliente *ngIf="clienteSeleccionado" 
[cliente] ="clienteSeleccionado">
</detalle-cliente>

<div class="card border-primary mb3">
    <div class="card-header">Clientes</div>
    <div class="card-body text-primary">
       <h5 class="card-title">Listado de clientes</h5>
      <div class="my-2 text-left">
         <button type="button" routerLink='/clientes/form' class="btn btn-success">Crear</button>       
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
            <td><button type="button" class="btn btn-info" (click)="abrirModal(cliente)" >Ver</button></td>
            <td>{{cliente.id}}</td>
            <td>{{cliente.nombre}}</td>
            <td>{{cliente.apellido}}</td>
            <td>{{cliente.email}}</td>
            <td>{{cliente.createAt}}</td>
            <td><button type="button" class="btn btn-secondary"   [routerLink]="['/facturas/form', cliente.id]" >Crear factura</button></td>
            <td><button type="button" [routerLink]="['/clientes/form',cliente.id]" class="btn btn-success" >Actualizar</button></td>
            <td><button type="button" (click)='delete(cliente)' class="btn btn-danger" >Elminar</button></td>
           </tr>
        </tbody>
      </table>
      
    </div>
    </div>
    

```



