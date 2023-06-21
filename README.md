# Angular VS React

## Bind event

```ts
@Component({
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
