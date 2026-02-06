# Arrays

- `[1, 2, 3]` 와 같은 배열을 명시하기 위해서는 `number[]` 구문을 사용할 수 있다.
- `변수타입[]` 으로 배열 타입을 선언한다.
- `Array<변수타입>` (예를 들어 Array<number> 과 같은 형식으로도 나타낼 수 있다.)

# any

- any 타입으로 선언된 변수에는 어떤 값이 들어와도 타입 에러가 발생하지 않는다.
- 만일 변수에 타입을 선언하지 않고, 타입스크립트가 context로 부터 타입 추론을 할 수 없을 경우 컴파일러는 기본적으로 any 타입을 지정하여 컴파일한다.
    - `noImplicitAny` flag를 사용하면 이런 묵시적 any를 수용하지 않도록 할 수 있다.
        - 이 경우 명시적으로 any를 선언하는 경우를 제외한 묵시적 any가 발생할 경우 에러를 일으킨다.

# Type Annotations on Variables

- `const`, `var`, `let` 등을 사용하여 변수를 선언할 경우 해당 변수의 타입을 함께 선언해줄 수 있다.
    - `let myName : string = "Alice"`
- 하지만 대부분의 경우 이는 불필요한데, 타입 스크립트가 자동적으로 타입 추론을 해주기 때문이다.
    - 기본적으로 가장 처음 초기화된 값의 타입으로 지정된다.

# Functions

- 함수의 input과 ouput에 타입을 명시해야 한다.

### Parameter Type Annotations

- 파라미터 이름 뒤에 해당 파라미터의 타입을 명시한다.
    
    ```jsx
    function greet(name : string) {
    	console.log("Hello, " + name.toUpperCase() + "!!");
    }
    ```
    

### Return Type Annotations

- 파라미터 리스트의 뒤에 함수의 반환 타입을 명시한다.
    
    ```jsx
    function getFavoriteNumber(): number {
    	return 5;
    }
    ```
    
    - 반환 타입은 반드시 명시할 필요는 없는데, `return` 문을 보고 타입 스크립트가 자동적으로 타입 추론을 해주기 때문이다.

**Functions Which Return Promises**

- promise를 반환하는 함수의 반환 타입을 명시할 때는 `Promise` 타입을 사용해야 한다.
    
    ```jsx
    async function getFavoriteNumber(): Promise<number> {
      return 26;
    }
    ```
    

## Anonymous Functions

- 타입 스크립트가 맥락적으로 파라미터의 타입을 추론해낼 수 있을 경우, 파라미터 타입 선언은 해주지 않아도 된다.
    
    ```jsx
    const name = ["Alice", "Bob", "Eve"];
    
    names.forEach(function (s) { // 맥락적으로 s는 string 타입임을 추론할 수 있음
    	console.log(s.toUpperCase());
    });
    ```
    

# Object Types

객체 타입을 정의하기 위해서는, 속성 이름과 그 타입을 나열해주면 된다.

- 각 속성은 `,` `;` 으로 구분한다. 마지막 속성 뒤에는 구분자를 명시하지 않아도 상관없다.
- 각 속성 타입은 반드시 적어줄 필요는 역시 없으나, 이 경우 묵시적으로 `any` 타입이 된다.

```jsx
// The parameter's type annotation is an object type
function printCoord(pt: { x: number; y: number }) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
printCoord({ x: 3, y: 7 });
```

## Optional Properties

객체에서, 어떤 속성은 있을 수도, 없을 수도 있다. (Optional)

이 경우 속성명 뒤에 `?` 를 붙여준다.

```jsx
function printName(obj: { first: string; last?: string }) {
  // ...
}
// Both OK
printName({ first: "Bob" });
printName({ first: "Alice", last: "Alisson" });
```

- 이렇게 optional properties로 명시한 값은 쓰기 전에 undefined 인지 아닌지 확인하는 작업이 필요하다. 만일 확인하지 않는다면 에러가 발생한다.
    
    ```jsx
    function printName(obj: {first: string; last?: string}) {
        console.log(obj.last.toUpperCase()); // 에러 발생
        if (obj.last !== undefined) {
            console.log(obj.last.toUpperCase()); // ok
        }
        console.log(obj.last?.toUpperCase()); // ok
    }
    ```
    

# Union Types

## Defining a Union Type

- `여러 타입들 중 하나` 가 값으로 들어올 수 있음을 나타낼 수 있다.
- 나열된 타입들 중 하나의 타입이 들어오면 된다.
    
    ```jsx
    function printId(id: number | string) {
      console.log("Your ID is: " + id);
    }
    // OK
    printId(101);
    // OK
    printId("202");
    ```
    
    - 가독성을 위해서 첫번째 타입 앞에 `|` 를 적어도 무방함
        
        ```jsx
        function printTextOrNumberOrBool(
          textOrNumberOrBool:
            string
            | number
            | boolean
        ) {
          console.log(textOrNumberOrBool);
        }
        
        function printTextOrNumberOrBool(
          textOrNumberOrBool:
            | string
            | number
            | boolean
        ) {
          console.log(textOrNumberOrBool);
        }
        ```
        

## Working with Union Types

- Union 타입으로 정의된 변수의 경우, 나열된 타입에 공통적으로 존재하는 함수만을 사용할 수 있다.
    
    ```jsx
    function printId(id: number | string) {
      console.log(id.toUpperCase()); // 에러 발생. number 타입에는 없는 함수
    }
    ```
    
- 이를 해결하기 위해서는 코드 단에서 해당 변수의 타입을 확인해주어야 한다.
    
    ```jsx
    function printId(id: number | string) {
      if (typeof id === "string") {
        // In this branch, id is of type 'string'
        console.log(id.toUpperCase());
      } else {
        // Here, id is of type 'number'
        console.log(id);
      }
    }
    ```
    
- 배열임을 확인하기 위해서는 `Array.isArray` 메서드를 사용하면 된다.
    
    ```jsx
    function welcomePeople(x: string[] | string) {
      if (Array.isArray(x)) {
        // Here: 'x' is 'string[]'
        console.log("Hello, " + x.join(" and "));
      } else {
        // Here: 'x' is 'string'
        console.log("Welcome lone traveler " + x);
      }
    }
    ```
    

# Type Aliases

- 같은 타입을 여러 변수에 지정하고 싶다면? 해당 Type을 따로 빼서 정의한 후, 여러 변수에 선언할 수 있다.
    
    ```jsx
    type Point = {
    	x: number;
    	y: number;
    }
    
    function printCoord(pt: Point) {
      console.log("The coordinate's x value is " + pt.x);
      console.log("The coordinate's y value is " + pt.y);
    }
    
    printCoord({x: 12, y: 13});
    ```
    
- 객체 뿐만 아니라 어떤 타입의 정의에도 type alias를 사용할 수 있다.
    
    ```jsx
    type ID = number | string;
    ```
    

# Interfaces

- 인터페이스 선언은 **객체의 타입을 정의하는 다른 방법**이다.
    
    ```jsx
    interface Point {
      x: number;
      y: number;
    }
     
    function printCoord(pt: Point) {
      console.log("The coordinate's x value is " + pt.x);
      console.log("The coordinate's y value is " + pt.y);
    }
     
    printCoord({ x: 100, y: 100 });
    ```
    

## Differences Between Type Aliases and Interfaces

- interface가 하는 것들을 type에서도 거의 대부분 똑같이 기능한다.
- 둘이 가장 다른 점은, **interface는 최초 선언 후 새로운 필드를 계속해서 추가할 수 있는 반면에, type의 경우 불가능하다는 것이다.**
    - interface
        
        ```jsx
        interface Window {
          title: string;
        }
        
        interface Window {
          ts: TypeScriptAPI;
        }
        
        const src = 'const a = "Hello World"';
        window.ts.transpileModule(src, {});
        ```
        
    - type
        
        ```jsx
        type Window = {
          title: string;
        }
        
        type Window = {
          ts: TypeScriptAPI;
        }
        
         // Error: Duplicate identifier 'Window'.
        ```
        

# Type Assertions

- 타입스크립트 컴파일러에 **어떤 타입으로 분석되길 원하는지 힌트를 제공**하는 수단이다.
    
    ```jsx
    const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;
    ```
    

### as vs. <>

- 원래 타입 표명으로 추가되었던 구문은 `<>` 꺽쇠였으나, 이런 스타일의 구문을 사용하면 JSX(TSX)에서 문법적으로 모호한 경우가 발생하여 `as` 구문을 추가하였다.
    
    ```jsx
    const myCanvas = <HTMLCanvasElement>document.getElementById("main_canvas");
    ```
    
- `as` 구문으로 타입 표명을 하는 것이 권장된다.

## Double assertion

- 다음은 정당한 사용 사례로, 타입 표명 구문이 의도한대로 동작한다.
    
    ```jsx
    function handler (event: Event) {
        let mouseEvent = event as MouseEvent;
    }
    ```
    
- 하지만 아래는 오류일 가능성이 높으며 사용자가 타입 표명을 하더라도 TypeScript는 에러를 발생시킨다.
    
    ```jsx
    function handler(event: Event) {
        let element = event as HTMLElement; // Error: Neither 'Event' nor type 'HTMLElement' is assignable to the other
    }
    ```
    
- 그래도 그 타입을 사용하고 싶을 경우 이중 표명을 사용할 수 있다.
    - 모든 타입과 호환되는 unknown 혹은 any로 타입 표명을 먼저 하면 된다.
        
        ```jsx
        function handler(event: Event) {
            let element = event as any as HTMLElement; // 오케이!
        }
        ```
        

> **기본적으로 S 타입이 T 타입의 하위 타입이거나 T가 S의 하위 타입이면, S에서 T로의 타입 표명이 성공한다.**
완전히 무작위적인 타입 표명을 하는데엔 위험이 따르고, 이를 위해선 이중 표명을 수행해야 한다.
