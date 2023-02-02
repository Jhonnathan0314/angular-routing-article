[![N|Header](https://user-images.githubusercontent.com/73840306/191827738-862c8cc9-6b3a-4e67-8c9a-b1483a8ecef3.png)](https://www.linkedin.com/company/iassoftware/)
# Angular routing
![Banner](https://github.com/Jhonnathan0314/angular-routing-article/blob/main/images/Banner.png?raw=true)  
*Imagen tomada de: [Routing of an Angular Component | Angular Routing](https://javascript.plainenglish.io/routing-of-an-angular-component-angular-routing-2b7e53046542)*

## **1. Conceptos clave**

- ### **RouterModule**
    #### Agrega las directivas y proveedores para la navegación en la aplicación, el cual hace uso del servicio [***Router***](https://angular.io/api/router/RouterModule) de angular, este módulo tiene 2 métodos, [***forRoot(routes)***](https://angular.io/api/router/RouterModule#forRoot) y [***forChild(routes)***](https://angular.io/api/router/RouterModule#forChild).

- ### **forRoot(routes)**
    #### Se encarga de crear y configurar un **módulo** con las rutas y directivas definidas haciendo uso del [***Router service***](https://angular.io/api/router/Router), permitiendo que la aplicación pueda tener dicha información. Es usado en el módulo principal de la aplicación.

- ### **forChild(routes)**
    #### Se encarga de crear y configurar un **módulo** con las rutas y directivas definidas sin hacer uso del Router service, por lo que será incluido en los módulos (creados manualmente) que se llaman en el módulo principal.

- ### **Router service**
    #### Es un servicio encargado de proveer la navegación entre vistas y URLs. Tiene métodos para obtener la navegación actual, reiniciar su configuración, navegar entre URLs, serializar URLs, entre otros.

- ### **Route interface**
    #### Es la configuración de un objeto que define una sola ruta, recibida en forma de Array desde el archivo ***app-routing.module.ts***, en esta interfaz se encuentran definidos los atributos que se pueden enviar desde el archivo nombrado anteriormente, tal como [**path**](https://angular.io/api/router/Route#path), [**pathMatch**](https://angular.io/api/router/Route#pathMatch), [**loadComponent**](https://angular.io/api/router/Route#loadComponent), [**redirecTo**](https://angular.io/api/router/Route#redirectTo), [**children**](https://angular.io/api/router/Route#children), [**loadChildren**](https://angular.io/api/router/Route#loadChildren), etc.


## **2. Definicion routing al crear proyectos o módulos**

- ### **Generación de routing al crear proyecto**
    #### Cuando creamos un proyecto angular por consola, esta misma nos pregunta si deseamos agregar angular routing al proyecto, por lo que debemos decirle que sí **(Y)**
    ~~~
    "? Would you like to add Angular routing? (y/N)": y
    ~~~

- ### **Generación de routing al crear un módulo**
    #### Cuando generamos un módulo haciendo uso del comando `ng generate module module-name` se genera un modulo sin routing, para generarlo con routing debemos agregar `--routing`, quedando de la siguiente manera: `ng generate module module-name --routing`, creando así tanto el módulo como el archivo routing-module.
    ~~~
    ng generate module pages/product --routing
    ~~~

## **3. AppRoutingModule**
#### Cuando se crea un proyecto en angular con el comando `ng new angular-routing` y generando Angular Routing, se crea un proyecto de angular con el módulo principal, el componente principal y el App Routing Module principal de la aplicación.
![App component](https://github.com/Jhonnathan0314/angular-routing-article/blob/main/images/appComponent.png?raw=true)

#### Suponiendo un proyecto con la estructura
![Project structure](https://github.com/Jhonnathan0314/angular-routing-article/blob/main/images/projectStructure.png?raw=true)
#### En donde create, product-all, update son componentes que pertenecen al módulo product y login un componente independiente (Stand Alone).

#### El archivo ***app-routing.module.ts*** tiene una estructura compuesta por:
- Importaciones
- Variable **routes** que contiene las rutas definidas en el módulo principal
- Decorador [**@NgModule**](https://angular.io/api/core/NgModule), encargado de importar y exportar el Router Module.
- Exportación de la clase AppRoutingModule

    ### **a. Variable routes**
    #### Esta variable se define de tipo ***Routes***, este tipo de dato representa una configuración de ruta para el servicio de router. Genera una matriz de ***Router Objects***, utilizada en Router.config y, también es usada para configuraciones de rutas anidadas en ***Route.children***.

    #### El contenido de la variable ***routes*** tiene una estructura de un Array de Objetos, en donde tenemos diferentes usos de la misma tal como:

    - #### **Redirigir a componentes**: si queremos que una ruta nos dirija a un componente puntual, podemos hacer uso de la siguiente sintaxis:
        ~~~typescript
        const routes: Routes = [
            { path: 'login', component: LoginComponent }
        ]
        ~~~

    - #### **Redirigir a otro módulo que contenga un routing module**: en caso de tener un módulo con sus propios componentes, en donde tenemos definido ya un enrutamiento orientado a la redirección de componentes podemos referenciar dicho módulo de la siguiente forma:
        ~~~typescript
        const routes: Routes = [
            { path: 'login', component: LoginComponent },
            { path: 'product', loadChildren: () => import('./pages/product/product.module').then(m => m.ProductModule) }
        ]
        ~~~

    - #### **Redirección para evitar URL inexistente o vacía**: angular permite prevenir el error 404 Not Found, gracias a especificar a dónde nos debemos dirigir en caso de rutas vacías o rutas que no están definidas en la aplicación, por medio de las siguientes rutas:
        ~~~typescript
        const routes: Routes = [
            { path: 'login', component: LoginComponent },
            { path: 'product', loadChildren: () => import('./pages/product/product.module').then(m => m.ProductModule) },
            { path: '', redirectTo: 'login', pathMatch: 'full' },
            { path: '**', redirectTo: 'login', pathMatch: 'full' }
        ]
        ~~~

    ### **b. @NgModule**
    #### El decorador [**@NgModule**](https://angular.io/api/core/NgModule) importa el [**RouterModule**](https://angular.io/api/router/RouterModule) especificado para la variable definida anteriormente y, finalmente exporta el mismo RouterModule.
    ~~~typescript
    @NgModule({
        imports: [RouterModule.forRoot(routes)],
        exports: [RouterModule]
    })
    ~~~


## **4. Angular Directives**
- #### **RouterLink**: Según la ruta dada, permite crear y redireccionar a una ruta estática, la cual parte de una ruta dinámica. Por ejemplo, al tener un módulo que controla los componentes correspondientes a un CRUD,el componente actualizar está compuesto de manera dinámica ya que, al recibir un parámetro esta varía según el mismo:
    #### El correcto uso de esta directiva es, por ejemplo, al tener una etiqueta  **`<a>Actualizar producto</a>`** en código HTML, se agrega el parámetro **`[routerLink]=”[‘ruta’]”`**
    ~~~html
    <a [routerLink]="['/product/update/12345']">Actualizar producto</a>
    ~~~

    #### Adicionalmente, podemos enviar los parametros necesarios en la ruta por medio del atributo `[queryParams]=”{atributo: valor}”`
    ~~~html
    <a [routerLink]="['/product/update/12345']" 
       [queryParams]="{detail: true}">
       
       Actualizar producto</a>
    ~~~

- #### **RouterLinkActive**: En caso de que la ruta definida se encuentre activa, permite especificar una o varias clases de CSS, las cuales se agregaran cuando la ruta se encuentra activa. Puede usarse de 2 maneras diferentes:
    ~~~html
    <a [routerLink]="['/product/update/12345']"
       routerLinkActive="class1 class2">
    
       Actualizar producto</a>
    ~~~
    ~~~html
    <a [routerLink]="['/product/update/12345']"
       [routerLinkActive]="['class1', 'class2']">
    
       Actualizar producto</a>
    ~~~

    #### En caso de querer que la ruta sea completamente específica (id estático, no dinámico), se agrega el parámetro `[routerLinkActiveOptions]=”{exact: true}”`
    ~~~html
    <a [routerLink]="['/product/update/12345']"
       [routerLinkActive]="['class1', 'class2']"
       [routerLinkActiveOptions]="{exact: true}">
    
       Actualizar producto</a>
    ~~~

- #### **RouterOutlet**: funciona como un marcador de posición, el cual se visualiza según la ruta activa. Su uso más común en el el componente principal, visualizando header y footer de manera fija, mientras que su contenido depende de la ruta activa
    ~~~html
    <app-header></app-header>
    
    <router-outlet></router-outlet>

    <app-footer></app-footer>
    ~~~


## **5. Angular objects**
- **Router**: permite la navegación de una vista a la siguiente a medida que los usuarios realizan las tareas de la aplicación.

- **ActivatedRoute**: proporciona acceso a información sobre una ruta asociada a un componente cargado en una salida. Se utiliza para recorrer el árbol RouterState y extraer información de los nodos tal como Path Variables, parent, children, entre otros.

#### Un ejemplo de uso para cada objeto mencionado requiere primeramente su llamado en el método constructor, quedando de la siguiente forma:
~~~typescript
constructor(private router: Router, private activatedRoute: ActivatedRoute) {}
~~~

#### Para la variable de tipo **Router**, podemos usarla de diferentes formas, las 2 mas comunes para cambiar de ruta son:
~~~typescript
this.router.navigate(["/product/all"]);
this.router.navigateByUrl('/product/all');
~~~

#### Para la variable de tipo **ActivatedRoute**, podemos obtener los query params de la siguiente manera (existen diferentes métodos para obtenerlos, este es uno de ellos).
~~~typescript
this.activatedRoute.paramMap.subscribe(id => {
    console.log(id.get('_id'));
})
~~~

> ### **Jonatan Fernando Franco Cardenas**  
> #### Desarrollador de software  
> #### [I.A.S. Ingeniería, Aplicaciones y Soluciones S.A.S.](https://www.ias.com.co/)
> [![Linkedin logo](https://cdn-icons-png.flaticon.com/32/174/174857.png)](https://www.linkedin.com/in/jonatan-fernando-franco-cardenas-06b742251/)

en [BlogIAS](https://www.ias.com.co/blog)
