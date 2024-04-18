# B-Tree 인덱스 (2부)  
1. 데이터 삭제  
  1-1. 삭제 예시  
  1-2. 삭제 시 재조정 예시  
3. Non-Clustered Index  
 

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


1.1 동생 (왼쪽 형제)이 여유가 있으면  
=> 동생의 가장 큰 key를 부모 노드의 나와 동생 사이에 둔다.  
=> 원래 그 자리에 있던 key는 나의 가장 왼쪽에 둔다.  

1.2 동생 (왼쪽 형제)이 여유가 없고, 형 (오른쪽 형제)이 여유가 있다면,  
=> 형의 가장 작은 key를 부모 노드의 나와 형 사이에 둔다.  
=> 원래 그 자리에 있던 key는 나의 가장 오른쪽에 둔다.  


**최종**   
![image](https://github.com/mithzinf/DB-Study/assets/124668883/12e62a7e-67a9-4e56-9326-7bf5b7c1c040)





##### 2. 1번이 불가능하면 부모의 지원을 받고 형제와 합친다(=왼쪽으로 옮긴다) (7 삭제)   
![image](https://github.com/mithzinf/DB-Study/assets/124668883/d240674e-8ab4-4b11-b35e-f5422a734797)    
🤯문제점 : 왼쪽, 오른쪽 형제 노드를 보니 다 여유가 없는 상황 -> 1번 방법 불가능한 상황 -> 2번 방법으로 넘어가야 함    


**최종 : 부모 노드 한 쪽이 사라지고, 그 값이 동생 노드의 값으로 들어감**   
![image](https://github.com/mithzinf/DB-Study/assets/124668883/c06cdf18-3c3b-4310-823b-466da878166b)   


*이어서  :2 삭제*  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/35e292c0-87f9-47ac-8158-5e0a9e3e4fc2)  

**2 삭제 최종**  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/db1f7a29-8d4d-4a76-9ef0-d809ccea0d75)  

*이어서 : 1 삭제*  

![image](https://github.com/mithzinf/DB-Study/assets/124668883/ff6fec5b-4183-4b5a-b12f-c3b4833b332f)  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/926c242d-2a12-4852-88df-80d0cea62d8b)  


**1 삭제 최종**   

![image](https://github.com/mithzinf/DB-Study/assets/124668883/565970fa-bf27-4fdd-8584-5e5c2ac4d05e)   
🤯문제점 : 부모의 지원을 받고, 형제와 합치고 나니, 부모 노드의 key 수가 0임    


















