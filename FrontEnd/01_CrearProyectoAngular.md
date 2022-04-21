# Crea proyecto de angular


## 1. Crear directorio del proyecto de front 

![image](https://user-images.githubusercontent.com/31961588/164351559-1f100a89-59cb-4c0c-8e9c-1de9c54e7fdf.png)

**Abrir el visual code en la opción de open folder**

![image](https://user-images.githubusercontent.com/31961588/164351651-45b9d0d7-0b73-4b43-8d98-711741592d5b.png)


**Seleccionar el directorio creado para el proyecto. Ya tenemos nuestro visual code relacionado con el proyecto.**

![image](https://user-images.githubusercontent.com/31961588/164351755-cc742d1b-b179-493f-a35a-30419e656345.png)

## 2. Abrir una terminal en el visual code y verificar que tenemos todo lo necesario para crear un proyecto de angular

**Abrir una terminal en el visual code**

![image](https://user-images.githubusercontent.com/31961588/164351956-3b9b9642-0994-4170-9bcd-bd10cb037231.png)

**Verificamos que este instaldo: nodejs, npm, git, cli angular. El  proceso de instalación se debe hacer previamente, su ambiente puede tener versiones más recientes y no es problema para el desarrollo de este proyecto.**

![image](https://user-images.githubusercontent.com/31961588/164352510-75872ae0-0102-4f2a-9828-ef5e24a4e5de.png)

**Observe el ambiente que tiene el angular listo para ser usado**

![image](https://user-images.githubusercontent.com/31961588/164352823-a1588fa3-cd73-4367-8b07-f6fc5eba8584.png)

## 3. Crear el proyecto de angular

**Con el comando ng new creamos nuestro proyecto seguido del nombre que le vamos a dar al proyecto. En este caso se va llamar invoice-uts**

![image](https://user-images.githubusercontent.com/31961588/164353034-353785ea-a7cd-4579-a516-f7e41c9d84eb.png)

**Yes para crear el modulo de routing, no ha problema si damos no, lo podemos hacer posterior a la creación del proyecto**

![image](https://user-images.githubusercontent.com/31961588/164353213-7c8c5ae3-83a7-4d53-a1c8-1ae55068c77f.png)

**Selecionamos css para el tipo de estilos a usar en nuestro proyecto**

![image](https://user-images.githubusercontent.com/31961588/164353279-99a1c4e3-bc9d-4dcf-b825-8b1845ce718f.png)

**Procede a conectarse al repositorio npm para descagar todas las dependencias del proyecto. Toma su tiempo y depende de su conexión a internet**

![image](https://user-images.githubusercontent.com/31961588/164353403-233f4626-a68d-4ee3-a36b-c8652fa5a63f.png)


**Se ha creado nuestro proyecto**

![image](https://user-images.githubusercontent.com/31961588/164353845-14aaa262-cc79-418e-a4dd-00bb2601392c.png)

**Subir el servicio del proyecto. Ingresamos al directorio del proyecto y ejecutamos el comando ng server**

![image](https://user-images.githubusercontent.com/31961588/164354095-84d65e26-2655-44fa-9cb6-3492c50fae0a.png)

**Inicio correctamente nuestra app**

![image](https://user-images.githubusercontent.com/31961588/164354248-b8d5f014-452d-45e8-8c9b-ee5a60e82ed4.png)

**Abrimos el navegador en http://localhost:4200/

![image](https://user-images.githubusercontent.com/31961588/164354371-f8d36243-be59-4964-8594-13d7f64c7328.png)


## 4. Proyecto angular es un SPA (Single Page Aplication)

**Todo lo que se haga se va cargar en <app-root> del index.html del proyecto**

![image](https://user-images.githubusercontent.com/31961588/164354671-1c1dbb8e-823d-4d9a-a22e-486b738b7293.png)

**Todo proyecto de angular va tener un componete principal que es el app.component. Angular construye aplicaciones a traves de componentes y un componente por lo generar cuenta de los siguientes tipo de  archivos:  .html, .ts, .css, y spect.ts como se observa.**
  
  ![image](https://user-images.githubusercontent.com/31961588/164355024-4b2688aa-feb6-4947-8f79-4657d7fcb2b9.png)
  
 **El app.module es  un archivo de configuración muy importante para un proyecto en agular, el cual, se va utilizar mucho a medida que se van creando componentes**
  
  ![image](https://user-images.githubusercontent.com/31961588/164355384-d5fee6d1-ee46-47c5-8066-a99d9cbfb814.png)
  
 **El app-routing.module.ts es el encargado de configurar las rutas que va tener nuestro proyecto, es decir del enrutamiento.**
  
  ![image](https://user-images.githubusercontent.com/31961588/164355538-46f811b7-14bd-4fe4-ad46-31a3c10ea9c2.png)

## 5. Crear un hola mundo en nuestro app.component.html 
  
  ![image](https://user-images.githubusercontent.com/31961588/164356070-61b52e98-be06-4b97-958f-f7f756ac98f4.png)

  
  ![image](https://user-images.githubusercontent.com/31961588/164355972-f95690d1-a996-4641-9a45-ff7ca16039e3.png)

  
  







