# Crear Interceptors

![image](https://user-images.githubusercontent.com/31961588/171081753-62ca4aa6-1af6-4704-a413-929b8e58cd83.png)

![image](https://user-images.githubusercontent.com/31961588/171081878-91d737ad-80dd-42f8-ac21-a20c70a2f63d.png)

![image](https://user-images.githubusercontent.com/31961588/171082325-798c8052-46fe-4fbe-9872-29fcfa945f66.png)

![image](https://user-images.githubusercontent.com/31961588/171082537-068ae072-03ab-47ec-bdf1-710d4ca95742.png)

**auth.intercepto.ts**
```TypeScript
import { Injectable } from '@angular/core';
import {
  HttpRequest,
  HttpHandler,
  HttpEvent,
  HttpInterceptor
} from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {

  constructor() {}

  intercept(request: HttpRequest<unknown>, next: HttpHandler): Observable<HttpEvent<unknown>> {
    return next.handle(request);
  }
}

```

**token.intercepto.ts**
```TypeScript
import { Injectable } from '@angular/core';
import {
  HttpRequest,
  HttpHandler,
  HttpEvent,
  HttpInterceptor
} from '@angular/common/http';
import { Observable } from 'rxjs';
import { AuthService } from '../auth.service';

@Injectable()
export class TokeInterceptor implements HttpInterceptor {

  constructor(private authService: AuthService) {}

  intercept(req: HttpRequest<any>, next: HttpHandler):
    Observable<HttpEvent<any>> {
    let token = this.authService.token;

    if (token != null) {
      const authReq = req.clone({
        headers: req.headers.set('Authorization', 'Bearer ' + token)
      });

      return next.handle(authReq);
    }

    return next.handle(req);
  }
 
}

```

## Configuración de app-routing

![image](https://user-images.githubusercontent.com/31961588/171085148-eb06bc82-03e4-467c-9b15-4cef868ca9fd.png)

## Configuración del app.module

![image](https://user-images.githubusercontent.com/31961588/171085263-29bd4e52-1621-4d86-8e97-2b731569f29f.png)

![image](https://user-images.githubusercontent.com/31961588/171085397-6e8fbea4-4bbe-4434-845c-c45b85e4e9e4.png)


