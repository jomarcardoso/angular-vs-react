# Angular VS React

## State and bind

```ts
@Component({
  template: `
    <div>{{ state }}</div>
    <button (click)="state += 1">
      increase button
    </button>
  `
})
class AppComponent {
  state = 1;
}
```

```ts
function Component() {
  const [state, setState] = React.useState();

  return (
    <>
      <div>{state}</div>
      <button onClick={() => setState(state + 1)}>
        increase button
      </button>
    </>
}
```
