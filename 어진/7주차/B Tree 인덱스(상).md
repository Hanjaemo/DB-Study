# B-Tree 인덱스  

0. 이진 탐색 트리   
1. B-Tree 인덱스란?   
   1-1. B-Tree 인덱스 특징  
   1-2. B-Tree의 주요 네 가지 파라미터  
2. B-Tree 데이터 삽입  
   2-1. 3차 B-Tree 데이터 삽입 예시  

---



0. 이진 탐색 트리(BST)
   - 모든 노드의 왼쪽 서브 트리는 해당 노드의 값보다 작은 값들만 가지고 있고,
   - 모든 노드의 오른쪽 서브 트리는 해당 노드의 값보다 큰 값들만 가진다
   - 각 노드는 최대 2개의 자식 노드를 가질 수 있고
   - 부모 노드의 값에 따라서, 최대 2개의 자식 노드의 값의 범위가 결정된다 (부모 노드의 값이 곧 기준이 됨)


   ![image](https://github.com/mithzinf/DB-Study/assets/124668883/dd07c013-8679-48ea-a14d-c910147b23b0)

  ![image](https://github.com/mithzinf/DB-Study/assets/124668883/9dc20b79-0283-488e-b0f0-941234bdb71a)

   
👉이진 탐색 트리에서 부모 노드의 값에 따라, 자식 노드의 값의 범위가 결정되는 것처럼  
👉위의 아이디어를 B-tree에서도 응용하게 되면 어떨까?  


##### 그러면 B-tree에서는 어떻게 적용하면 될까?    
![image](https://github.com/mithzinf/DB-Study/assets/124668883/b50d5c3b-56d8-44c9-baa9-9f32da66b5fe)  
3개의 자식 노드를 다음과 같은 기준으로 값의 범위를 나눌려면 k1, k2 두개의 값이 필요하게 됨   
So, **부모 노드에는** 1개의 값만 저장하면 되는 것이 아니라 **k1값과 k2값을 같이 저장**할 수 있도록 해야함  

![image](https://github.com/mithzinf/DB-Study/assets/124668883/00a896ff-633f-4822-9a9f-53ba53c47409)  



> ex. 부모 노드에 50. 80 값이 들어가 있는 경우
> k1 = 50 & k2 = 80이 됨  

![image](https://github.com/mithzinf/DB-Study/assets/124668883/dcdc7d21-fb89-40a4-af86-90cd5d7a7f05)![image](https://github.com/mithzinf/DB-Study/assets/124668883/48bcb1a6-e40e-44a7-8b49-46970f5ba73d)




🪽**사고의 확장**  
위와 같은, B-tree 노드 그림을 '서버트리'화를 하게 된다면 다음과 같은 그림이 될 수 있다.   


![image](https://github.com/mithzinf/DB-Study/assets/124668883/e4ca844b-96d1-4466-8fd9-4c31fdd025bb)




1. B-tree 인덱스란?  
   1-1. B-Tree 인덱스 특징  

![image](https://github.com/mithzinf/DB-Study/assets/124668883/9c6204d8-940a-4c20-97b9-140d95b92870)    


- 특징  
  - 자녀 노드의 최대 갯수를 늘리기 위해 부모 노드에 key를 하나 이상 저장한다 (k1,k2)  
  - 부모 노드의 key들을 오름차순으로 정렬함  
  - 정렬된 순서에 따라 자녀 노드들의 key 값의 범위가 결정됨  


*이런 특징들을 활용하면, 자녀 노드의 최대 개수를 상황에 맞게 결정하여 활용할 수 있음*  


##### BST와의 차이점
- BST는 최대 2개까지의 자녀 노드를 가질 수 있었으나,
- B tree같은 경우에는 자녀 노드의 최대 갯수를 원하는 만큼 결정하여 사용할 수 있다





1-2. B-Tree의 주요 네 가지 파라미터  

![image](https://github.com/mithzinf/DB-Study/assets/124668883/cf6ff036-b47f-46dc-a090-47ac4a1f2c37)  


**M**  
각 노드의 최대 자식 노드 수  
최대 M개의 자식을 가질 수 있는 B-Tree를 M차 B-Tree라고 부름   

**M - 1**  
각 노드의 최대 키(key) 수  

**M / 2** (반올림)  
각 노드의 최소 자식 노드 수  
이 값은 루트 노드와 리프 노드를 제외한 모든 내부 노드에 해당  
ex. 3/2 -> 1.5 -> 2로 반올림해라  

**(M / 2 (반올림)) -1**  
각 노드의 최소 키 수  
이 값은 루트 노드에 해당 X  

**B-Tree 노드의 key 수와 자녀 수의 관계**  
internal 노드의 key 수가 x개라면, 자녀 노드의 수는 언제나 x+1개!!    





---



2. B-Tree 데이터 삽입

##### 데이터 삽입에 있어서 중요한 포인트
1. 삽입 위치는 ? : 데이터 추가는 항상 leaf 노드에 한다!!!  
2. 노드가 넘친다면? : 노드 넘침이란 '데이터를 삽입하려는 leaf 노드가 이미 최대 키 수를 가진 경우, 노드가 넘치게 되는 상황'
 - 이때 분할 작업을 수행해야 함  
3. 분할은 어떻게? : 노드가 넘치면, 가운데 key(Median key)를 기준으로 좌우 key들은 분할 & 가운데 key는 승진
  - 넘친 노드키 가운데, 가운데 key를 찾음
  - 가운데 키를 기준으로, 작은 값들은 좌측 하위 노드로, 큰 값들은 우측 하위 노드로 이동
  - 가운데 키는 상위 레벨 노드에 삽입





2-1. 3차 B-Tree 데이터 삽입 예시  

> 3차 B-Tree의 각 노드의 최대 key 수는 2개  
> 즉, 각 노드가 최대 2개의 키를 보유할 수 있음을 의미


STEP 1. 15 추가    
👉추가는 항상 LEAF 노드에 한다  

![image](https://github.com/mithzinf/DB-Study/assets/124668883/482de2fa-4c73-4006-a7d1-e2aebcd2cff7)    
👉이 자리는, 루트 노드이기도 하면서 리프 노드이기 때문에 15를 이 빈 자리에 삽입함  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/8b365873-7ab3-432d-b41d-64a0d095adc4)      



STEP 2. 2 추가  
👉추가는 항상 LEAF 노드에 한다   

![image](https://github.com/mithzinf/DB-Study/assets/124668883/95e3c74e-ff74-4849-ab05-dad71b752bd2)  
👉오름차순으로 정렬하여 2를 추가하여 저장해야 한다  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/c5b70dd1-ed5a-46b8-853a-10cb90413224)  
👉2까지 추가해놓고 보니, 노드가 넘친 것을 볼 수 있다


STEP 3. 노드 넘침에 대한 분할 작업 수행   

*HOW? : 가운데 KEY를 기준으로 좌우 KEY는 분할하고, 가운데 KEY는 승진*    
즉, 2는 승진이 되어야 하고 1&15는 좌우로 분할이 되어야 함


![image](https://github.com/mithzinf/DB-Study/assets/124668883/31304a7a-38c8-4501-b75d-d24e5239202d)  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/952068cd-b43d-4a2e-aa9e-3e8fe77fdbe5)    


STEP 4. 5 추가  
👉추가는 항상 LEAF 노드에 한다 

*✔️5를 어디에 넣어야 할지, 적합한 LEAF 노드를 찾는 것이 관건 : 즉, B-Tree에서 **조회**를 실시해야 한다는 의미*  


1. 2와 5를 비교 : 5가 더 큼 -> 우측 하위 노드로 빠짐  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/b407c4a5-a0dd-408b-8163-85d722589a4a)    
2. 정렬된 상태로, 값을 추가해줘야 하기 때문에 자리바꿈이 이뤄짐   
![image](https://github.com/mithzinf/DB-Study/assets/124668883/01476092-d04e-4057-8d20-2b2404af2c14)  
3. 최종ver  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/8326a25c-5c41-4f94-8e03-dd821e383cf3)


STEP 4. 30 추가  

1. 2와 30을 비교 : 30이 더 큼 -> 우측 하위 노드로 빠짐   
![image](https://github.com/mithzinf/DB-Study/assets/124668883/2779ff7d-a945-48dd-820e-bf444d0f8682)   
2. 우측 하위 노드는, 정렬된 상태로 값이 추가 되어야 함 : 5 , 15, 30 순으로 추가됨  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/7951aa8f-1d2a-41a2-9969-f47e612b1891)   
3. 🤯🤯맥시멈 노드 2개인데, 현재 노드 3개로 노드 넘침 발생🤯🤯  
   - 가운데 키 15를 기준으로, 5&30은 분할이 되고 15는 부모 노드로 승진함    
![image](https://github.com/mithzinf/DB-Study/assets/124668883/5e58ed1c-2577-40bb-ae03-84e5007c108b)  


STEP 4. 90 추가  

1. 2와 90을 비교 : 90이 더 큼 -> 우측 하위 노드로 빠짐  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/3fd7ae74-0b35-4bbc-8e17-ec7d632abf68)   


STEP 5. 20 추가   

1. 2와 20을 비교 : 20이 더 큼 -> 15와 20 비교 : 20이 더 큼 -> 우측 하위 노드로 빠짐  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/065966fa-15dd-4329-a469-122ce07dccb6)   
2. 우측 하위 노드는 정렬된 상태로 값이 추가가 되어야 함 : 20, 30, 90 순으로 추가  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/4da38c7e-52dc-4ee5-b9c6-ee12bfe97521)    
3. 🤯🤯맥시멈 노드 2개인데, 현재 노드 3개로 노드 넘침 발생🤯🤯  
   - 가운데 키 30을 기준으로, 20 & 90은 분할이 되고 30은 부모 노드로 승진함  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/de6a3ebc-4aa2-4839-86a7-6f24eefdccfd)  
4. 현재, 넘침 상태의 부모 노드도 한번 정리가 필요함  
   - 가운데 키 15를 기준으로, 2 & 30은 분할이 되고, 15는 그 윗 노드로 승진함  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/68c383bd-4f3b-48fa-857d-0487c0df1e1c)  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/d16e927f-d0b3-4c69-9331-0da720aed648)  
 
즉, 15가 새로운 부모 노드가 된 것!  



STEP 6. 7 추가   

1. 15와 7을 비교 : 15가 더 큼 -> 좌측 하위 노드로 빠짐  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/33c7bf3b-7589-4ab9-87d0-8027ea321f0e)   
2. 그 아래, 노드도 리프 노드가 아니기 때문에 비교 작업 한번 더 거쳐야 함 : 2와 7 비교 -> 7이 더 큼 -> 우측 하위 노드로 빠짐  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/767b0395-ebb5-4422-913e-9ae7262fd34a)   
3. 최종ver.   
![image](https://github.com/mithzinf/DB-Study/assets/124668883/293c75c5-7d34-4b67-96ad-d14ca9361d80)  


STEP 7. 9 추가    


1. 15와 9를 비교 : 15가 더 큼 -> 좌측 하위 노드로 빠짐   
![image](https://github.com/mithzinf/DB-Study/assets/124668883/a5c9d3df-08d7-4c4c-b17a-f583b6191586)    
2. 그 아래, 노드에서도 비교 작업을 한번 더 거침 : 2와 9 비교 -> 9가 더 큼 -> 우측 하위 노드로 빠짐   
![image](https://github.com/mithzinf/DB-Study/assets/124668883/9b2ffc27-6440-4ccc-9fbb-924e55c89123)    
3. 우측 하위 노드에서 정렬된 상태로 값이 추가가 되어야 함 : 5, 7, 9 순으로 추가   
![image](https://github.com/mithzinf/DB-Study/assets/124668883/5d6cc381-584f-45c7-a18a-0ac52b1bbd28)     
4. 🤯🤯맥시멈 노드 2개인데, 현재 노드 3개로 노드 넘침 발생🤯🤯   
   - 가운데 키 7 기준으로, 5 & 9은 분할이 되고 7은 그 윗 노드로 승진   
![image](https://github.com/mithzinf/DB-Study/assets/124668883/9c95a330-301b-4d67-9b9d-be24e5ee5c20)   


STEP 8. 8 추가    

1. 15와 8을 비교 : 15가 더 큼 -> 좌측 하위 노드로 빠짐   
![image](https://github.com/mithzinf/DB-Study/assets/124668883/beaabb7f-2e35-4f5d-b8aa-332da296a6c4)    
2. 그 아래, 노드에서도 비교 작업 한 번 더! : 2와 8을 비교 -> 8이 더 큼 -> 7과 8을 비교 -> 8이 더 큼 -> 우측 하위 노드로 삐짐  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/cf53919c-d952-46df-8c5b-c5af2c7a3f67)    
3. 우측 하위 노드는 정렬된 상태로 값이 추가가 되어야 함 : 8,9 순으로 추가    
![image](https://github.com/mithzinf/DB-Study/assets/124668883/35de3892-177d-41fe-839b-b7464f8b86eb)  


STEP 9. 10 추가   

1. 10과 15 비교 : 15가 더 큼 -> 좌측 하위 노드로 빠짐  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/6fbbc77f-83da-4deb-b54f-ad6ee7a7e910)   
2. 그 아래, 노드에서도 비교 작업 한 번 더! : 2와 10을 비교 -> 10이 더 큼 -> 7과 2차 비교 -> 10이 더 큼 -> 우측 하위 노드로 빠짐  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/21c36647-5d31-4227-bc3c-f4d8f4e5f00c)    
3. 우측 하위 노드에서 정렬된 상태로 값이 추가가 되어야 함 : 8,9,10 순으로 추가 -> 노드 넘침 발생    
![image](https://github.com/mithzinf/DB-Study/assets/124668883/23607443-85c0-4cd5-a5d5-a14e5cd407e4)    
4. 가운데 키 9 기준으로, 8 & 10은 분할이 되고 9는 그 윗 노드로 승진  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/95f9f8a6-269c-487d-a08d-1a9a3c7dd364)   
5. '2,7,9' 노드 넘침 발생으로 인해, 분할 작업 실행   
  - 가운데 키 7 기준으로, 2 & 9는 분할이 되고 7는 그 윗 노드로 승진    
![image](https://github.com/mithzinf/DB-Study/assets/124668883/9aae2b70-6083-47ff-a505-28a612b0e473)  


STEP 10. 50 추가    

![image](https://github.com/mithzinf/DB-Study/assets/124668883/1c45f29c-b997-479d-b823-82b18015c976)   



1. 7&15와 50 순차적 비교 : 두 수 보다 더 큼 -> 우측 하위 노드로 빠짐   
2. 그 아래, 노드에서도 비교 작업 한 번 더! : 30과 50 비교 -> 50이 더 큼 -> 하위 우측 노드로 빠짐  
3. 최종 ver.   

STEP 11. 70 추가   
![image](https://github.com/mithzinf/DB-Study/assets/124668883/7fb31e45-808c-48ce-8577-84f143604b85)  

1. 7&15와 70 순차적 비교 : 두 수 보다 더 큼 -> 우측 하위 노드로 빠짐   
2. 그 아래, 노드에서도 비교 작업 한 번 더! : 30과 70 비교 -> 70이 더 큼 -> 하위 우측 노드로 빠짐   
3. 우측 하위 노드에서 정렬된 상태로 값이 추가가 되어야 함 : 50,70,90 순으로 추가 -> 노드 넘침 발생   
4. 가운데 키 70 기준으로, 50 & 90은 분할이 되고 70은 그 윗 노드로 승진    



STEP 12. 60 추가   
![image](https://github.com/mithzinf/DB-Study/assets/124668883/8f86a26e-96b4-4292-83af-8edeee2f0b99)   

1. 7&15와 60 순차적 비교 : 두 수 보다 더 큼 -> 우측 하위 노드로 빠짐    
2. 그 아래, 노드에서도 비교 작업 한 번 더! : 30&70과 60 비교 -> 30<60<70 -> 하위 가운데 노드로 빠짐  




![image](https://github.com/mithzinf/DB-Study/assets/124668883/f8ade81e-b129-4989-bb1f-d3a4e6adfa10)   

진짜...   
마지....막......단계...  




STEP 13. 40 추가   


1. 7&15와 40 순차적 비교 : 두 수 보다 더 큼 -> 우측 하위 노드로 빠짐   
2. 그 아래, 노드에서도 비교 작업 한 번 더! : 30&70과 40 비교 -> 30<40<70 -> 하위 가운데 노드로 빠짐  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/7ccfe2e3-a961-4f2a-8cfc-18b96ac803de)   
3. 우측 가운데 노드에서 정렬된 상태로 값이 추가가 되어야 함 : 40,50,60 순으로 추가 -> 노드 넘침 발생   
4. 가운데 키 50 기준으로, 40 & 60은 분할이 되고 50은 그 윗 노드로 승진    
5. 그 윗 노드로 50이 승진 : 30,50,70 순으로 정렬 & 노드 넘침 발생   
![image](https://github.com/mithzinf/DB-Study/assets/124668883/4108d610-3560-4761-a0eb-f36a339db97d)     
6. 가운데 키 50 기준으로, 30 & 70은 분할이 되고 50은 그 윗 노드(부모 노드)로 승진   
![image](https://github.com/mithzinf/DB-Study/assets/124668883/6ba2d9db-4c77-4b31-b8b9-3eb37a9a611a)   
7. 부모 노드 - 노드 넘침 발생    
![image](https://github.com/mithzinf/DB-Study/assets/124668883/37c01231-2f39-4c21-94ea-987903aaba6a)    
8. 가운데 키 15 기준으로, 7 & 50은 분할이 되고 15는 그 윗 노드로 승진  

9. **3차 B-Tree 데이터 삽입 최종 ver**  
![image](https://github.com/mithzinf/DB-Study/assets/124668883/591b7330-3c2e-4ed2-9b09-cbd8ab9af540)  




위와 같은 예시로 알 수 있는 B-Tree의 특징 : 모든 leaf 노드들은 같은 레벨에 있다!






























 
 
