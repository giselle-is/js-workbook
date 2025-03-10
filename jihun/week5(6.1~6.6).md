## 6.1 재귀와 스택

함수에 대해 좀 더 깊이 알아보도록 하자. 함수 심화학습, 첫 번째 주제는 *재귀(recursion)* 이다.

재귀는 큰 목표 작업 하나를 동일하면서 간단한 작업 여러 개로 나눌 수 있을 때 유용한 프로그래밍 패턴이며, 목표 작업을  간단한 동작 하나와 목표 작업을 변형한 작업으로 단순화시킬 수 있을 때도 재귀를 사용할 수 있다. 곧 살펴보겠지만, 특정  자료구조를 다뤄야 할 때도 재귀가 사용된다.

문제 해결을 하다 보면 함수에서 다른 함수를 호출해야 할 때가 있다. 이때 함수가 *자기 자신*을 호출할 수도 있는데, 이를 *재귀* 라고 부르기 때문에 꼭 기억하자.



#### 두 가지 사고방식

간단한 예시를 시작으로 재귀에 대해 알아보자. 

`x`를 `n` 제곱해 주는 함수 `pow(x, n)`를 만들어보면, `pow(x, n)`는 `x`를 `n`번 곱해주기 때문에 아래 결과를 만족해야 한다.

```javascript

pow(2, 2) = 4
pow(2, 3) = 8
pow(2, 4) = 16
```

구현하는 방법은 두 가지가 있다.

1. 반복적인 사고를 통한 방법: `for` 루프

```javascript
function pow(x, n) {
  let result = 1;

  // 반복문을 돌면서 x를 n번 곱함
  for (let i = 0; i < n; i++) {
    result *= x;
  }

  return result;
}

alert( pow(2, 3) ); // 8
```



2. 재귀적인 사고를 통한 방법: 작업을 단순화하고 자기 자신을 호출함

```javascript
function pow(x, n) {
  if (n == 1) {
    return x;
  } else {
    return x * pow(x, n - 1);
  }
}

alert( pow(2, 3) ); // 8
```



재귀를 이용한 예시가 반복문을 사용한 예시와 어떤 부분에서 근본적인 차이가 있는지 유심히 살펴보자.

`pow (x, n)`을 호출하면 아래와 같이 두 갈래로 나뉘어 코드가 실행된다.

```javascript

              if n==1  = x
             /
pow(x, n) =
             \
              else     = x * pow(x, n - 1)
```

1. `n == 1`일 때: 모든 절차가 간단해진다. 명확한 결괏값을 즉시 도출하므로 이를 *재귀의 베이스(base)* 라고 한다. `pow(x, 1)`는 `x` 이다.
2. `n == 1`이 아닐 때: `pow(x, n)`은 `x * pow(x, n - 1)`으로 표현할 수 있다. 수학식으론 `xn = x * xn-1`로 표현할 수 있다. 이를 *재귀 단계(recursive step)* 라고 부른다. 여기선 목표 작업 `pow(x, n)`을 간단한 동작(`x`를 곱하기)과 목표 작업을 변형한 작업(`pow(x, n - 1)`)으로 분할하였다. 재귀 단계는 `n`이 `1`이 될 때까지 계속 이어진다.

즉, `pow`는 `n == 1`이 될 때까지 *재귀적으로 자신을 호출*한다.

![Screen Shot 2021-05-08 at 11 39 09 AM](https://user-images.githubusercontent.com/79819941/117524042-b21d1c80-aff6-11eb-993b-4f42b322bd32.png) 

`pow (2, 4)`를 계산하려면 아래와 같은 재귀 단계가 차례대로 이어진다.

1. `pow(2, 4) = 2 * pow(2, 3)`
2. `pow(2, 3) = 2 * pow(2, 2)`
3. `pow(2, 2) = 2 * pow(2, 1)`
4. `pow(2, 1) = 2`

이렇게 재귀를 이용하면 함수 호출의 결과가 명확해질 때까지 함수 호출을 더 간단한 함수 호출로 계속 줄일 수 있다.

> 재귀를 사용한 코드는 짧다.
>
> 재귀를 사용한 코드는 반복적 사고에 근거하여 작성한 코드보다 대개 짧다.
>
> `if` 대신 조건부 연산자 `?`를 사용하면 `pow (x, n)`를 더 간결하고 읽기 쉽게 만들 수도 있다.
>
> ​                      
>
> ```javascript
> function pow(x, n) {
>   return (n == 1) ? x : (x * pow(x, n - 1));
> }
> ```



가장 처음 하는 호출을 포함한 중첩 호출의 최대 개수는 *재귀 깊이(recursion depth)* 라고 한다. `pow(x, n)`의 재귀 깊이는 `n`이다.

**자바스크립트 엔진은 최대 재귀 깊이를 제한한다. 만개 정도까진 확실히 허용하고, 엔진에 따라 이보다 더 많은 깊이를  허용하는 경우도 있다. 하지만 대다수의 엔진이 십만까지는 다루지 못한다. 이런 제한을 완화하려고 엔진 내부에서 자동으로  'tail calls optimization’라는 최적화를 수행하긴 하지만, 모든 곳에 적용되는 것은 아니고 간단한 경우에만  적용된다.**

재귀 깊이 제한 때문에 재귀를 실제 적용하는데 제약이 있긴 하지만, 재귀는 여전히 광범위하게 사용되고 있다. 재귀를 사용하면, 간결하고 유지보수가 쉬운 코드를 만들 수 있기 때문이다.





#### 실행 컨텍스트와 스택

실제 재귀 호출이 어떻게 동작하는지 알아보자. 이를 위해서 함수의 내부 동작에 대해 살펴보도록 한다.

실행 중인 함수의 실행 절차에 대한 정보는 해당 함수의 *실행 컨텍스트(execution context)* 에 저장된다.

[실행 컨텍스트](https://tc39.github.io/ecma262/#sec-execution-contexts)는 함수 실행에 대한 세부 정보를 담고 있는 내부 데이터 구조이다. 제어 흐름의 현재 위치, 변수의 현재 값, `this`의 값(여기선 다루지 않음) 등 상세 내부 정보가 실행 컨텍스트에 저장된다.

함수 호출 일 회당 정확히 하나의 실행 컨텍스트가 생성된다.

함수 내부에 중첩 호출이 있을 때는 아래와 같은 절차가 수행된다.

- 현재 함수의 실행이 일시 중지된다.
- 중지된 함수와 연관된 실행 컨텍스트는 *실행 컨텍스트 스택(execution context stack)* 이라는 특별한 자료 구조에 저장된다.
- 중첩 호출이 실행된다.
- 중첩 호출 실행이 끝난 이후 실행 컨텍스트 스택에서 일시 중단한 함수의 실행 컨텍스트를 꺼내오고, 중단한 함수의 실행을 다시 이어간다.

이제 `pow (2, 3)`가 호출되면 실행 컨텍스트에서 무슨 일이 일어나는지 살펴보자.



#### pow(2, 3)

`pow (2, 3)`를 호출하는 순간, 실행 컨텍스트엔 변수 `x = 2, n = 3`이 저장되고, 실행 흐름은 함수의 첫 번째 줄에 위치한다.

이를 도식화하면 다음과 같다.

- ​    Context: { x: 2, n: 3, 첫 번째 줄 }    pow(2, 3)  

위 그림은 함수 실행이 시작되는 순간을 나타낸 것이다. 지금 상태론 조건 `n == 1`을 만족하지 못하므로 실행 흐름은 `if`의 두 번째 분기로 넘어간다.

​                      

​                      

```javascript

function pow(x, n) {
  if (n == 1) {
    return x;
  } else {
    return x * pow(x, n - 1);
  }
}

alert( pow(2, 3) );
```

변수는 동일하지만, 실행 흐름의 위치가 변경되면서 실행 컨텍스트도 다음과 같이 변경된다.

- ​    Context: { x: 2, n: 3, 다섯 번째 줄 }    pow(2, 3)  

`x * pow (x, n - 1)`을 계산하려면 새로운 인수가 들어가는 `pow`의 서브 호출(subcall), `pow (2, 2)`을 만들어야 한다.



#### pow(2, 2)

중첩 호출을 하기 위해, 자바스크립트는 *실행 컨텍스트 스택* 에 현재 실행 컨텍스트를 저장한다.

지금 보고 있는 예시에선 실행 컨텍스트 스택에 동일한 함수 `pow`를 호출하였는데, 이는 중요치 않다. 모든 함수에 대해 아래 프로세스가 똑같이 적용된다.

1. 스택 최상단에 현재 컨텍스트가 '기록’된다.
2. 서브 호출을 위한 새로운 컨텍스트가 만들어진다.
3. 서브 호출이 완료되면. 기존 컨텍스트를 스택에서 꺼내(pop) 실행을 이어나간다.

다음은 서브 호출 `pow (2, 2)`이 시작될 때의 실행 컨텍스트 스택이다.

- ​    Context: { x: 2, n: 2, 첫 번째 줄 }    pow(2, 2)  
- ​    Context: { x: 2, n: 3, 다섯 번째 줄 }    pow(2, 3)  

굵은 테두리로 표시한 새 실행 컨텍스트는 상단에, 기존 컨텍스트는 하단에 있다.

이전 컨텍스트에 변수 정보, 코드가 일시 중단된 줄에 대한 정보가 저장되어있기 때문에 서브 호출이 끝났을 때 이전 컨텍스트가 문제없이 다시 시작된다.



예시엔 한 줄에 서브 호출 하나만 있기 때문에, 그림에서 '줄’이라는 단어를 사용했다. 하지만 한 줄에는 `pow(…) + pow(…) + somethingElse(…)` 같이 복수의 서브 호출이 있을 수 있다.

따라서 좀 더 정확히는 실행이 '서브 호출 바로 직후’에 시작된다고 이야기 할 수 있다.



#### pow(2, 1)

동일한 과정이 다시 반복된다. 다섯 번째 줄에서 인수 `x = 2`, `n = 1`과 함께 새로운 서브 호출이 만들어진다.

새로운 실행 컨텍스트가 만들어지고, 이전 실행 컨텍스트는 스택 최상단에 올라간다(push).

- ​    Context: { x: 2, n: 1, 첫 번째 줄 }    pow(2, 1)  
- ​    Context: { x: 2, n: 2, 다섯 번째 줄 }    pow(2, 2)  
- ​    Context: { x: 2, n: 3, 다섯 번째 줄 }    pow(2, 3)  

기존 컨텍스트 두 개가 밑에, `pow (2, 1)`에 상응하는 컨텍스트가 맨 위에 있는 것을 확인할 수 있다.



#### 실행 종료

`pow (2, 1)`가 실행될 땐 상황이 달라진다. 이전과는 달리 조건 `n == 1`을 만족시키므로 `if`문의 첫 번째 분기가 실행된다.

```javascript

function pow(x, n) {
  if (n == 1) {
    return x;
  } else {
    return x * pow(x, n - 1);
  }
}
```

이젠 호출해야 할 중첩 호출이 없습니다. 따라서 함수는 종료되고 `2`가 반환된다.

함수가 종료되었기 때문에 이에 상응하는 실행 컨텍스트는 쓸모가 없어졌다. 따라서 해당 실행 컨텍스트는 메모리에서 삭제된다. 스택 맨 위엔 이전의 실행 컨텍스가 위치하게 된다.

- ​    Context: { x: 2, n: 2, 다섯 번째 줄 }    pow(2, 2)  
- ​    Context: { x: 2, n: 3, 다섯 번째 줄 }    pow(2, 3)  

`pow (2, 2)`의 실행이 다시 시작된다. 서브 호출 `pow (2, 1)`의 결과를 알고 있으므로, 쉽게 `x * pow (x, n - 1)`를 계산해 `4`를 반환한다.

그리고 다시 이전 컨텍스트가 스택 최상단에 위치하게 된다.

- ​    Context: { x: 2, n: 3, 다섯 번째 줄 }    pow(2, 3)  

마지막 실행 컨텍스트까지 처리되면 `pow (2, 3) = 8`이라는 결과가 도출된다.

지금 보신 예시의 재귀 깊이는 **3** 이다.

도식을 통해 살펴보았듯이, 재귀 깊이는 스택에 들어가는 실행 컨텍스트 수의 최댓값과 같다.

실행 컨텍스트는 메모리를 차지하므로 재귀를 사용할 땐 메모리 요구사항에 유의해야 한다. `n`을 늘리면 `n`이 줄어들 때마다 만들어지는 `n`개의 실행 컨텍스트가 저장될 메모리 공간이 필요하기 때문이다.

한편, 반복문 기반 알고리즘을 사용하면 메모리가 절약된다.

```javascript

function pow(x, n) {
  let result = 1;

  for (let i = 0; i < n; i++) {
    result *= x;
  }

  return result;
}
```

반복을 사용해 만든 함수 `pow`는 컨텍스트를 하나만 사용한다. 이 컨텍스트에서 `i`와 `result`가 변경된다. 실행 컨텍스트가 하나이기 때문에 `n`에 의존적이지 않고, 필요한 메모리가 적다. 사용 메모리 공간도 고정된다.

**재귀를 이용해 작성한 코드는 반복문을 사용한 코드로 다시 작성할 수 있다. 반복문을 사용하면 대개 함수 호출의 비용(메모리 사용)이 절약된다.**

하지만 코드를 다시 작성해도 큰 개선이 없는 경우가 있다. 조건에 따라 함수가 다른 재귀 서브 호출을 하고 그 결과를  합칠 때가 그렇다. 분기문이 복잡하게 얽혀있을 때도 메모리가 크게 절약되지 않는다. 이런 경우엔 최적화기 필요하지 않을 수  있고 최적화에 드는 노력이 무용지물일 수 있다.

재귀를 사용하면 코드가 짧아지고 코드 이해도가 높아지며 유지보수에도 이점이 있다. 모든 곳에서 메모리 최적화를 신경 써서 코드를 작성해야 하는 것은 아니다. 우리가 필요한 것은 좋은 코드이다. 이런 이유 때문에 재귀를 사용한다.



#### 재귀적 순회

재귀는 재귀적 순회(recursive traversal)를 구현할 때 사용하면 좋다.

한 회사가 있다고 가정해 보자. 임직원을 아래와 같이 객체로 표현해 보았다.

```javascript

let company = {
  sales: [{
    name: 'John',
    salary: 1000
  }, {
    name: 'Alice',
    salary: 1600
  }],

  development: {
    sites: [{
      name: 'Peter',
      salary: 2000
    }, {
      name: 'Alex',
      salary: 1800
    }],

    internals: [{
      name: 'Jack',
      salary: 1300
    }]
  }
};
```

회사엔 부서가 있다.

- 부서에는 여러 명의 직원이 있는데, 이를 배열로 표현할 수 있다. `sales` 부서의 John과 Alice라는 2명의 직원을 배열 요소로 표현해 보았다.

- 부서는 하위 부서를 가질 수 있다. `development` 부서는 `sites`와 `internals`라는 두 개의 하위 부서를 갖는다. 각 하위부서에도 직원이 있다.

- 하위 부서가 커지면 더 작은 단위의 하위 부서(또는 팀)로 쪼개질 가능성도 있다.

  `sites` 부서는 미래에 `siteA`와 `siteB`로 나뉠 수 있다. 이렇게 나눠진 부서가 미래에 더 세분화될 수도 있다. 미래에 벌어질 일까진 나타내지 않았지만, 이러한 가능성도 있다는 걸 염두에 두어야 한다.

자, 이제 모든 임직원의 급여를 더한 값을 구해야 한다고 해보자. 어떻게 할 수 있을까?

구조가 단순하지 않기 때문에 반복문을 사용해선 구하기 쉽지 않아 보인다. 가장 먼저 떠오르는 생각은 `company`를 대상으로 동작하는 `for` 반복문을 만들고 한 단계 아래의 부서에 중첩 반복문를 돌리는 걸거다. 그런데 이렇게 하면 `sites` 같은 두 단계 아래의 부서에 속한 임직원의 급여를 뽑아낼 때 또 다른 중첩 반복문이 필요하다. 세 단계 아래의 부서가 미래에  만들어진다고 가정하면 또 다른 중첩 반복문이 필요하겠다. 얼마만큼의 깊이까지 중첩 반복문을 만들 수 있을까? 객체를 순회하는  중첩 반복문의 깊이가 3~4개가 되는 순간 코드는 정말 지저분해질 것이다.

재귀를 이용한 풀이법을 시도해 보자.

앞서 본 바와 같이 임직원 급여 합계를 구할 때는 두 가지 경우로 나누어 생각할 수 있다.

1. 임직원 *배열* 을 가진 ‘단순한’ 부서 – 간단한 반복문으로 급여 합계를 구할 수 있다.
2. `N`개의 하위 부서가 있는 *객체* – 각 하위 부서에 속한 임직원의 급여 합계를 얻기 위해 `N`번의 재귀 호출을 하고, 최종적으로 모든 하위부서 임직원의 급여를 더한다.

배열을 사용하는 첫 번째 경우는 간단한 경우로, 재귀의 베이스가 된다.

객체를 사용하는 두 번째 경우는 재귀 단계가 된다. 복잡한 작업은 작은 작업(하위 부서에 대한 반복문)으로 쪼갤 수 있다. 부서의 깊이에 따라 더 작은 작업으로 쪼갤 수 있는데, 결국 마지막엔 첫 번째 경우가 된다.

코드를 직접 읽어보면서 재귀 알고리즘을 이해해보자.

​                      

​                      

```javascript

let company = { // 동일한 객체(간결성을 위해 약간 압축함)
  sales: [{name: 'John', salary: 1000}, {name: 'Alice', salary: 1600 }],
  development: {
    sites: [{name: 'Peter', salary: 2000}, {name: 'Alex', salary: 1800 }],
    internals: [{name: 'Jack', salary: 1300}]
  }
};

// 급여 합계를 구해주는 함수
function sumSalaries(department) {
  if (Array.isArray(department)) { // 첫 번째 경우
    return department.reduce((prev, current) => prev + current.salary, 0); // 배열의 요소를 합함
  } else { // 두 번째 경우
    let sum = 0;
    for (let subdep of Object.values(department)) {
      sum += sumSalaries(subdep); // 재귀 호출로 각 하위 부서 임직원의 급여 총합을 구함
    }
    return sum;
  }
}

alert(sumSalaries(company)); // 7700
```

짧고 이해하기 쉬운 코드로 원하는 기능을 구현하였다. 재귀의 강력함은 여기에 있다. 하위 부서의 깊이와 상관없이 원하는 값을 구할 수 있게 되었다.

아래는 호출이 어떻게 일어나는지를 나타낸 그림이다.

그림을 보면 규칙을 쉽게 확인할 수 있다. 객체 `{...}`를 만나면 서브 호출이 만들어지는 반면, 배열 `[...]`을 만나면 더 이상의 서브 호출이 만들어지지 않고 결과가 바로 계산된다.

함수 내부에선 앞서 학습한 두 문법을 사용하고 있는 것도 눈여겨보자.

- [배열과 메서드](https://ko.javascript.info/array-methods) 챕터에서 학습한 메서드 `arr.reduce`는 배열의 합을 계산해 준다.
- `for(val of Object.values (obj))`에서 쓰인 `Object.values`는 프로퍼티의 값이 담긴 배열을 반환한다.





#### 재귀적 구조

재귀적으로 정의된 자료구조인 재귀적 자료 구조는 자기 자신의 일부를 복제하는 형태의 자료 구조이다.

위에서 살펴본 회사 구조 역시 재귀적 자료 구조 형태이다.

회사의 *부서* 객체는 두 가지 종류로 나뉜다.

- 사람들로 구성된 배열
- 하위 부서로 이루어진 객체

웹 개발자에게 익숙한 HTML과 XML도 재귀적 자료 구조 형태를 띤다.

HTML 문서에서 *HTML 태그*는 아래와 같은 항목으로 구성되기 때문이다.

- 일반 텍스트
- HTML-주석
- 이 외의 *HTML 태그* (이 아래에 일반 텍스트, HTML-주석, 다른 HTML 태그가 올 수 있다.)

이렇게 다양한 곳에서 재귀적으로 정의된 자료구조가 쓰인다.

다음은 '연결 리스트’라는 재귀적 자료 구조를 살펴보면서 재귀적 구조에 대해 더 알아보도록 하자. 몇몇 상황에서 배열 대신 연결 리스트를 사용하면 더 좋은 경우가 있다.



#### 연결 리스트

객체를 정렬하여 어딘가에 저장하고 싶다고 가정해 보자.

가장 먼저 떠오르는 자료 구조는 아마 배열일 것이다.

```javascript

let arr = [obj1, obj2, obj3];
```

하지만 배열은 요소 '삭제’와 '삽입’에 들어가는 비용이 많이 든다는 문제가 있다. `arr.unshift(obj)` 연산을 수행하려면 새로운 `obj`를 위한 공간을 만들기 위해 모든 요소의 번호를 다시 매겨야 하는데 배열이 커지면 연산 수행 시간이 더 걸리게 된다. `arr.shift()`를 사용할 때도 마찬가지이다.

요소 전체의 번호를 다시 매기지 않아도 되는 조작은 배열 끝에 하는 연산인 `arr.push/pop` 뿐이다. 앞쪽 요소에 무언가를 할 때 배열은 이처럼 꽤 느리다.

빠르게 삽입 혹은 삭제를 해야 할 때는 배열 대신 [연결 리스트(linked list)](https://en.wikipedia.org/wiki/Linked_list)라 불리는 자료 구조를 사용할 수 있다.

*연결 리스트의 요소* 는 객체와 아래 프로퍼티들을 조합해 정의할 수 있다.

- `value`
- `next`: 다음 *연결 리스트 요소*를 참조하는 프로퍼티. 다음 요소가 없을 땐 `null`이 된다.



```javascript
let list = {
  value: 1,
  next: {
    value: 2,
    next: {
      value: 3,
      next: {
        value: 4,
        next: null
      }
    }
  }
};
```

![Screen Shot 2021-05-08 at 11 39 33 AM](https://user-images.githubusercontent.com/79819941/117527688-d4b83100-b008-11eb-912f-cd67bb88ca8b.png) 

위 연결 리스트를 그림으로 표현하면 다음과 같다.

아래처럼 코드를 작성해도 동일한 연결 리스트가 된다.

```javascript
let list = { value: 1 };
list.next = { value: 2 };
list.next.next = { value: 3 };
list.next.next.next = { value: 4 };
list.next.next.next.next = null;
```

이렇게 연결 리스트를 만드니 객체가 여러개 있고, 각 객체엔 `value`와 이웃 객체를 가리키는 프로퍼티인 `next`가 있는 게 명확히 보인다. 체인의 시작 객체는 변수 `list`에 저장되어 있다. 우리는 `list`의 `next` 프로퍼티를 이용해 이어지는 객체 어디든 도달할 수 있다.

연결 리스트를 사용하면 전체 리스트를 여러 부분으로 쉽게 나눌 수 있고, 다시 합치는 것도 가능하다.

```javascript
let secondList = list.next.next;
list.next.next = null;
```

![Screen Shot 2021-05-08 at 11 39 38 AM](https://user-images.githubusercontent.com/79819941/117527673-beaa7080-b008-11eb-89df-8120c1a87187.png) 



합치기:

```javascript

list.next.next = secondList;
```

그리고 쉽게 요소를 추가하거나 삭제할 수 있다,

리스트의 처음 객체를 바꾸면 리스트 맨 앞에 새로운 값을 추가할 수 있다.

```javascript

let list = { value: 1 };
list.next = { value: 2 };
list.next.next = { value: 3 };
list.next.next.next = { value: 4 };

// list에 새로운 value를 추가한다.
list = { value: "new item", next: list };
```

![Screen Shot 2021-05-08 at 11 39 44 AM](https://user-images.githubusercontent.com/79819941/117527651-97ec3a00-b008-11eb-9c9d-2718a72c598c.png) 

중간 요소를 제거하려면 이전 요소의 `next`를 변경해주면 된다.

```javascript
list.next = list.next.next;
```

![Screen Shot 2021-05-08 at 11 39 51 AM](https://user-images.githubusercontent.com/79819941/117527663-aa667380-b008-11eb-822f-0bd37938def5.png) 

`list.next`가 `1`이 아닌 `2`를 `value`로 갖는 객체를 가리키게 만들어보았다. 이제 `value` `1`은 체인에서 제외된다. 이 객체는 다른 곳에 따로 저장하지 않으면 자동으로 메모리에서 제거된다.

지금까지 살펴본 바와 같이 연결 리스트는 배열과는 달리 대량으로 요소 번호를 재할당하지 않으므로 요소를 쉽게 재배열할 수 있다는 장점이 있다.

물론 연결 리스트가 항상 배열보다 우월하지는 않다. 그렇지 않았다면 모든 사람들이 연결 리스트만 사용했을 것이다.

연결 리스트의 가장 큰 단점은 번호(인덱스)만 사용해 요소에 쉽게 접근할 수 없다는 점인데, 배열을 사용하면 `arr[n]`처럼 번호 `n`만으로도 원하는 요소에 바로 접근할 수 있다. 그러나 연결 리스트에선 `N`번째 값을 얻기 위해 첫 번째 항목부터 시작해 `N`번 `next`로 이동해야 한다.

그런데 중간에 요소를 삽입하거나 삭제하는 연산이 항상 필요한 것은 아니다. 이럴 땐 순서가 있는 자료형 중에 큐(queue)나 [데크(deque)](https://en.wikipedia.org/wiki/Double-ended_queue)를 사용할 수 있다. 데크를 사용하면 양 끝에서 삽입과 삭제를 빠르게 수행할 수 있다.

위에서 구현한 연결 리스트는 아래와 같은 기능을 더해 개선할 수 있다.

- 이전 요소를 참조하는 프로퍼티 `prev`를 추가해 이전 요소로 쉽게 이동하게 할 수 있다.
- 리스트의 마지막 요소를 참조하는 변수 `tail`를 추가할 수 있다. 다만 이때 주의할 점은 리스트 마지막에 요소를 추가하거나 삭제할 때 `tail`도 갱신해 줘야 한다.
- 이 외에도 요구사항에 따라 구조를 변경할 수 있다.



> #### 요약
>
> - *재귀(recursion)* – 함수 내부에서 자기 자신을 호출하는 것을 나타내는 프로그래밍 용어이다. 재귀 함수는 우아하게 원하는 문제를 해결할 때 자주 쓰이곤 한다.
>
>   함수가 자신을 호출하는 단계를 *재귀 단계(recursion step)* 라고 부른다. basis라고도 불리는 재귀의 *베이스(base)* 는 작업을 아주 간단하게 만들어서 함수가 더 이상은 서브 호출을 만들지 않게 해주는 인수이다.
>
> - [재귀적으로 정의된](https://en.wikipedia.org/wiki/Recursive_data_type) 자료 구조는 자기 자신을 이용해 자료 구조를 정의한다.
>
>   재귀적으로 정의된 자료구조에 속하는 연결 리스트는 리스트 혹은 null을 참조하는 객체로 이루어진 데이터 구조를 사용해 정의된다.
>
>   ```javascript
>   list = {value, next -> list}
>   ```
>
> HTML 문서의 HTML 요소 트리나 위에서 다룬 부서를 나타내는 트리 역시 재귀적인 자료 구조로  만들었다. 이렇게 재귀적인 자료 구조를 사용하면 가지가 여러 개인데 각 가지가 여러 가지로 뻗쳐 나가는 형태로 자료 구조를  만들 수 있다.
>
> 예시에서 구현한 `sumSalary`같은 재귀 함수를 사용하면 각 분기(가지)를 순회할 수 있다.
>
> 모든 재귀 함수는 반복문을 사용한 함수로 다시 작성할 수 있다. 최적화를 위해 반복문으로 다시 작성해야 할 수도 있죠.  그러나 상당수 작업은 재귀를 사용해도 만족할 만큼 빠르게 동작한다. 재귀를 사용하면 구현과 유지보수가 쉽다는 장점도 있다.







## 6.2 나머지 매개변수와 전개 문법

상당수의 자바스크립트 내장 함수는 인수의 개수에 제약을 두지 않는다.

- `Math.max(arg1, arg2, ..., argN)` – 인수 중 가장 큰 수를 반환한다.
- `Object.assign(dest, src1, ..., srcN)` – `src1..N`의 프로퍼티를 `dest`로 복사한다.
- 기타 등등

이번 챕터에서는 이렇게 임의의 수의 인수를 받는 방법에 대해 알아보고 함수의 매개변수에 배열을 전달하는 방법에 대해서도 알아보자.



#### 나머지 매개변수 `...`

함수 정의 방법과 상관없이 함수에 넘겨주는 인수의 개수엔 제약이 없다.

```javascript
function sum(a, b) {
  return a + b;
}

alert( sum(1, 2, 3, 4, 5) );
```

함수를 정의할 땐 인수를 두 개만 받도록 하고, 실제 함수를 호출할 땐 이보다 더 많은 ‘여분의’ 인수를 전달했지만, 에러가 발생하지 않았다. 다만 반환 값은 처음 두 개의 인수만을 사용해 계산된다.

이렇게 여분의 매개변수는 그 값들을 담을 배열 이름을 마침표 세 개 `...`뒤에 붙여주면 함수 선언부에 포함시킬 수 있다. 이때 마침표 세 개 `...`는 "나머지 매개변수들을 한데 모아 배열에 집어넣어라."는 것을 의미한다.

아래 예시에선 모든 인수가 배열 `args`에 모인다.                 

```javascript

function sumAll(...args) { // args는 배열의 이름이다.
  let sum = 0;

  for (let arg of args) sum += arg;

  return sum;
}

alert( sumAll(1) ); // 1
alert( sumAll(1, 2) ); // 3
alert( sumAll(1, 2, 3) ); // 6
```



앞부분의 매개변수는 변수로, 그 이외의 매개변수들은 배열로 모을 수도 있다.

아래 예시에선 처음 두 인수는 변수에, 나머지 인수들은 `titles`이라는 배열에 할당된다. 

```javascript

function showName(firstName, lastName, ...titles) {
  alert( firstName + ' ' + lastName ); // Julius Caesar

  // 나머지 인수들은 배열 titles의 요소가 됩니다.
  // titles = ["Consul", "Imperator"]
  alert( titles[0] ); // Consul
  alert( titles[1] ); // Imperator
  alert( titles.length ); // 2
}

showName("Julius", "Caesar", "Consul", "Imperator");
```



나머지 매개변수는 항상 마지막에 있어야 한다.

나머지 매개변수는 남아있는 인수를 모으는 역할을 하므로 아래 예시에선 에러가 발생한다.

```javascript

function f(arg1, ...rest, arg2) { // ...rest 후에 arg2가 있으면 안 된다.
  // 에러
}
```

`...rest`는 항상 마지막에 있어야 한다.



#### ‘arguments’ 변수

`arguemnts`라는 특별한 유사 배열 객체(array-like object)를 이용하면 인덱스를 사용해 모든 인수에 접근할 수 있다.



```javascript

function showName() {
  alert( arguments.length );
  alert( arguments[0] );
  alert( arguments[1] );

  // arguments는 이터러블 객체이기 때문에
  // for(let arg of arguments) alert(arg); 를 사용해 인수를 나열할 수 있다.
}

// 2, Julius, Caesar가 출력됨
showName("Julius", "Caesar");

// 1, Bora, undefined가 출력됨(두 번째 인수는 없음)
showName("Bora");
```

나머지 매개변수는 비교적 최신의 문법이다. 과거엔 함수의 인수 전체를 얻어내는 방법이 `arguments`를 사용하는 것밖에 없었다. 물론 지금도 `arguments`를 사용할 수 있다. 오래된 코드를 보다 보면 `arguments`를 만나게 된다.

`arguments`는 유사 배열 객체이면서 이터러블(반복 가능한) 객체이다. 어쨌든 배열은 아니다. 따라서 **배열 메서드를 사용할 수 없다**는 단점이 있다. `arguments.map (...)`을 호출할 수 없다.

여기에 더하여 `arguments`는 인수 전체를 담기 때문에 나머지 매개변수처럼 인수의 일부만 사용할 수 없다는 단점도 있다.

따라서 배열 메서드를 사용하고 싶거나 인수 일부만 사용하고자 할 때는 나머지 매개변수를 사용하는 것이 좋다.



화살표 함수에는 `\'arguments\'`가 없다.

화살표 함수에서 `arguments` 객체에 접근하면, 외부에 있는 ‘일반’ 함수의 arguments 객체를 가져온다.

```javascript

function f() {
  let showArg = () => alert(arguments[0]);
  showArg();
}

f(1); // 1
```

앞서 배운 바와 같이 화살표 함수는 자체 `this`를 가지지 않습니다. 여기에 더하여 `arguments` 객체도 없다는 것을 위 예시를 통해 확인해 보았습니다.





#### spread 문법

지금까지 매개변수 목록을 배열로 가져오는 방법에 대해 살펴보았다.

그런데 개발을 하다 보면 반대되는 기능이 필요할 때가 생긴다. 배열을 통째로 매개변수에 넘겨주는 것 같이 말이다.

예시를 통해 이런 경우를 살펴보자. 내장 함수 [Math.max](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math/max)는 인수로 받은 숫자 중 가장 큰 숫자를 반환한다.



```javascript
alert( Math.max(3, 5, 1) ); // 5
```

배열 `[3, 5, 1]`가 있고, 이 배열을 대상으로 `Math.max`를 호출하고 싶다고 가정해보자.

아무런 조작 없이 배열을 ‘있는 그대로’ `Math.max`에 넘기면 원하는 대로 동작하지 않는다. `Math.max`는 배열이 아닌 숫자 목록을 인수로 받기 때문이다.         

```javascript

let arr = [3, 5, 1];

alert( Math.max(arr) ); // NaN
```

`Math.max (arr[0], arr[1], arr[2])` 처럼 배열 요소를  수동으로 나열하는 방법도 있긴 한데 배열 길이를 알 수 없을 때는 이마저도 불가능하다. 스크립트가 돌아갈 때 실제 넘어오는  배열의 길이는 아주 길 수도 있고, 아예 빈 배열일 수도 있기 때문이다. 수동으로 이걸 다 처리하다 보면 코드가 지저분해진다.

*전개 문법(spread syntax)* 은 이럴 때 사용하기 위해 만들어졌다. `...`를 사용하기 때문에 나머지 매개변수와 비슷해 보이지만, 전개 문법은 나머지 매개변수와 반대의 역할을 한다.

함수를 호출할 때 `... arr`를 사용하면, 이터러블 객체 `arr`이 인수 목록으로 '확장’된다.

`Math.max`를 사용한 예시로 다시 돌아가 보자.

```javascript

let arr = [3, 5, 1];

alert( Math.max(...arr) ); // 5 (전개 문법이 배열을 인수 목록으로 바꿔주었다.)
```



아래와 같이 이터러블 객체 여러 개를 전달하는 것도 가능하다.

```javascript

let arr1 = [1, -2, 3, 4];
let arr2 = [8, 3, -8, 1];

alert( Math.max(...arr1, ...arr2) ); // 8
```

전개 문법을 평범한 값과 혼합해 사용하는 것도 가능하죠.

​                      

​                      

```javascript

let arr1 = [1, -2, 3, 4];
let arr2 = [8, 3, -8, 1];

alert( Math.max(1, ...arr1, 2, ...arr2, 25) ); // 25
```



배열을 합칠 때 전개 문법을 활용할 수도 있다.                   

```javascript

let arr = [3, 5, 1];
let arr2 = [8, 9, 15];

let merged = [0, ...arr, 2, ...arr2];

alert(merged); // 0,3,5,1,2,8,9,15 (0, arr, 2, arr2 순서로 합쳐진다.)
```

앞선 예시들에선 배열을 대상으로 전개 문법이 어떻게 동작하는지 살펴보았다. 그런데 배열이 아니더라도 이터러블 객체이면 전개 문법을 사용할 수 있다.



전개 문법을 사용해 문자열을 문자 배열로 변환 시켜 보자.

```javascript

let str = "Hello";

alert( [...str] ); // H,e,l,l,o
```

전개 문법은 `for..of`와 같은 방식으로 내부에서 iterator(반복자)를 사용해 요소를 수집한다.

문자열에 `for..of`를 사용하면 문자열을 구성하는 문자가 반환된다. `...str`도 `"H","e","l","l","o"`가 되는데, 이 문자 목록은 배열 초기자(array initializer) `[...str]`로 전달된다.

메서드 `Array.from`은 문자열 같은 이터러블 객체를 배열로 바꿔주기 때문에 `Array.from`을 사용해도 동일한 작업을 할 수 있다.                      

```javascript

let str = "Hello";

// Array.from은 이터러블을 배열로 바꿔준다.
alert( Array.from(str) ); // H,e,l,l,o
```

`[...str]`와 동일한 결과가 출력되는 것을 확인할 수 있다.

그런데 `Array.from(obj)`와 `[...obj]`에는 다음과 같은 미묘한 차이가 있다.

- `Array.from`은 유사 배열 객체와 이터러블 객체 둘 다에 사용할 수 있다.
- 전개 문법은 이터러블 객체에만 사용할 수 있다.

이런 이유때문에 무언가를 배열로 바꿀 때는 전개 문법보다 `Array.from`이 보편적으로 사용된다.





array/object의 새 복사본 가져오기

예전에  `Object.assign()`에 대해서 말한 적이 있는데, 이를 spread 문법으로 동일한 작업을 수행할 수 있다.

```javascript

let arr = [1, 2, 3];
let arrCopy = [...arr]; // array를 매개 변수 목록으로 spread 문법 사용
                        // 그리고 해당 결과를 새로운 array에 복사

// array의 내용이 동일할까?
alert(JSON.stringify(arr) === JSON.stringify(arrCopy)); // true

// 같은 array 일까?
alert(arr === arrCopy); // false (동일한 참조가 아니기 때문)

// 초기 배열은 수정을 해도 사본은 수정되지 않는다.
arr.push(4);
alert(arr); // 1, 2, 3, 4
alert(arrCopy); // 1, 2, 3
```



Object의 복사본을 만들기 위해 동일한 작업을 수행할 수 있다.                                            

```javascript

let obj = { a: 1, b: 2, c: 3 };
let objCopy = { ...obj }; // 객체를 매개 변수 목록으로 spread 문법 사용.
                          // 그리고 해당 결과를 새 object로 반환.

// object에 내용이 동일할까?
alert(JSON.stringify(obj) === JSON.stringify(objCopy)); // true

// 같은 Object 일까?
alert(obj === objCopy); // false (동일한 참조가 아님)

// 초기 객체를 수정해도 사본은 수정되지 않는다.
obj.d = 4;
alert(JSON.stringify(obj)); // {"a":1,"b":2,"c":3,"d":4}
alert(JSON.stringify(objCopy)); // {"a":1,"b":2,"c":3}
```

Object를 복사하는 Spread 문법은 'let objCopy = Object.assign ({, obj);' 또는 'let arcopy = Object.assign ([, arr;)' 방식보다 훨씬 구문이 짧기 때문에 해당 방법으로 사용하는 것을 권장한다.



> #### 요약
>
> `"..."`은 나머지 매개변수나 전개 문법으로 사용된다.
>
> 나머지 매개변수와 전개 문법은 아래의 방법으로 구분할 수 있다.
>
> - `...`이 함수 매개변수의 끝에 있으면 인수 목록의 나머지를 배열로 모아주는 '나머지 매개변수’이다.
> - `...`이 함수 호출 시 사용되면 배열을 목록으로 확장해주는 '전개 문법’이다.
>
> 사용 패턴:
>
> - 인수 개수에 제한이 없는 함수를 만들 때 나머지 매개변수를 사용한다.
> - 다수의 인수를 받는 함수에 배열을 전달할 때 전개 문법을 사용한다.
>
> 둘을 함께 사용하면 매개변수 목록과 배열 간 전환을 쉽게 할 수 있다.
>
> 조금 오래된 방법이긴 하지만 `arguments`라는 반복 가능한 유사 배열 객체를 사용해도 인수 모두를 사용할 수 있다.

