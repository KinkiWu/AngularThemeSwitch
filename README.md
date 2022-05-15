# Angular Material dark them switch

## Install Angular Material
```
ng add @angular/material
```
> You can install angular material through `NPM` and `Ng`.
If you install angular material `Ng`, it'll also help you import related moudle and component in `app.module.ts` and add material style to `style.scss`

`app.module.ts`
```ts
import { MatGridListModule } from '@angular/material/grid-list';
import { MatCardModule } from '@angular/material/card';
import { MatMenuModule } from '@angular/material/menu';
import { MatIconModule } from '@angular/material/icon';
import { MatButtonModule } from '@angular/material/button';
import { LayoutModule } from '@angular/cdk/layout';
import { MatToolbarModule } from '@angular/material/toolbar';
import { MatSidenavModule } from '@angular/material/sidenav';
import { MatListModule } from '@angular/material/list';
import {MatSlideToggleModule} from '@angular/material/slide-toggle';

@NgModule({
  declarations: [...],
  imports: [
    ...
    MatGridListModule,
    MatCardModule,
    MatMenuModule,
    MatIconModule,
    MatButtonModule,
    LayoutModule,
    MatToolbarModule,
    MatSidenavModule,
    MatListModule,
    MatSlideToggleModule
  ],
  providers: [],
  bootstrap: [AppComponent],
  exports: [DashboardComponent]
})
export class AppModule { }

```

`style.scss`
```scss
$Demo-theme: mat.define-light-theme((
  color: (
    primary: $Demo-primary,
    accent: $Demo-accent,
    warn: $Demo-warn,
  )
));

@include mat.all-component-themes($Demo-theme);
```

> You can see more angular material component in the following link:
https://material.angular.io/components/categories\

---
## Create dark them 
`style.scss` : create a `$Demo-dark-theme` variable 
```scss
$Demo-dark-theme: mat.define-dark-theme((
  color: (
    primary: $Demo-primary,
    accent: $Demo-accent,
    warn: $Demo-warn,
  )
));
```
Add `.dark-theme` and `.light-theme` class to include the different theme
```scss
.theme-light{
  @include mat.all-component-themes($Demo-theme);
}

.theme-dark{
  @include mat.all-component-themes($Demo-dark-theme);
}
```

## Binding variable to Host class property
`app.component.ts`
```ts
export class AppComponent {
  title = 'Demo';

  private isDark=false;

  @HostBinding('class')
  get themeMode(){
    return this.isDark ? 'theme-dark' : 'theme-light';
  }
}  

```


---
## Use slide toggle button to switch theme
- Add SlideToggle component
`app.module.ts`
```ts
import {MatSlideToggleModule} from '@angular/material/slide-toggle';

@NgModule({
  declarations: [
    ...
  ],
  imports: [
    ...
    MatSlideToggleModule
  ],
```
`navigation.component.html`
```html
<mat-slide-toggle (change)="onDarkModeSwitched($event)">Dark Mode</mat-slide-toggle>
```
- Binding change event to slide toggle, and add `@Output` property for parent component
```ts
export class NavigationComponent {

  @Output()
  readonly darkModeSwitched = new EventEmitter<boolean>();
  ...
  onDarkModeSwitched({checked}:MatSlideToggleChange){
    this.darkModeSwitched.emit(checked);
  }
   
}
```


- Add switch function and get  @Output EventEmitter<boolean>(); property from slide toggle component
`app.component.html`
```html
<app-navigation (darkModeSwitched)="switchMode($event)"></app-navigation>
```
`app.component.ts`
```ts
switchMode(isDarkMode:boolean){
    this.isDark = isDarkMode;
}
```



