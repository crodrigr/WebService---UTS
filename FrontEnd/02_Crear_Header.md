# Crear header, instalar boostrap y estructura del proyecto. 

## 1. Instalar boostrap

**Boostrap tiene varias formas de instalación. Se va instalar CDN

![image](https://user-images.githubusercontent.com/31961588/164356943-653428cf-bfc9-4c42-831b-12dd04d31bad.png)

**En index.html de add los link de boostrat, jquery y de iconos**

![image](https://user-images.githubusercontent.com/31961588/164357151-179670c5-e7c2-4b86-9c75-782e80889f7a.png)

**Código**

```Html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>C4G8Facturador</title>
  <base href="/">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3" crossorigin="anonymous">
  
  

  <link rel="preconnect" href="https://fonts.gstatic.com">
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500&display=swap" rel="stylesheet">
  <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
</head>
<body class="mat-typography">
  <app-root></app-root>
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-ka7Sk0Gln4gmtz2MlQnikT1wXgYsOg+OMhuP+IlRH9sENBO0LRn5q+8nbTov4+1p" crossorigin="anonymous"></script> 
  <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.10.2/dist/umd/popper.min.js" integrity="sha384-7+zCNj/IqJ95wo16oMtfsKbZ9ccEh31eOz1HGyDuCQ6wgnyJNSYdrPa03rtR1zdB" crossorigin="anonymous"></script>
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.min.js" integrity="sha384-QJHtvGhmr9XOIpI6YVutG+2QOK9T+ZnN4kzFN1RtK3zEFEIsxhlmWl5/YESvpZ13" crossorigin="anonymous"></script>
  <script src="https://code.jquery.com/jquery-3.2.1.slim.min.js" integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN" crossorigin="anonymous"></script>
 
</body>
</html>
```

**Observamos que cambia el estilo de la fuente de nuesto proyecto, esta aplicando los estilos de boostrap **

![image](https://user-images.githubusercontent.com/31961588/164357421-a4ced7f3-a483-446a-9a00-2345485a4ba7.png)

## 2 Definir estilos de forma global para todo el proyecto

![image](https://user-images.githubusercontent.com/31961588/164357608-d32b56eb-f7e8-4e8c-bfec-c5ab22814d89.png)


```Css
/* You can add global styles to this file, and also import other style files */

html, body { height: 100%; }
body { margin: 0; font-family: Roboto, "Helvetica Neue", sans-serif; }

```

## 3. Creamos un folder en el proyecto para el componente header

![image](https://user-images.githubusercontent.com/31961588/164358080-d3ec885d-1ac7-4fdf-ac94-46511bbad1f7.png)

**Ingreso en la terminal al directorio creado "header" y a través del comando _ng generate component header_ creo el componente header. El _--flat_ es para que no me cree un directorio header, no es necesario lo  hicimos de forma manual y ya esta creado, si no colocamos el --flat va crea dentro de header otro directorio header**

![image](https://user-images.githubusercontent.com/31961588/164358535-867a652b-25e6-4b83-af2f-6a7580f0f512.png)

**Observer lo que hizo: crea tres archivos: header.component.html, header.component.spec.ts, header.component.ts, header.component.css y actuliza el app.module.ts**

![image](https://user-images.githubusercontent.com/31961588/164358790-e8b3fa19-17bd-4a8a-8537-3823f5091bcd.png)

## 4. Crear un nav bar en el header

**Buscamos un ejemplo de un nav bar en boostrap y lo copiamos**

![image](https://user-images.githubusercontent.com/31961588/164359439-5deb4691-3578-41a2-b1e6-d16600afd9b9.png)

![image](https://user-images.githubusercontent.com/31961588/164359638-2f4c7bf2-4868-4122-8e98-e4b918d79530.png)


**Código**

```Html
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Navbar</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <a class="nav-link active" aria-current="page" href="#">Home</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Link</a>
        </li>
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown" aria-expanded="false">
            Dropdown
          </a>
          <ul class="dropdown-menu" aria-labelledby="navbarDropdown">
            <li><a class="dropdown-item" href="#">Action</a></li>
            <li><a class="dropdown-item" href="#">Another action</a></li>
            <li><hr class="dropdown-divider"></li>
            <li><a class="dropdown-item" href="#">Something else here</a></li>
          </ul>
        </li>
        <li class="nav-item">
          <a class="nav-link disabled">Disabled</a>
        </li>
      </ul>
      <form class="d-flex">
        <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
        <button class="btn btn-outline-success" type="submit">Search</button>
      </form>
    </div>
  </div>
</nav>
```

## 5 app.component htlm usamos el componente header creado a través del selector <app-header>
  
  ![image](https://user-images.githubusercontent.com/31961588/164360228-0f8961fc-d3bd-46ec-8a9b-0874d2480999.png)

  ![image](https://user-images.githubusercontent.com/31961588/164360297-e3130834-7a91-4f9a-8dbc-e651988c5bf4.png)

**Código**
  
 ```Typescript
  <app-header></app-header>
<div class="container">
  <router-outlet></router-outlet>
</div>
```
