# Se crea le componente de login.

## 1 Se crea el componente login

![image](https://user-images.githubusercontent.com/31961588/171077740-09f3d0b6-0769-4548-be86-90208653f4c7.png)

## 2 Se importa las clases necesarias

![image](https://user-images.githubusercontent.com/31961588/171077911-8a4c67cc-8c95-4c15-aeb4-c342dbb3409e.png)

## 3 Se definie le metodo constructor y  login

![image](https://user-images.githubusercontent.com/31961588/171078807-b389033f-2e90-456e-b61a-7f83e8e24256.png)

## 4 Login html

![image](https://user-images.githubusercontent.com/31961588/171079355-5e3cfba7-1fa7-40e3-941e-41092327e213.png)

**login.component.ts**
```TypeScript
import { Component, OnInit } from '@angular/core';
import { Usuario } from './usuario';
import swal from 'sweetalert2';
import { AuthService } from './auth.service';
import { Router } from '@angular/router';

@Component({
  selector: 'app-login',
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.css']
})
export class LoginComponent implements OnInit {

  titulo: string = 'Por favor Sign In!';
  usuario: Usuario;

  constructor(private authService: AuthService, 
    private router: Router) {
    this.usuario = new Usuario();
}

ngOnInit(): void {
}

login(): void {    
if (this.usuario.username == null || this.usuario.password == null) {
swal.fire('Error Login', 'Username o password vacías!', 'error');
return;
}
this.authService.login(this.usuario).subscribe(response => {     
this.authService.guardarUsuario(response.access_token);
this.authService.guardarToken(response.access_token);
let usuario = this.authService.usuario;
this.router.navigate(['/clientes']);
swal.fire('Login', `Hola ${usuario.username}, has iniciado sesión con éxito!`, 'success');
}, err => {
if (err.status == 400) {
swal.fire('Error Login', 'Usuario o clave incorrectas!', 'error');
}
}
);
}

}


```

**login.component.html**
```TypeScript
<div class="card border-primary text-center" style="width: 40%; margin: 10px auto auto;">
    <div class="card-header">{{titulo}}</div>
    <div class="card-body">
  
      <form method="post"> 
        <div class="form-group col-sm-6">
          <input [(ngModel)]="usuario.username" type="text" class="form-control"
          name="username" id="username" placeholder="Username" autofocus style="display:inline; width:400px;"  required>
        </div>
  
        <div class="form-group col-sm-6">
          <input [(ngModel)]="usuario.password" type="password" class="form-control"
          name="password" id="password" placeholder="Password" style="display:inline; width:400px; margin-top: 10px;" required>
        </div>
  
        <div class="form-group col-sm-6">
          <input (click)='login()' type="submit" class="btn btn-lg btn-primary btn-block" style="display:inline; width:400px; margin-top: 10px;" value="Login">
        </div>
      </form>
    </div>
  </div>

```
