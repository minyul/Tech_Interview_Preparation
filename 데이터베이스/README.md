# 🐼 데이터베이스




---


### 데이터베이스 Lock의 종류 및 각 Lock에 대한 설명을 해보시오.    
</br>
Lock의 종류에는 Shared Lock, Exclusive Lock이 있다.    
    
1) Shared Lock    
Shared Lock은 Read Lock이라고도 한다. 즉, 데이터를 '읽을' 때 사용하는 Lock이다. Shared Lock은 같은 Shared Lock끼리는 동시에 접근이 가능하다.
이는 데이터 일관성과 무결성을 해치지 않기 때문이다. 즉, 어떤 자원에 Shared Lock이 동시에 여러개가 적용될 수 있다.    
하지만 Exclusive Lock의 접근은 막는다. 즉, 어떤 자원에 Shared Lock이 하나라도 걸려있으면 Exclusive Lock을 걸 수 없다.    
     
2) Exclusive Lock    
Exclusive Lock은 Write Lock이라고도 한다. 즉, 데이터를 '변경'할 때 사용하는 Lock이다. Exclusive Lock은 트랜잭션이 끝날 때 까지 유지된다.
Exclusive Lock이 끝날 때 까지 '어떠한' 접근도 허용되지 않는다. 즉, 다른 트랜잭션이 읽거나 수정할 수 없다.    

---


### 데이터베이스 Blocking 이 무엇이며 어떠한 경우에 발생하는 것이며 해결하기 위한 방법을 설명하시오.    
</br>          
Blocking은 Lock들의 경합(Race Condition)이 발생하여 특정 세션이 작업을 진행하지 못하고 멈춰선 상태를 의미한다.    
Shared Lock과 Exclusive Lock 혹은 Exclusive Lock과 Exclusive Lock끼리 Blocking이 발생할 수 있다. 이를 해결하는 방안은 Transaction commit 혹은 rollback뿐이다.    
    
Blocking을 줄이기 위한 방법    
1. SQL 문장을 리팩토링하여 빠르게 실행되도록 한다.    
2. 트랜잭션 처리시간이 짧으면 경합을 줄일 수 있다. 따라서 작업 단위가 긴 트랜잭션을 여러개의 작업 단위로 쪼갠다.   
3. 동일한 데이터를 변경하는 작업은 하지 않는다. 또한, 트랜잭션이 활발한 시간에는 대용량 갱신 작업을 수행하지 않는다.    
4. 대용량 갱신 작업을 해야할 경우 2번과 같이 작업 단위를 쪼개거나 lock_timeout을 설정하여 Lock의 최대 시간을 정해준다.    

---



### 데이터베이스 Dead Lock 이 무엇이며 해결하기 위한 방법을 설명하시오.    
</br>    
Dead Lock(교착상태)는 두 개 이상의 트랜잭션이 각각 Lock을 설정하고 상대의 Lock을 획득하려고 할 때, 서로의 Lock이 해제되는 것을 무한정 기다리는 상태를 말한다.   
    
교착상태를 해결하기 위한 방법은 크게 3가지가 있다.    
1) 타임아웃 방식    
일정시간 이상 트랜잭션이 Lock을 기다리지 않게 하는 방식이다. 트랜잭션이 영구히 Lock을 기다리지 않으므로 Dead Lock이 발생하지 않는다.    
    
2) 교착상태 예방(방지) 방식   
트랜잭션이 접근해야 하는 모든 데이터에 대한 Lock을 트랜잭션이 수행하기 전에 사전 선언하는 방식이다. 하지만 실제적으로는 트랜잭션이 접근해야 하는 데이터를 사전에 알 수가 없기 떄문에 실효성이 없는 방식이다.
    
3) 교착상태 감지 및 해결 방식    
교착상태 감지를 위하여 데이터베이스 시스템은 내부적으로 자원 할당 그래프를 변형한 대기 그래프(wait-for graph)를 유지해야 한다. 이 대기 그래프는 자원 할당 그래프에서 자원을 제거한 후에 간선들을 결합하여 얻을 수 있다. 대기 그래프는 방향성을 가지는 그래프이기 때문에 사이클이 있다면 교착상태를 의미한다.    
교착상태 해결 방식으로는 current blocker를 철회하는 방식이 널리 쓰인다. current blocker란 대기 그래프에서 사이클을 만드는 Lock을 제공하는 트랜잭션을 의미한다.


---

