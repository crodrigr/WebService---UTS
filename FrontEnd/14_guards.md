# Guards

![image](https://user-images.githubusercontent.com/31961588/171080528-0968a59b-5004-4eb1-8037-91b27a8972a1.png)

![image](https://user-images.githubusercontent.com/31961588/171080564-ce7ac621-a507-402c-bc07-1fe3d92fa811.png)

![image](https://user-images.githubusercontent.com/31961588/171080810-a4b8c925-5687-4219-a654-998497e5b297.png)

**auth.guards.ts**
```TypeScript
import { Injectable } from '@angular/core';
import { Router,ActivatedRouteSnapshot, CanActivate, RouterStateSnapshot, UrlTree } from '@angular/router';
import { Observable } from 'rxjs';
import { AuthService } from '../auth.service';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {

  constructor(private authService: AuthService,
    private router: Router) { }

    canActivate(
      next: ActivatedRouteSnapshot,
      state: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean {
      if (this.authService.isAuthenticated()) {
        if (this.isTokenExpirado()) {
          this.authService.logout();
          this.router.navigate(['/login']);
          return false;
        }
        return true;
      }
      this.router.navigate(['/login']);
      return false;
    }
    isTokenExpirado(): boolean {
      let token = this.authService.token;
      let payload = this.authService.obtenerDatosToken(token);
      let now = new Date().getTime() / 1000;
      if (payload.exp < now) {
        return true;
      }
      return false;
    }
    
}

```
