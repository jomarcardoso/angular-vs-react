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

