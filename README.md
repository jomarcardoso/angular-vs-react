# Angular vs React comparison

I work with both frameworks and I brought some data and codes for comparisons.

The conclusion is yours.

## Downloads

Weakly downloads

- React: 17.399.094
- Angular: 2.831.767

This numbers might be incorrectly very faborable to React, but it doesn't consider private NPM registries, like Artifactory.
Angular is supposed to be more used in corporate environments.

## [State of JavaScript 2022](https://2022.stateofjs.com/en-US/libraries/front-end-frameworks/)

![image](https://github.com/jomarcardoso/angular-vs-react/assets/27368585/5c7a13e3-48ff-476b-a6dc-841ad2509f6f)

## Community

React has many libraries.

Angular is a complete framework.

## Starters

|         | Angular           | React                          |
| ------- | ----------------- | ------------------------------ |
| static  | Angular Core, NX  | Create React App, Gatsby, Vite |
| dynamic | Angular Universal | Next.js                        |

## Client side use

How do they run without a build on the client side.

### Angular

Angular JS or nothing.

### React

React Use a build to transpile the JSX, but it starts in any root node and does not need a build.

```html
<!-- example from https://github.com/jomarcardoso/quickly-how-react-works/tree/main/projetos/2-prehistory-react -->
<html>
  <body>
    <div id="root"></div>
    <script crossorigin src="https://unpkg.com/react@17/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
    <script>
      function Input({ value = '', onChange }) {
        return React.createElement('input', {
          onChange,
          value,
        });
      }
      
      function App() {
        const [value, setValue] = React.useState('');
      
        function handleChange(event) {
          setValue(event.target.value);
        }
      
        return React.createElement(
          React.Fragment,
          null,
          React.createElement(Input, { value, onChange: handleChange }),
          React.createElement('p', null, value)
        );
      }
      
      ReactDOM.render(
        React.createElement(App, null),
        document.getElementById('root')
      );
    </script>
  </body>
</html>
```

## Syntax

### Angular

```ts
@Component({
  selector: 'app-title',
  template: '<h1 class="title"><ng-content></ng-content></h1>',
  styles: ['.title { font-weight: bold; }'],
  standalone: true,
})
class TitleComponent {}
```

### React

```tsx
const styles = { fontWeight: 'bold' };

function Title() => <h1 style={styles}>{children}</h1>;
```

---

## Bind event

### Angular

```ts
@Component({
  selector: 'app-root',
  template: ` <button (click)="onClick($event)">common button</button> `,
})
class AppComponent {
  onClick(event: Event) {
    console.log(event);
  }
}
```

### React

```tsx
function App() {
  const handleClick = useCallback<MouseEventHandler<HTMLButtonElement>>(
    (event) => console.log(event),
    [],
  );

  return <button onClick={handleClick}>common button</button>;
}
```

## State

### Angular

```ts
@Component({
 selector: 'app-root',
  template: `
    <div>{{ state }}</div>
    <button (click)="state += 1">increase button</button>
  `,
})
class AppComponent {
  state = 1;
}
```

### React

```tsx
function App() {
  const [state, setState] = useState();

  return (
    <>
      <div>{state}</div>
      <button onClick={() => setState(state + 1)}>increase button</button>
    </>
  );
}
```

## Life cycles

### Angular

```ts
@Component({ selector: 'app-root' })
class AppComponent
  implements
    OnChanges,
    OnInit,
    DoCheck,
    AfterContentInit,
    AfterContentChecked,
    AfterViewInit,
    AfterViewChecked,
    OnDestroy
{
  ngOnChanges() {}
  ngOnInit() {}
  ngDoCheck() {}
  ngAfterContentInit() {}
  ngAfterContentChecked() {}
  ngAfterViewInit() {}
  ngAfterViewChecked() {}
  ngOnDestroy() {}
}
```

### React

```tsx
function App(newState) {
  useEffect(() => {
    // run once
    return () => {
      // on destroy
    }
  }, []);

  useEffect(() => {
    // run every change
  }, [newState]);

  return <></>;
}
```

## Forwarded attributes

Components or templates with other elements or components inside.

### Angular

```ts
// directive breaks the CSS encapsulation.
@Directive({ selector: '[appInput]' })
class InputDirective {
  @Input()
  @HostBinding('type')
  type = 'text';

  @HostBinding('class')
  className = 'input';
}

@Component({
  selector: '[appField]',
  template: `
    <ng-content select="label"></ng-content>
    <ng-content></ng-content>
  `,
})
class FieldComponent {
  @Input()
  @HostBinding('class.field')
  rootClass = true;
}

@Component({
  selector: 'app-root',
  template: `
    <div appField id="age-field">
      <label>age</age>

      <input appInput type="number" />
    </div>
  `,
})
class AppComponent {}
```

### React

```tsx
function Field({ inputProps, children, labelProps, ...props }) {
  return (
    <div className="field" {...props}>
      <label {...labelProps}>{children}</label>
      <input type="text" className="input" {...inputProps} />
    </div>
  );
}

function App() {
  return (
    <Field id="age-field" inputProps={{ type: 'number' }}>
      age
    </Field>
  );
}
```

## Composition and Inheritance

### Angular

```ts
interface ButtonApi {
  type: string;
  id: string;
}

@Component({
  selector: 'app-button',
  template: `
    <button [class]="className" [id]="id" [type]="type">
      <ng-content></ng-content>
    </button>
  `
})
class ButtonComponent implements ButtonApi {
  @Input()
  set className(className) {
    this._className = className;
  }
  get className() {
    return `button ${this._className}`;
  }
  protected _className = 'button';

  @Input()
  id = '';

  @Input()
  type = '';
}

// inheritance
@Component({
  selector: 'app-secondary-button',
  // limimitation: the HTML structure needs to be the same
  templateUrl: 'app-button.html'
})
class SecondaryButtonComponent extends ButtonComponent {
  get className() {
    return super.className + ' secondary';
  }
}

// composition
@Component({
  selector: 'app-secondary-button',
  templateUrl: `
    <app-button class="secondary" [id]="id" [type]="type">
      additional content
      <ng-content></ng-content>
    </app-button>
  `
})
class SecondaryButtonComponent implements ButtonApi {
  @Input()
  id = '';

  @Input()
  type = '';
}
```

### React

```tsx
function Button({ children, className, ...props }) {
  return (
    <button className={`button ${className}`} {...props}>
      {children}
    </button>
  );
}

function SecondaryButton({ children, ...props }) {
  return (
    <Button className="secondary" {...props}>
      additional content
      {children}
    </Button>
  )
}
```

## Share state between components

### Angular

With services.

```ts
@Injectable({
  providedIn: 'root',
})
export class MessageService {
  subject = new Subject('');

  send(message = '') {
    this.subject.next(message);
  }

  clear() {
    this.subject.next('');
  }
}

@Component({
  selector: 'app-home',
})
class HomeComponent {
  constructor(private messageService: MessageService) {}

  ngOnit() {
    this.messageService.send('welcome');
  }
}

@Component({
  selector: 'app-root',
})
class AppComponent {
  message = '';

  constructor(private messageService: MessageService) {
    this.messageService.subject.subscribe(
      (message) => (this.message = message),
    );
  }
}
```

### React

With Context API or [useReducer.](https://react.dev/reference/react/useReducer)

```tsx
const MessageContext =
  createContext<
    [message: string, setMessage: Dispacth<SetStateAction<string>>]
  >();

function Home() {
  const [message, setMessage] = useContext(MessageContext);

  useEffect(() => {
    setMessage('welcome')
  }, []);

  return <>{message}</>;
}

function AppContent() {
  const [message] = useContext(MessageContext);

  return <>{message}</>
}

function App() {
  const [message, setMessage] = useSate('');
  return (
    <MessageContext.provider value={[message, setMessage]}>
      <AppContent />
      <Home /> 
    </MessageContext.provider>
  )
}
```

## Refs

### Angular

```ts
@Component({
  selector: 'app-root',
  template: '<div #content></div>'
})
class AppComponent implements AfterViewInit {
  @ViewChild('content') content;

  ngAfterViewInit() {
    console.log(this.content.nativeElement);
  }
}
```

### React

```tsx
function App() {
  const ref = useRef();

  useEffect(() => {
    console.log(ref.current);
  }, []);

  return <div ref={ref} />;
}
```

## Two way binding

### Angular

```ts
@Component({
  selector: 'app-input',
  template: '<input [value]="value" (change)="onChange($event)">'
})
class InputComponent {
  @Input()
  value = '';

  @Output()
  valueChange = new EventEmitter();

  onChange(event) {
    this.valueChange.emit(event.currentTarget.value);
  }
}

@Component({
  selector: 'app-root'
  template: '<app-input [(value)]="state"></app-input>'
})
class AppComponent {
  state = '';
}
```

### React

```tsx
function Input({ state, setState, onChange }) {
  const handleChange = useCallback((event) => {
    if (onChange) {
      onChange(event);
    }

    setState(event.currentTarget.value);
  }, [setState, onChange]);

  return <input value={state} onChange={handleChange} />
}

function App() {
  const [state, setState] = useState('');
  
  return <Input value={state} setValue={setState} />;
}
```

## Manual listener

### Angular

```ts
@Component({
  selector: 'app-root',
})
class AppComponent implements AfterViewInit, OnDestroy {
  private readonly _destroy = new Subject();

  ngAfterViewInit() {
    fromEvent(window, 'scroll')
      .pipe(takeUntil(this._destroy))
      .subscribe((event) => {
        console.log(event);
      });
  }

  ngOnDestroy() {
    this._destroy.complete();
  }
}
```

### React

```tsx
function App() {
  const handleScroll = useCallback((event) => {
    console.log(event);
  }, []);

  useEffect(() => {
    window.addEventListener('scroll', handleScroll);

    return () => {
      window.removeEventListener('scroll', handleScroll);
    };
  });

  return <></>;
}
```

## Empty components

### Angular

Directives in Angular are components without HTML that put attributes to elements and can be used to comunicate between other components/directives with a service.

```ts
@Directive({
  selector: '[appCurrencyInput]',
})
class CurrencyInputDirective {
  @HostBinding()
  type = 'number';
}

@Component({
  selector: 'app-root',
  template: '<input appCurrencyInput>',
})
class AppComponent {}
```

### React

There's no directive in React, but we can do something similiar with "render props". And also use context and any hook to share data between other components.

```tsx
function CurrencyInput({ children, ...props }) {
  const type = 'number';

  return <>{children({ type, ...props })}</>;
}

function App() {
  return <CurrencyInput>{(props) => <input {...props} />}</CurrencyInput>;
}
```

## Typing

### Angular

```
class TooltipComponent implments OnInit {
  constructor(@Self @Optional() private ngControl: NgControl) {}

  ngOnInit() {}
}
```

### React

The typing in React is 100%, but boring and difficult.

```tsx
// examples
const setOpen: Dispatch<SetStateAction<boolean>>;

const Tooltip: FC<TooltipProps> => <></>;

const tooltipRef = useRef() as MutableRefObject<HTMLElement>;

const handleChange: FormEventHandler<HTMLInputElement> = (event) => {                                                                                                                                             
```

## Transform data in HTML

### Angular

Used pipe for this.

```ts
@Pipe({
  name: 'uppercase',
})
class UppercasePipe {
  transform(text = '') {
    return text.toUpperCase();
  }
}

@Component({
  selector: 'app-root',
  template: '<p>{{ Jomar | uppercase }}</p>',
})
class AppComponent {}
```

### React

There's no pipes in React because it's not necessary.

```tsx
// common reusable function
function toUpperCase(text = '') {
  return text.toUpperCase();
}

// you do not need documentation to understand this
function App() {
  return <p>{toUpperCase('Jomar')}</p>;
}
```

## Test

### Angular

```ts
import { createHostFactory, SpectatorHost } from '@ngneat/spectator';

describe('app-root', () => {
  const createHost = createHostFactory({
    component: AppComponent,
    declareComponent: false,
    imports: [AppModule],
  });

  it('should render app', () => {
    const { fixture } = createHost('<app-root></app-root>');

    expect(fixture).toMatchSnapshot();
  });
});
```

Personal opinion

- lots of workarounds to get almost 100% coverage
- verbose

### React

```tsx
import { render } from '@testing-library/react';

describe('<App />', () => {
  it('should render app', () => {
    const { container } = render(<App />);

    expect(container).toMatchSnapshot();
  });
});
```

Personal opinion

- easy to get 100% coverage
- intuitive

## CSS encapsulation

### Angular

Pros

- Only need to import 💯

Cons

- You don't override without `::ng-deep`

```ts
@Component({
  selector: '[appButton]',
  styleUrls: ['./button.component.css'],
  template: '<ng-content></ng-content>'
})
class Button {}
```

### React

Pros

- First level use is simple

Cons

- Override is boring because you need to send again the class `<Tag className={styles.tab} />`;
- Override childrens of children 😨 I don't know;
- Override pseudo-selectors 😵

```tsx
// with Create React App
import styles from './Button.module.css';

function Button({ children, props }) {
  return (
    <button className={styles.button} {...props}>
      {children}
    </button>
  );
}
```

## Lazy Load components

### [Angular](https://angular.io/guide/lazy-loading-ngmodules)

```ts
const routes: Routes = [
  {
    path: 'items',
    loadChildren: () => import('./items/items.module').then(m => m.ItemsModule)
  }
];
```

### [React](https://legacy.reactjs.org/docs/code-splitting.html)

```tsx
import React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';

const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </Suspense>
  </Router>
);
```

## Cache

### Angular

```ts
@Component({
  selector: 'app-field',
  changeDetection: ChangeDetection.onPush,
  template: `
    <div>
      <label><ng-content></ng-content>
      <input [value]="value" [id]="id | async">
    </div
})
class FieldComponent {
  id: string;

  @Input()
  value: string;

  constructor(private idService: IdServe) {
    idService.subject.subscribe((id) => this.id = id);
  }
}
```

### React

```tsx
import React from 'react';

function Field({ children, value }) {
  const id = useContext(IdContext);

  const memoizedLabel = useMemo(() => (
    <label>{children}</label>
  ), [children]);

  const memoizedInput = useMeno(() => (
    <input value={value} id={id} />
  ), [value, id])

  return (
    <div>
      {memoizedLabel}
      {memoizedInput}
    </div>
  );
}
```

