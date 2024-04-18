# B-Tree 인덱스 (2부)  
1. B-Tree 데이터 삭제  
  1-1. 삭제 예시  
  1-2. 삭제 시 재조정 예시
2. B-Tree Internal 노드 - 데이터 삭제   
   2-1. 삭제 예시  
   2-2. 5자 B-Tree 삭제 예시       
 

---


1. 데이터 삭제

- 특징
  - 삭제도 항상 leaf 노드에서 발생
  - 삭제 후 최소 key 수보다 적어졌다면 재조정함
      1. key 수가 여유있는 형제의 지원을 받음
      2. 1번이 불가능하면 부모의 지원을 받고 형제와 합침
      3. 2번 후 부모에 문제가 있다면 거기서 다시 재조정
  - B-Tree에서 각 노드의 최소 key 수 : **(M/2 (반올림)) -1**
  - ex. 3차 B-tree에서 각 노드의 최소 key 수: **1** (root node 제외)
 


1-1. 삭제 예시  


STEP 1. 6 삭제  

![image](https://github.com/mithzinf/DB-Study/assets/124668883/3b554229-8c7d-49be-abe6-7a528a87347c)  

STEP 2. 5 삭제  

![image](https://github.com/mithzinf/DB-Study/assets/124668883/2a99c4b6-6daf-4145-8547-2a51d84654a4)  
👉삭제 후, 노드의 각 key 수가 0이기 때문에, 저 빈 자리는 재조정이 필요함  
      1. key 수가 여유있는 형제의 지원을 받음
      2. 1번이 불가능하면 부모의 지원을 받고 형제와 합침
      3. 2번 후 부모에 문제가 있다면 거기서 다시 재조정




---



1-2. 3차 B-Tree 삭제 시 재조정 예시  

##### 1. key 수가 여유있는 형제의 지원을 받는다(3 삭제)    

![image](https://github.com/mithzinf/DB-Study/assets/124668883/f3e15fdd-831f-4105-8efd-c4680adb0fc0)  

```
1.1 동생 (왼쪽 형제)이 여유가 있으면  
=> 동생의 가장 큰 key를 부모 노드의 나와 동생 사이에 둔다.  
=> 원래 그 자리에 있던 key는 나의 가장 왼쪽에 둔다.  

1.2 동생 (왼쪽 형제)이 여유가 없고, 형 (오른쪽 형제)이 여유가 있다면,  
=> 형의 가장 작은 key를 부모 노드의 나와 형 사이에 둔다.  
=> 원래 그 자리에 있던 key는 나의 가장 오른쪽에 둔다.  
```

**최종**   
![image](https://github.com/mithzinf/DB-Study/assets/124668883/12e62a7e-67a9-4e56-9326-7bf5b7c1c040)





##### 2. 1번이 불가능하면 부모의 지원을 받고 형제와 합친다(=왼쪽으로 옮긴다) (7 삭제)   
![image](https://github.com/mithzinf/DB-Study/assets/124668883/d240674e-8ab4-4b11-b35e-f5422a734797)    
🤯문제점 : 왼쪽, 오른쪽 형제 노드를 보니 다 여유가 없는 상황 -> 1번 방법 불가능한 상황 -> 2번 방법으로 넘어가야 함    

```
2.1 동생이 있으면 동생과 나 사이의 key를 부모로부터 받음  
=> 그 key와 나의 key를 차례대로 동생에게 합침  
=> 나의 노드를 삭제  

1.2 동생이 없으면, 형과 나 사이의 key를 부모로부터 받음  
=> 그 key와 형의 key를 차례대로 나에게 합침  
=> 형의 노드를 삭제  
```


**최종 : 부모 노드 한 쪽이 사라지고, 그 값이 동생 노드의 값으로 들어감**   
![image](https://github.com/mithzinf/DB-Study/assets/124668883/c06cdf18-3c3b-4310-823b-466da878166b)   


*예제 이어서  :2 삭제*  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/35e292c0-87f9-47ac-8158-5e0a9e3e4fc2)  

**2 삭제 최종**  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/db1f7a29-8d4d-4a76-9ef0-d809ccea0d75)  

*예제 이어서 : 1 삭제*  

![image](https://github.com/mithzinf/DB-Study/assets/124668883/ff6fec5b-4183-4b5a-b12f-c3b4833b332f)  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/926c242d-2a12-4852-88df-80d0cea62d8b)  


**1 삭제 최종**   

![image](https://github.com/mithzinf/DB-Study/assets/124668883/565970fa-bf27-4fdd-8584-5e5c2ac4d05e)   
🤯문제점 : 부모의 지원을 받고, 형제와 합치고 나니, 부모 노드의 key 수가 0임    




##### 3. 부모가 지원한 후 부모에 문제가 있다면 상황에 맞게 대응한다  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/e73a51dd-c086-4fd4-ad83-9b39469a12be)  
현재 상황 : 부모 노드의 key 수가 0인 문제가 발생


```
부모가 지원한 후 부모의 문제가 있다면 상황에 맞게 대응하기

3.1 부모가 root 노드가 아니라면  
=> 해당 위치에서부터 다시 1번부터 재조정을 시작  

3.2 부모가 root 노드인데다 비어있다면  
=> 부모 노드를 삭제!  
=> 직전의 합쳐진 node가 root 노드가 됨  
```


1. 부모(초록색 노드)의 지원을 받기로 하고, 형제 노드와 합치기로 함  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/2fda9907-e07c-41e9-be63-b90ac42019ab)  
2. 합친 후  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/4888f393-61e3-4b4d-b5e4-6ee48441e65c)
![image](https://github.com/mithzinf/DB-Study/assets/124668883/59ec021d-2bd0-40d0-904d-947f667b7124)  
3. 합친 후, 부모에게 문제가 생겼음(부모 노드 key 다 털림) > 부모 노드 삭제 처리 해줌  
4. 최종   
![image](https://github.com/mithzinf/DB-Study/assets/124668883/1f1f26fb-0f50-401a-91ad-234e98b453cf)



**정리 : B-Tree 데이터 삭제 -> leaf 노드에서 삭제하고 필요하면 재조정을 거친다**  


😯 그렇다면? Internal 노드 데이터 삭제할 땐 어떻게 해야하지?
   

---

2. B-Tree Internal 노드 - 데이터 삭제    
- 정의 : leaf 노드에 있는 데이터와 위치를 바꾼 후 삭제  

- 방법
 - internal 노드에 있는 데이터를 삭제하려면 leaf 노드에 있는 데이터와 위치를 바꾼 후 삭제
   - leaf 노드에 있는 데이터 중에 어떤 데이터와 위치를 바꿔줄 것인지가 이슈가 됨  
   - 선임자나 후임자는 항상 leaf 노드에 있으므로, 삭제할 데이터의 선임자나 후임자와 위치를 교체
   - 선임자(predecessor) : 나보다 작은 데이터들 중에서 가장 큰 데이터
   - 후임자(successor) : 나보다 큰 데이터들 중에서 가장 작은 데이터  



2-1. B-Tree Internal 노드 삭제 예시  


1. 예시 (선임자 pick)  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/e1d6a962-b4ad-4ba9-b678-dd1ba2056c4e)  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/ece2152d-121f-4f28-864b-c3f492b65ba4)
  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/fefe654e-aa2e-43ba-92f0-a983e9eb45be)  




2-2. 5자 B-Tree 삭제 예시


![image](https://github.com/mithzinf/DB-Study/assets/124668883/a791dd75-9344-420e-816d-f65baeb41bdf)


![image](https://github.com/mithzinf/DB-Study/assets/124668883/baf5244f-efd8-4206-99bc-a06cc9c3f5f1)


![image](https://github.com/mithzinf/DB-Study/assets/124668883/5dea5ac1-18c9-44c6-b49e-c7a899e12dd5)


![image](https://github.com/mithzinf/DB-Study/assets/124668883/e4113ec2-c51c-45ad-8c44-623730adc423)


![image](https://github.com/mithzinf/DB-Study/assets/124668883/e9a3e476-147e-42e5-9f8d-9892a17f15b8)


![image](https://github.com/mithzinf/DB-Study/assets/124668883/4ce553a3-4012-4eed-b4e9-c1e936275c7f)

![image](https://github.com/mithzinf/DB-Study/assets/124668883/559989b7-98ac-4626-b7a0-cfe58e2e7f43)

![image](https://github.com/mithzinf/DB-Study/assets/124668883/2c31b773-d803-44da-90c6-e47f7401c9a5)


![image](https://github.com/mithzinf/DB-Study/assets/124668883/332154da-2084-4e9c-94ff-d04b53be0118)


![image](https://github.com/mithzinf/DB-Study/assets/124668883/0ac1d90d-b718-44b6-87f2-a27a3b30fea7)


