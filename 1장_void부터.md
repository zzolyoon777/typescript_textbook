### void는 JS에서와 달리 TS에서 타입으로 사용됩니다. 함수의 반환값을 무시하도록 하는 특수한 타입입니다. 사용자의 함수 반환값 사용을 억제할 수 있습니다.

---

### JS에서는 함수의 반환값이 없다면 undefined이지만, TS에서는 void입니다.

---

### 반환값을 무시하고 싶다면, 반환값 그 자체에 void를 적어야 합니다. 타입에만 적으면 오류만 발생합니다.

---

### {} 타입은, 객체가 아니라 null과 undefined를 제외한 모든 값을 의미합니다. Object 타입과 같습니다. 하지만 이는 값을 넣어도 사용할 수 없어서, 쓸모가 없는 타입입니다. 그저 알아두기만 하면 됩니다.

### {} 타입이 null과 undefined를 수용한다면 이는 unknown과 같아집니다. 실제로 조건문에서 unknown을 한 번 거르면 {} 타입이 나옵니다.
```typescript
const unk: unknown = 'hello';
if(unk) {
    unk; // const unk: {}
}
```

---

### never 타입에는 어떠한 타입도 대입할 수 없습니다. 함수 선언문에서는 never가 발생하지 않으므로, never가 발생할 상황에서는 직접 반환형을 적어 주어야 합니다.

---

### 자바스크립트에서 특정 값을 변수에 저장해서 변수의 이름으로 대신 사용하듯이, 타입스크립트에서는 특정 타입을 특정 이름(타입 변수)에 저장할 수 있습니다.
```typescript
type A = string;
const str: A = 'hello';

// and

const funcWithoutType: (value: number, unit: string) => string 
    = (value, unit) => value + unit;

type ValueWithUnit = (value: number, unit: string) => string;
const funcWithType: ValueWithUnit = (value, unit) => value + unit;
```

### 이를 기존 타입에 새로 이름을 붙였다고 해서, 타입 별칭이라고 부릅니다. type 키워드를 사용해서 선언할 수 있습니다. 이는 대문자로 시작해야 합니다.

### 객체나 배열도 가능합니다.
```typescript
type Person = {
    name: string,
    age: number,
    married: boolean
};

const person2: Person = {
    name: 'zero',
    age: 28,
    married: false
};
```

---

### 객체나 배열에 이름을 붙이는 또 다른 방법인 인터페이스가 있습니다.
```typescript
interface Person {
    name: string,
    age: number,
    married: boolean
};

const personWithInterface: Person = {
    name: 'justin',
    age: 21,
    married: false
};
```

### 함수나 배열도 가능합니다.
```typescript
interface Func {
    (x: number, y: number): number;
};

const add: Func = (x, y) => x + y;

// 배열

interface Arr {
    length: number;
    [key: number]: string; // length 프로퍼티를 제외한 모든 키가 number 타입이라는 뜻
};

const arr: Arr = ['3', '5', '7'];
```

### 인터페이스의 속성 키 자리에 [key: number]라는 문법이 있는데, 이는 이 객체의 length 프로퍼티를 제외한 속성 키가 전부 number라는 의미입니다. 이를 인덱스 시그니처(Index Signature)라고 부릅니다.

### 방금 만든 배열은 완전한 배열이 아니므로 메서드까지 사용할 수는 없습니다.

### 일반적으로, 객체의 속성 키는 문자열과 심볼만 가능하고, 다른 자료형의 값이 속성 키로 들어오면 JS에서 알아서 문자열로 바꿔서 사용합니다.

### 속성이 없는 인터페이스는, {} 타입과 동일하게 null과 undefined를 제외한 값을 대입할 수 있습니다. 이유는 일반적으로 인터페이스를 빈 객체로 선언할 일이 없기 때문입니다. 치장템과 같은 느낌이라고 생각합니다.

---

### 인터페이스는, 타입 별칭과는 달리 이름이 같은 인터페이스끼리는 서로 합쳐집니다. 이를 선언 병합(declaration merging)이라고 부릅니다. 선언 병합의 존재 이유는, 나중에 다른 사람이 인터페이스를 문제 없이 확장할 수 있도록 하기 위함입니다.
```typescript
interface Merge {
    one: string;
}

interface Merge {
    two: number;
}

const example: Merge = {
    one: '1',
    two: 2
};
```

### 이렇게, 다른 사람이 수정해도 되는 객체의 타입을 인터페이스로 선언해 두면 다른 사람은 언제든지 같은 이름의 인터페이스를 만들어 타입을 수정할 수 있습니다.

### 주의할 점은, 인터페이스 간에 속성이 겹치는데 타입이 다를 경우에는 에러가 발생한다는 것입니다. 속성이 같다면 타입도 같아야 합니다.

---

### 선언 병합을 하면서 큰 단점은, 라이브러리를 설치해 사용하는 경우가 있을 때 그러한 라이브러리의 인터페이스와 우연의 일치로 내 인터페이스 이름과 겹칠 수 있는데, 그 경우 인터페이스가 병합되어 원치 않은 결과를 낳게 됩니다.

### 이러한 문제점을 대비해 네임스페이스(namespace)라는 것이 존재합니다.
```typescript
namespace Example {
    export interface Inner {
        test: string;
    }
    export type test2 = number;
}

const ex1: Example.Inner = {
    test: 'hello'
};

const ex2: Example.test2 = 123;
```

### 네임스페이스 안의 요소는 export 키워드를 추가해줘야 접근할 수 있습니다.

### 네임스페이스 내부에 실제 값이 있다면, 자바스크립트 값으로 사용할 수 있는데, 이는 올바른 사용법이 아닙니다.

### 하지만, 예상했다 싶이 네임스페이스도 병합되는 특성이 있으므로, 다른 사람이 같은 네임스페이스를 만든다면 합쳐지는 문제가 발생할 수 있습니다. 이를 방지하기 위해 모듈 파일이 존재합니다. 모듈 파일은 5단원에서 알아봅니다.

---

### 객체의 속성에 붙을 수 있는 특징을 알아봅시다.

### 객체의 속성에도 옵셔널(?)이나 readonly 수식어가 가능합니다. 여러 수식어를 한 프로퍼티에 동시에 붙일 수도 있습니다.
```typescript
interface Example {
    hello: string;
    world?: number;
    readonly wow: boolean;
    readonly multiple?: symbol;
}

const example: Example = {
    hello: 'hi',
    wow: false
};
example.no; // Property 'no' does not exist on type 'Example'.
example.wow = true; // Cannot assign to 'wow' because it is a read-only prop.
```

---

### 인터페이스 속성에는 없는 속성을 구현하면서 만들 때, 오류가 발생하는 경우가 있고, 아닌 경우가 있습니다. 이는 객체 리터럴을 넣느냐 변수를 넣느냐에서 차이가 발생합니다.

### 객체 리터럴을 대입하면, 잉여 속성 검사가 실행되는데, 이는 타입 선언에서 선언하지 않은 속성을 사용할 때 에러를 표시하는 것을 의미합니다. 변수는 또 다른데, 나중에 다루겠습니다.

---

### 객체에서도, 전개 문법과 나머지 속성을 사용할 수 있습니다.
```typescript
const { prop: { nested, ...rest } } = { prop: { nested: 'hi', a: 1, b: true } };

const spread = { a: 'hi', b: 123 };
const obj = {...spread};

// and

const { prop: { nested } }: { prop: { nested: string } } = {
    prop: { nested: 'hi' }
};
console.log(nested);
```

### and 아래에 쓴 코드는, 타입을 왜 구조 분해 할당에 적지 않고 다시 적었냐면, 이는 타입임을 명시하는 코드는 타입 란에만 허용하기 때문입니다.

---

### 특정 속성의 타입을 복사 붙여넣기하여 다른 타입 변수에서 확인하고자 한다면 이렇게 해야 합니다. 이렇게 객체 속성의 타입에 접근하는 방식을 '인덱스 접근 타입(Indexed Access Type)'이라고 부릅니다.

```typescript
type Animal = {
    name: string;
};

type N1 = Animal['name'];
```

### 위 코드에서 보시는 바와 같이, 인덱스 접근자는 사용할 수 있으나 마침표 연산자는 사용이 불가능합니다.

---

### 속성의 키와 값이 궁금할 떄는 이렇게 하면 됩니다.
```typescript
const obj = {
    hello: 'world',
    name: 'zero',
    age: 28
};

type Keys = keyof typeof obj; 
type Values = typeof obj[Keys];
```

- obj는 값이라서 타입 자리에 바로 쓸 수 없기에, typeof 연산자를 붙여 타입으로 만들었습니다.

---

### 인덱스 접근 타입을 활용해서, 특ㅈ어 키들의 값 타입만 추릴 수 있습니다.
```typescript
const obj = {
    hello: 'world',
    name: 'zero',
    age: 28
};
type Values = typeof obj['hello' | 'name']; // type Values = string
```

---

### 인덱스 시그니처와는 달리 일부 속성에만 타입을 부여하는 방법도 존재합니다. 이를 '매핑된 객체 타입(Mapped Object Type)' 이라고 합니다. 기존의 다른 타입으로부터 새로운 객체 속성을 만들어내는 타입을 의미합니다. 

### 매핑된 객체 타입은, 인터페이스에서는 쓰지 못하고, 타입 별칭에서만 사용할 수 있습니다.

```typescript
type HelloAndHi = {
    [key in 'hello' | 'hi']: string; 
};
/* 
type HelloAndHi = {
    hello: string;
    hi: string;
}
*/
```

### in 연산자를 사용해서 인덱스 시그니처가 표현하지 못하는 타입을 표현하는데, 오른쪽에는 유니언 타입이 와야 합니다. 

### 유니언 타입에 속한 타입이 하나씩 순서대로 평가되어 객체의 속성이 됩니다.

### 매핑된 객체 타입은, 더 복잡한 상황에 주로 사용합니다.

```typescript
interface Original {
    name: string;
    age: number;
    married: boolean;
};

type Copy = {
    [key in keyof Original]: Original[key];
}
```

### 다른 타입으로부터 값을 가져오면서, 수식어를 붙이거나 제거할 수도 있습니다.

```typescript
interface Original {
    name: string;
    age: number;
    married: boolean;
};

type Copy = {
    readonly [key in keyof Original]? : Original[key];
}

type CopyD = {
    -readonly [key in keyof Original]-? : Original[key];
}

type CopyN = {
    [key in keyof Original as Capitalize<key>]: Original[key];
}
```

### 수식어 앞에 dash(-)를 붙여, 수식어를 제거합니다.

### 위 코드의 CopyN과 같이, 가져오는 과정에서, 타입의 이름도 바꿀 수 있습니다.

---

### 유니언(|)을 합집합으로, 인터섹션(&)을 교집합으로 생각하면 좋습니다.

### 그런데, string & number에서 교집합에 속하는 값이 없습니다. 이를 공집합이라고 부릅니다. 타입스크립트에서는 never가 공집합이라는 역할을 맡습니다.
```typescript
type nev = string & number; // type nev = never;
```

- 전체 집합: unknown (가장 넓은 타입)
- 공집합: never (가장 좁은 타입)
- 합집합: | (유니언 연산자)
- 교집합: & (인터섹션 연산자)

---

### 타입스크립트에서는, 좁은 타입을 넓은 타입에 대입할 수 있습니다. 반대는 불가능합니다.

---

### any 타입은 집합 관계를 무시하므로, & | 연산을 any와 함께 사용하지 않는 것이 좋습니다. 일관성이 없어진다는 이유 때문입니다.

### 예시들
```typescript
type A = string | boolean;
type B = boolean | number;
type C = A & B; // type C = boolean

type D = {} & (string | null); // type D = string

type E = string & boolean; // type E = never;

type F = unknown | {}; // type F = unknown
type G = never & {}; // type G = never
```
- 타입 C는 A와 B의 교집합입니다.
- 타입 D는 {}과 string | null의 교집합입니다. {}에는 string만 포함되기에 D는 string이 됩니다.
- 타입 E처럼 서로 간에 겹치는 것이 없다면 never가 됩니다.
- 타입 F와 G처럼 unknown과의 | 연산은 무조건 unknown, never과의 & 연산은 무조건 never입니다.

### 여기서 중요한 점은, null과 undefined를 제외한 원시 자료형과, 비어 있지 않은 객체를 교집합 연산할 때는 never가 되지 않습니다.

---

### JS에서 객체 간에 상속을 하여 중복을 제거하듯이, 타입스크립트에서도 객체 타입 간에 상속하는 방법이 있습니다.
```typescript
interface Animal {
    name: string;
}

interface Dog extends Animal {
    bark(): void;
}

interface Cat extends Animal {
    meow(): void;
}

// 타입에서 상속 받는 방법
type Animal = {
    name: string;
};

type Dog = Animal & {
    bark(): void;
};

type Cat = Animal & {
    meow(): void;
}

type Name = Cat['name'];
```

### 타입 별칭이 인터페이스를 상속할 수도 있고, 인터페이스가 타입 별칭을 상속할 수도 있습니다.

### 다중 상속도 가능합니다.

```typescript
interface DojaCat extends Dog, Cat {};
```

### 부모 속성의 타입을 변경할 수도 있긴 한데, 완전히 다른 타입으로의 변경은 불가능합니다.

---

### 변수를 대입할 떄는, 객체 리터럴의 잉여 속성 검사와는 다르게 대입할 수 있는지 여부를 따져봐야 합니다.

### 좁은 타입은 넓은 타입에 대입할 수 있지만, 반대는 불가능하다는 개념은 객체에도 똑같이 적용됩니다. 어떤 객체가 더 넓은가만 판단할 수 있으면 됩니다.

```typescript
interface A {
    name: string;
};

interface B {
    name: string;
    age: number;
}
```

### B 타입에는 name, age 속성이 꼭 있어야 하지만 A를 넣으면 age가 공백이 생겨버리기에 B에 A를 넣을 수 없습니다.

### 인터페이스에 적힌 속성이 더 많을 수록, 더 구체적이고 좁은 타입인 것입니다.

### readonly가 배열에 붙으면, 일반 배열보다 더 넓은 타입이 됩니다. 즉 튜플도 일반 배열보다 넓어질 수 있습니다.

### 이렇게, 넓은 타입 좁은 타입만 구분할 수 있다면, 누가 누구에게 대입할 수 있는지 없는지를 쉽게 판단할 수 있습니다.

---

### 타입스크립트에서는, 모든 속성이 동일하면 객체 타입의 이름이 다르더라도 동일한 타입으로 취급합니다.

```typescript
interface Money {
    amount: number;
    unit: string;
}

interface Liter {
    amount: number;
    unit: string;
}

const liter: Liter = { amount: 1, unit: 'liter' };
const circle: Money = liter;
```

### 이렇게, 객체를 어떻게 만들었든 간에 구조가 같으면 같은 객체로 인식하는 것을 구조적 타이핑(structural typing)이라고 부릅니다.

---

### 그럼 서로 대입하지 못하게, 구조적 타이핑이 맞지 않도록 하려면 서로를 구분하기 위한 속성을 추가해야 합니다. 다른 속성과 겹치지 않는 이름으로 대표적으로 __type이 있습니다. 이를 브랜드 속성이라고 부르고, 브랜드 속성을 사용하는 것을 '브랜딩'이라고 부릅니다.
```typescript
interface Money {
    __type: 'money';
    amount: number;
    unit: string;
};

interface Liter {
    __type: 'liter';
    amount: number;
    unit: string;
}
```

---

```typescript
interface Zero {
    type: 'human';
    race: 'yellow';
    name: 'zero';
    age: 28;
}

interface Nero {
    type: 'human';
    race: 'yellow';
    name: 'nero';
    age: 32;
}
```

### 위 코드에서, 공통적인 속성과 값은 type, race입니다. 다른 것은 name과 age이며, 둘은 구조적으로 같습니다. 이 때, 그저 공통적인 인터페이스를 만들어 이렇게 하면 됩니다.

```typescript
interface Person<N, A> {
    type: 'human';
    race: 'yellow';
    name: N;
    age: A;
};

interface Zero extends Person<'zero', 28> {}
interface Nero extends Person<'nero', 32> {}
```

### 제네릭 표기는 꺽쇠(<>)로 하며, 인터페이스 이름 바로 뒤에 위치합니다. 이 안에 타입 매개변수(Type Parameter)를 넣으면 됩니다.

### 선언한 제네릭을 사용할 때는, Person<'zero', 28>과 같이 매개변수에 대응하는 실제 타입 인수(Type Argument)를 넣으면 됩니다.

### Array도 제네릭 타입이기에 <> 부분이 있었던 것입니다.

### 제네릭이 없었다면? : 쓸 때마다 이렇게 만들어주어야 합니다.

```typescript
interface StringArray { . . . }
interface BooleanArray {.  .. } 
```

### 인터페이스 뿐만 아니라, 클래스와 타입 별칭, 함수도 제네릭을 가질 수 있습니다.

### 함수에서는 함수 표현식이나 선언문이냐에 따라 제네릭 표기 위치가 다릅니다.
```typescript
const 함수이름 = <GN, NG>(name: GN, age: NG) => . . .;

function 함수이름<GN, NG>(name: GN, age: NG) {. . .}
```

### 제네릭에 기본값도 넣을 수 있습니다.

### 상수 타입 매개변수도 사용 가능합니다.

```typescript
function values<const T>(initial: T[]) {
    return {
        hasValue(value: T) { return initial.includes(value) }
    };
}

const savedValues = values(["a", "b", "c"]);
savedValues.hasValue("x"); // x 할당 불가능 에러
```

---

### 타입 매개변수에는, 제약을 사용할 수 있습니다.
```typescript
interface Example<A extends number, B extends A> {
    a: A,
    b: B
}

type Usecase1 = Example<string, boolean>; // 오류
type Usecase2 = Example<1, boolean>;
type Usecase3 = Example<number>;
```

### 너무 강박적으로 제네릭을 사용할 필요는 없습니다. 그저 필요할 때만 사용하면 됩니다.

---

### 컨디셔널 타입이란, 조건에 따라 다른 타입이 됨을 뜻합니다.

### 여기서도 extends가 사용되는데, 여기의 extends는 삼항연산자와 같이 사용됩니다.

### 특정 타입 extends 다른 타입 ? 참일 때 타입 : 거짓일 때 타입

```typescript
type A1 = string;
type B1 = A1 extends string ? number : boolean;
```

---

### 매핑된 객체 타입에서, 키가 never이면 해당 속성은 제거됩니다. 따라서 다음과 같이 사용할 수 있습니다.

```typescript
type OmitByType<O, T> = {
    [K in keyof O as O[K] extends T ? never : K]: O[K];
};

type Result = OmitByType<
{
    name: string;
    age: number;
    married: boolean;
    rich: boolean;
}, boolean
>;
```

### 또는 인덱스 접근 타입으로, 컨디셔널 타입을 표현할 수도 있습니다.
```typescript
type A1 = string;
type B1 = A1 extends string ? number : boolean;
type B2 = {
    't': number;
    'f': boolean;
}[A1 extends string ? 't' : 'f'];
```

