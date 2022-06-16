---
layout: post
title: 9강 InterLocked

categories: [GameServer]
tags: [GameServer]
render_with_liquid: false
---

# 9강 InterLocked

**부제: 경합 조건 (Race condition)**

공유 변수 접근의 문제점

```csharp
class Program{
	static int num = 0;
	static object obj = new object();

	static void Tread_1(){
		for(int i=0; i< 10000; i++){            
            num++;
		}
	}

    static void Tread_2(){

        for (int i=0; i<10000; i++){
            num--;
        }
    }
}

//...
//스레드들 실행시키는 구문

```

- 10000번씩 1씩 더하고 1씩 빼는 코드
- 결과로 0이 나온다 (나와야 한다) ⇒ 하지만 이상한 값이 나오네?
- int number에 volatile 키워드를 붙여도 똑같이 이상한 값이 나옴

# 경합 조건

**Race condition**

스토리와 비교

1. 손님 1명과 직원 3명
2. 손님이 콜라 하나를 주문시킴
3. 직원 3명이 하나씩 콜라를 가져다줌
4. 손님은 별안간 콜라가 3개를 받게됨
5. **== 경합 조건 발생한 것 !**

### ++ 연산자의 실행 원리

```csharp
num++;
```

위 코드의 어셈블리 코드에서의 실행 원리는 다음과 같다

```csharp
int tmp = num;
tmp += 1;
num = tmp;
```

- - - 도 위에서 `tmp -= 1;` 일뿐 마찬가지이다.

스레드에서 이 코드가 실행되는 도중 경합이 일어나게 되어서 이상한 값이 나온 것 같다.

## Interlocked 를 이용한 해결법

```csharp
class Program{
	static int num = 0;
	static object obj = new object();

	static void Tread_1(){
		for(int i=0; i< 10000; i++){

            Interlocked.Increment(ref num);

		}
	}

    static void Tread_2(){

        for (int i=0; i<10000; i++){
            Interlocked.Decrement(ref num);
        }
    }
}

// ...
//스레드들 실행시키는 구문
```

- ++은 `Interlocked.Incremnt(ref num)`
- - - 은 `Interlocked.Decrement(ref num)`
- 스레드 1과 2의 (1) ++과 (2) - - 작업이 원자적으로 일어나게 된다
- 원자적 := 순차적으로 (1) 작업 뒤에 (2) 작업이 일어나게 된다 (순서가 보장됨)

### Interlocked.increment (ref num) 에서 ref의 사용

실제 num ‘값’이 아니라 num의 주소를 가져와서 레퍼런스를 사용함

## Interlock.Decrement () 의 단점

대부분 정수만 사용가능

사용할 수 있는 것이 한정되어 있음
