# worker chat

## Durable Object

DO를 활용하면 서비스에서 지속적이고 공통적으로 사용하는 데이터를 사용할 수 있음. 모든 서버에서 연결되는 데이터임  
즉 모든 workers에서 공통된 state를 사용할 수 있게 됨.

```toml
[durable_objects]
bindings = [
    {name = "COUNTER", class_name = "CounterObject"}
]
```

wrangler.toml 파일에 DO를 추가해준다.

```ts
export interface Env {
  COUNTER: DurableObjectNamespace;
}
```

위와 같이 interface에 추가해줘서 타입스크립트의 도움을 받을 수 있다.

```ts
export class CounterObject {
  counter: number;
  constructor() {
    this.counter = 0;
  }

  async fetch(request: Request) {
    const { pathname } = new URL(request.url);
    switch (pathname) {
      case "/":
        return new Response(this.counter);
      default:
        return handleNotFound();
    }
  }
}
```

위와 같은 형태로 DO를 만들면 된다. 사용자는 직접 DO를 건드릴 수 있다. fetch를 통해서 바꿔야함

> In order to use Durable Objects, you must agree to pricing
