# Blocking/Non-blocking & Synchronous/Asynchronous

### - 동기/비동기(Synchronous/Asynchronous)

​	프로세서의 수행 순서 보장에 대한 메커니즘

### - 블로킹/논블로킹(Blocking/Non-blocking)

​	프로세스의 유휴 상태에 대한 개념

> #### 참고
>
> **제어권** : 자신(함수)의 코드를 실행할 권리 같은 것. 제어권을 가진 함수는 자신의 코드를 끝까지 실행한 후, 자신을 호출한 함수에게 돌려준다.
>
> **결과값을 기다린다는 것** : A 함수에서 B 함수를 호출했을 때, A 함수가 B 함수의 결과값을 기다리느냐의 여부를 의미



## Synchronous/Asynchronous

> 처리해야 할 작업들을 어떠한 '흐름'으로 처리할 것인가에 대한 관점

=> 즉, 호출되는 함수의 작업 완료 여부를 신경쓰냐에 따라, 함수 실행/리턴 순차적인 흐름을 따르는가 여부

**Synchronous(동기)**

호출하는 함수 A가 호출되는 함수 B의 작업 완료 후 리턴을 기다리거나, 바로 리턴 받더라도 미완료 상태이라면 작업 완료 여부를 스스로 계속 확인하며 신경

**Asynchronous(비동기)**

함수 A가 함수 B를 호출할 때 콜백 함수를 함께 전달해서, 함수 B의 작업이 완료되면 함께 보낸 콜백 함수를 실행한다. 함수 A는 함수 B를 호출한 후로 함수 B의 작업 완료 여부에는 신경쓰지 않는다.



## Blocking/Non-blocking

> 처리 되어야하는 (하나의) 작업이, 전체적인 작업 '흐름'을 막느냐 안막느냐에 대한 관점

=>  즉, 제어권이 누구에게 있느냐가 관심사

**Blocking(블로킹)**

A함수가 B함수르 호출하면, 제어권을 A가 호출한 B 함수에 넘겨준다.

![image-20230206231021674](C:\Users\dongi\AppData\Roaming\Typora\typora-user-images\image-20230206231021674.png)

**Non-Blocking(논블로킹)**

A함수가 B함수를 호출해도 제어권은 그대로 자신이 가지고 있는다.

![image-20230206231126959](C:\Users\dongi\AppData\Roaming\Typora\typora-user-images\image-20230206231126959.png)

## 동기/비동기 + 블로킹/논블로킹

![image-20230206234526418](C:\Users\dongi\AppData\Roaming\Typora\typora-user-images\image-20230206234526418.png)

### 1. Sync-Blocking

![image-20230206234844970](C:\Users\dongi\AppData\Roaming\Typora\typora-user-images\image-20230206234844970.png)

함수 A는 함수 B의 리턴값을 필요로 한다(**동기**)

그래서 제어권을 함수 B에게 넘겨주고, 함수 B가 실행을 완료하여 리턴값과 제어권을 돌려줄때까지 기다린다(**블로킹**)

### 2. Sync-Nonblocking

![image-20230206235719971](C:\Users\dongi\AppData\Roaming\Typora\typora-user-images\image-20230206235719971.png)

A함수는 B 함수를 호출한다.

이 때 A 함수는 B 함수에게 제어권을 주지 않고, 자신의 코드를 계속 실행한다(**논블로킹**)

그런데 A 함수는 B 함수의 리턴값이 필요하기 때문에, 중간중간 B 함수에게 함수 실행을 완료했는지 물어본다(**동기**)

### 3. Async-Nonblocking

![image-20230207000038254](C:\Users\dongi\AppData\Roaming\Typora\typora-user-images\image-20230207000038254.png)

A 함수는 B 함수를 호출한다.

이 때 제어권을 B 함수에 주지 않고, 자신이 계속 가지고 있는다. 따라서 B 함수를 호출한 이후에도 멈추지 않고 자신의 코드를 계속 실행한다.(**논블로킹**)

그리고 B 함수를 호출할 때 콜백 함수를 함께 준다. 자신의 작업이 끝나면 A 함수가 준 콜백함수를 실행한다.(**비동기**)

### 4. Async-blocking

![image-20230207000250822](C:\Users\dongi\AppData\Roaming\Typora\typora-user-images\image-20230207000250822.png)

A 함수는 B 함수의 리턴값에 신경쓰지 않고, 콜백함수로 보낸다(**비동기**)

그런데, B 함수의 작업에 관심없음에도 불구하고, A 함수는 B 함수에게 제어권을 넘긴다(**블로킹**)

따라서, A 함수는 자신과 관련없는 B 함수의 작업이 끝날때 까지 기다려야 한다.