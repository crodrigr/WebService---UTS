# Crea componente factura

![image](https://user-images.githubusercontent.com/31961588/168708592-645fe298-56fa-48f2-88d7-d4b2746e2ebc.png)


## 1. Métodos de factura.components.ts

**Importar clases**

![image](https://user-images.githubusercontent.com/31961588/168709140-a0e40709-76c6-41e7-bb91-3e2f9e26a039.png)

**Definicion de atributos del componente**

![image](https://user-images.githubusercontent.com/31961588/168709214-3d0bd9e8-7d1b-49db-870d-9c14502124e8.png)

**Método constructor. Inyección de dependencia**

![image](https://user-images.githubusercontent.com/31961588/168709331-bbd27266-f28d-4af2-addc-f04bc6ccc213.png)

**Crear factura**

![image](https://user-images.githubusercontent.com/31961588/168709411-62ef6a09-644d-4b6d-bb40-1ebf36cc728b.png)

**Eliminar factura**

![image](https://user-images.githubusercontent.com/31961588/168709571-a69720dc-48da-4d31-89bc-7e5a3310002d.png)


## 2. Crear formulario factura.component.html

## 2.1 Usamos un card de boostrap 

**Formulario**

![image](https://user-images.githubusercontent.com/31961588/168710555-872eadb3-0f69-4d75-a4b4-49b5e8a08b0d.png)

**Card**

![image](https://user-images.githubusercontent.com/31961588/168710640-f97a34e1-6165-4885-91a3-210aa4aeb016.png)

**Formulario con los campos nombre, descripción y observación**

![image](https://user-images.githubusercontent.com/31961588/168711516-8b6d9916-1cb7-42cd-8748-b5f0626ccf92.png)

**Adiciona el botón crear factura dentro de formulario. Dicho botón llama al método crear y pasa el formulario**

![image](https://user-images.githubusercontent.com/31961588/168712309-f4b353f7-3fc6-4975-bcf5-4d31fe0910cc.png)

**Filtro de productos**

![image](https://user-images.githubusercontent.com/31961588/168713884-9d66697e-379b-4f9d-80b4-adc1ea2ae622.png)

**Usamos el componente autocomplete de angular material**

![image](https://user-images.githubusercontent.com/31961588/168714152-07b029da-7464-48ef-8edc-ed8515a62ed0.png)

![image](https://user-images.githubusercontent.com/31961588/168714690-8c1e3eb8-3e0e-4351-a2fd-d5f77a8d36c2.png)

**Instalar angular material**

![image](https://user-images.githubusercontent.com/31961588/168719978-bc26aed9-a6f3-4ffa-b497-1d42cd223419.png)


**Nota al usar un componente de angular material se debe importa en el app.modulo las librerias del componente para que estén disponible en todo el proyecto**

![image](https://user-images.githubusercontent.com/31961588/168921735-b7ea3dab-c266-40fc-9de5-ca7bf46c122f.png)

**D**

![image](https://user-images.githubusercontent.com/31961588/168715323-7acedbd2-7e55-4e91-8f54-c5436cfacac4.png)

**Método filtre que llama al back**

![image](https://user-images.githubusercontent.com/31961588/168715369-8890c1ad-ec93-408e-b680-3471cb567d8a.png)

**Método que al seleccionar un producto**

![image](https://user-images.githubusercontent.com/31961588/168715748-a8e80823-da39-4e6f-a1fb-aef92f447fc6.png)

**Mensaje si la factura no tiene lineas**

![image](https://user-images.githubusercontent.com/31961588/168715958-40a55517-e2c1-43ca-bbad-37b9e62a07b1.png)

**Tabla que muestra los productos seleccionados**

![image](https://user-images.githubusercontent.com/31961588/168716046-aa72363f-77f5-4239-aa5f-21ca5d7656f0.png)

**Muestra el total de la factura**

![image](https://user-images.githubusercontent.com/31961588/168716133-ac88bfe1-b7a8-4df3-aa6f-873627c3397b.png)

## 3 Configuramos la ruta

**app.routing.module.ts**
![image](https://user-images.githubusercontent.com/31961588/168717375-54c056f5-edf8-4d64-948e-68ea433d5b89.png)

**cliente.component.html**
![image](https://user-images.githubusercontent.com/31961588/168717534-42449021-1e99-4d18-8dd1-8a78781ba9e1.png)


## 4 Fuentes

**facturas.component.ts**

```TypeScript
import { Component, OnInit } from '@angular/core';
import {FormControl} from '@angular/forms';
import {Observable} from 'rxjs';
import {map, flatMap,startWith} from 'rxjs/operators';
import { Factura } from './models/factura';
import { ClienteService } from '../../app/Cliente/cliente.service';
import { ActivatedRoute, Router } from '@angular/router';
import { FacturaService } from './services/factura.service';
import { Producto } from './models/producto';
import { ItemFactura } from './models/item-factura';
import swal from 'sweetalert2';


@Component({
  selector: 'app-facturas',
  templateUrl: './facturas.component.html',
  styleUrls: ['./facturas.component.css']
})
export class FacturasComponent implements OnInit {

  
  titulo: string = 'Nueva Factura';
  factura: Factura = new Factura();
  autocompleteControl = new FormControl();
  productosFiltrados: Observable<Producto[]>;

  ngOnInit() {
    this.activatedRoute.paramMap.subscribe(params => {
      let clienteId = +params.get('clienteId');
      this.clienteService.getCliente(clienteId).subscribe(cliente => this.factura.cliente = cliente);
    });

    this.productosFiltrados = this.autocompleteControl.valueChanges
      .pipe(
        map(value => typeof value === 'string' ? value : value.nombre),
        flatMap(value => value ? this._filter(value) : [])
      );
  }

  constructor(private clienteService: ClienteService,
    private facturaService: FacturaService,
    private router: Router,
    private activatedRoute: ActivatedRoute) { 
    
  }

  private _filter(value: string): Observable<Producto[]> {
    const filterValue = value.toLowerCase();
    return this.facturaService.filtrarProductos(filterValue);
  }

  mostrarNombre(producto?: Producto): string | undefined {
    return producto ? producto.nombre : undefined;
  }

  seleccionarProducto(value: any): void {
    
    let producto =value as Producto;
    console.log(producto);

    if (this.existeItem(producto.id)) {
      this.incrementaCantidad(producto.id);
    } else {
      let nuevoItem = new ItemFactura();
      nuevoItem.producto = producto;
      this.factura.items.push(nuevoItem);
    }

    this.autocompleteControl.setValue('');
    

  }

  actualizarCantidad(id: number, event: any): void {
    let cantidad: number = event.target.value as number;

    if (cantidad == 0) {
      return this.eliminarItemFactura(id);
    }

    this.factura.items = this.factura.items.map((item: ItemFactura) => {
      if (id === item.producto.id) {
        item.cantidad = cantidad;
      }
      return item;
    });
  }

  existeItem(id: number): boolean {
    let existe = false;
    this.factura.items.forEach((item: ItemFactura) => {
      if (id === item.producto.id) {
        existe = true;
      }
    });
    return existe;
  }

  incrementaCantidad(id: number): void {
    this.factura.items = this.factura.items.map((item: ItemFactura) => {
      if (id === item.producto.id) {
        ++item.cantidad;
      }
      return item;
    });
  }

  eliminarItemFactura(id: number): void {
    this.factura.items = this.factura.items.filter((item: ItemFactura) => id !== item.producto.id);
  }

  create(facturaForm): void {
    console.log(this.factura);
    if (this.factura.items.length == 0) {
      this.autocompleteControl.setErrors({ 'invalid': true });
    }

    if (facturaForm.form.valid && this.factura.items.length > 0) {
      this.facturaService.create(this.factura).subscribe(factura => {
        swal.fire(this.titulo, `Factura ${factura.descripcion} creada con éxito!`, 'success');
        this.router.navigate(['/clientes']);
      });
    }
  }

  

}


```

**facturas.component.html**

```TypeScript
<div class="card bg-light">
  <div class="card-header">{{titulo}}: {{factura.descripcion}}</div>
  <div class="card-body">
    <h4 class="card-title">
      <a [routerLink]="['/clientes']" class="btn btn-light btn-xs">&laquo; volver</a>
    </h4>

    <form #facturaForm="ngForm">

      <div class="form-group row" *ngIf="factura.cliente">
        <label for="cliente" class="col-sm-2 col-form-label">Cliente</label>
        <div class="col-sm-6">
          <input type="text" name="cliente" value="{{factura.cliente.nombre}} {{factura.cliente.apellido}}" class="form-control" disabled>
        </div>
      </div>

      <div class="form-group row">
        <label for="descripcion" class="col-sm-2 col-form-label">Descripción</label>
        <div class="col-sm-6">
          <input type="text" name="descripcion" [(ngModel)]="factura.descripcion" class="form-control" required #descripcion="ngModel">
          <div class="alert alert-danger" *ngIf="descripcion.invalid && descripcion.touched || descripcion.invalid && facturaForm.submitted">
            La descripción es requerida.
          </div>
        </div>
      </div>

      <div class="form-group row">
        <label for="observacion" class="col-sm-2 col-form-label">Observación</label>
        <div class="col-sm-6">
          <textarea name="observacion" [(ngModel)]="factura.observacion" class="form-control"></textarea>
        </div>
      </div>

      <div class="form-group row">
        <div class="col-sm-6">
          <mat-form-field>
            <input type="text" placeholder="Añadir producto" aria-label="Productos" matInput [formControl]="autocompleteControl" [matAutocomplete]="auto">
            <mat-autocomplete #auto="matAutocomplete" [displayWith]="mostrarNombre" (optionSelected)="seleccionarProducto($event.option.value)">
              <mat-option *ngFor="let producto of productosFiltrados | async" [value]="producto">
                {{producto.nombre}}
              </mat-option>
            </mat-autocomplete>
          </mat-form-field>
          <div class="alert alert-danger" *ngIf="autocompleteControl.invalid && facturaForm.submitted">
            La factura no puede no tener líneas!.
          </div>
        </div>
      </div>

      <div class="alert alert-info my-4" *ngIf="factura.items.length == 0">
        No hay líneas asignadas para la factura. Debe agregar al menos una!
      </div>
      <table class="table table-striped table-hover table-sm" *ngIf="factura.items.length > 0">
        <thead>
          <tr>
            <th>Producto</th>
            <th>Precio</th>
            <th>Cantidad</th>
            <th>Total</th>
            <th>Eliminar</th>
          </tr>
        </thead>
        <tbody>
          <tr *ngFor="let item of factura.items">
            <td>{{item.producto.nombre}}</td>
            <td>{{item.producto.precio |currency:'COP':"symbol"}}</td>
            <td><input type="number" value="{{item.cantidad}}" class="form-control col-sm-4" (change)="actualizarCantidad(item.producto.id, $event)"></td>
            <td>{{item.calcularImporte() |currency:'COP':'symbol'}}</td>
            <td><button class="btn btn-danger btn-sm" type="button"
              (click)="eliminarItemFactura(item.producto.id)">x</button></td>
          </tr>
        </tbody>
      </table>
      <h5 class="float-right" *ngIf="factura.items.length > 0">Gran Total: <span>{{factura.calcularGranTotal() |currency:'COP':'symbol' }}</span></h5>

      <div class="form-group row">
        <div class="col-sm-6">
          <input type="submit" (click)="create(facturaForm)" value="Crear Factura" class="btn btn-secondary">
        </div>
      </div>
      
    </form>

  </div>
</div>



```
**app.module.ts**

```TypeScript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HttpClient } from '@angular/common/http';
import { HttpClientModule } from '@angular/common/http';
import { FormsModule, ReactiveFormsModule  } from '@angular/forms';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { HeaderComponent } from './header/header.component';
import { ClienteComponent } from './Cliente/cliente.component';
import { FormComponent } from './Cliente/form.component';
import { FacturasComponent } from './Facturas/facturas.component';
import { NoopAnimationsModule } from '@angular/platform-browser/animations';

import {MatAutocompleteModule} from '@angular/material/autocomplete';
import { MatFormFieldModule } from '@angular/material/form-field';
import { MatInputModule } from '@angular/material/input';

@NgModule({
  declarations: [
    AppComponent,
    HeaderComponent,
    ClienteComponent,
    FormComponent,
    FacturasComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    HttpClientModule,
    FormsModule,
    ReactiveFormsModule,
    NoopAnimationsModule,
    MatAutocompleteModule,
    MatFormFieldModule,
    MatInputModule

  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }

```

**app-routing.module.ts**

```TypeScript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { ClienteComponent } from './Cliente/cliente.component';
import { FormComponent } from './Cliente/form.component';
import { FacturasComponent } from './Facturas/facturas.component';

const routes: Routes = [ 
   { path: 'clientes', component: ClienteComponent },
   { path: 'clientes/form', component: FormComponent },
   { path: 'clientes/form/:id', component: FormComponent},
   { path: 'facturas/form/:clienteId', component: FacturasComponent}
   ];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```
