## Downloads

Weakly downloads

React: 17.399.094
Angular: 2.831.767

## [State of JavaScript 2022](https://2022.stateofjs.com/en-US/libraries/front-end-frameworks/)

![image](https://github.com/jomarcardoso/angular-vs-react/assets/27368585/5c7a13e3-48ff-476b-a6dc-841ad2509f6f)

## Community

React has many libraries.

Angular is a complete framework.

## Practicality

React starts every where with or without a build.

## Build

React does not need build. But has:

- Create React App
- Vite
- Gatsby
- Next

# Angular VS React

## Bind event

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

In Angular it breaks the CSS encapsulation.

```ts
@Directive({ selector: '[appInput]' })
class InputDirective {
  @Input()
  @HostBinding('type')
  type = 'text';
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

```tsx
function Field({ inputProps, children, labelProps, ...props }) {
  return (
    <div className="field" {...props}>
      <label {...labelProps}>{children}</label>
      <input type="text" {...inputProps} />
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
    <Button {...props}>
      additional content
      {children}
    </Button>
  )
}
```

## State machine

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

## Directive

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

```tsx
function CurrencyInput({ children, ...props }) {
  const type = 'number';

  return <>{children({ type, ...props })}</>;
}

function App() {
  return <CurrencyInput>{(props) => <input {...props} />}</CurrencyInput>;
}
```

## Service

## Pipe

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

```ts
@Component({
  selector: '[appButton]',
  styleUrls: ['./button.component.css'],
  template: '<ng-content></ng-content>'
})
class Button {}
```

### React

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

## Cache

useMemo


