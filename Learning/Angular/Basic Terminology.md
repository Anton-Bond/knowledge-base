create a new project
```bash
ng new <project_name> --strict false --routing false --standalone false
```

create a new component without test file
```shell
 ng g c <component_name> --skip-tests
```

decorator
class component
AppModule
NgModule decorator
Component decorator
main.ts -> bootstrap
Standalone compnent
Databinding (business logic <-> template)
	-->
	string interpolation {{ data }}
	Property binding [property]="data"
	<--
	event binding (event)="expression"
	<-->
	Two-way-binding [(ngMode)]="data"
ngModel  directive. This is done by adding the FormsModule  to the imports[] array in the AppModule.
directives (-- instraction in the DOM)
@Input('alias_name')
EventEmitter<>()
@Output('alias_name')
View Encapsulation
#refElement -- local reference
@ViewChild()
	Instead of:
	@ViewChild('serverContentInput') serverContentInput: ElementRef;
	use
	@ViewChild('serverContentInput', {static: true}) serverContentInput: ElementRef;
	 IF you plan on accessing the selected element inside of ngOnInit()
<ng-content>
<ng-template>
Lifecycle
OnInit lificycle has a problem with refElements, AfterViewInit - not 
@Directive
ElemnentRef
Renderer2 and it's methods
@HostListener
@HostBinding
TemplateRef
ViewContainerRef
[ngSwitch] *ngSwitchCase *ngSwitchDefault
Service
inject() function
providers: [serviceName] - create a new servise instance
@Injectable() -- use service inside of another service
 "new syntax" @Injectable({providedIn: 'root'}) -- Instead of adding a service class to the providers[]  array in AppModule
<router-outlet>
routerLink="/users"
[routerLink]="['/users', 'something']"
routerLinkActive="class"
routerLinkActiveOptions="{exact: true}"
router: Router
route: ActivatedRoute
router.navigate([], {relativeTo: this.route})
this.route.snapshot.params['id']
this.route.params.subscribe((params: Params) => {})
 do not forget: ngOnDestroy() { this.paramsSubscription.unsubscribe() }
this.route.snapshot.data['message']
queryParamsHandling: 'preserve'
{ path: '', redirectTo: '/somewhere-else', pathMatch: 'full' }
CanActivate
ActivatedRouteSnapshot
RouterStateSnapshot
CanActivateChild
interface
CanDeactivate
RxJS
  interval()
  Observable
  Subscription
Operators
  pipe()
  map()
  filter()
  tap()
  take()
  exhaustMap()
  ...
new Subject() -- use it instead of EventEmiter (but steel use it in @Output())
Forms
ReactiveFormsModule
Pipes -- transform output for view
HttpClientModule
HttpClient
catchError
Interceptors
  HttpInterceptor
  HTTP_INETRCEPTORS
BehaviorSubject
Dynamic Component Loader
ComponentFactoryResolver
