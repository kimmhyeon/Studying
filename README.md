# 알고리즘 공부

## 주의점
>StringTokenizer의 countTokens()는 O(len) -> 시간 잡아먹으므로 사용자제  
>ArrayList의 get()은 O(1), LinkedList의 get()은 O(n) -> get() 사용할 일 있으면 ArrayList사용 추천  


## DP 예제
>### LIS (Longest Increasing Subsequence, 최장 증가 부분 수열)
>>이전 dp와 참고하여 더 크면 1증가하여 부여
>>수행시간 O(n^2)
>>
>>```java
>>for (int i = 0; i < arr.length; i++){
>>    length[i] = 1;
>>    for (int j = 0; j < i; j++){
>>        if(arr[i] > arr[j]){
>>            dp[i] = max(dp[i], dp[j] + 1);
>>        }        
>>    }
>>}
>>```
>>이분탐색 사용하면 수행시간 O(n log n)으로 단축 가능  
>>
>>```java
>>if (arr.size() == 0)
>>    arr.add(num);
>>else if (arr.get(arr.size()-1) < num)  // 리스트의 가장 마지막 값 보다 크면 추가
>>    arr.add(num);
>>else                                  // lower_bound()를 이용하여 해당 인덱스값 변경
>>    arr.set(lowerBound(num), num);
>>```
>>>
>>>lower_bound() : 이진탐색기반, key값 보다 <U>크거나 같은</U> 첫 번째 주소 반환   
>>>upper_bound() : 이진탐색기반, 처음으로 value값 <U>초과</U>하는 주소 반환
>>>```java
>>>int lowerBound(int key){
>>>    int start=0, end=arr.size()-1, idx=Integer.MAX_VALUE;
>>>
>>>    while (start <= end){
>>>        int mid = (start+end)/2;
>>>
>>>        if (arr.get(mid) >= key){
>>>            end = mid -1;
>>>            idx = Integer.min(idx, mid);
>>>        }
>>>        else
>>>            start = mid +1;
>>>    return idx;
>>>}	
>>>```
>
>### LCS (Longest Common Subsequence, 최장 공통 부분 수열)
>>
>>```java
>>// 입력
>>input1 = br.readLine();
>>input2 = br.readLine();
>>
>>// 초기화
>>dp = new int[input1.length()+1][input2.length()+1];
>>	
>>// LCS
>>for (int i=1; i<=input1.length(); i++){
>>  for (int j=1; j<=input2.length(); j++){
>>	if(input1.charAt(i-1) == input2.charAt(j-1))
>>	    dp[i][j] += 1 + dp[i-1][j-1];
>>	else
>>	    dp[i][j] = Math.max(dp[i][j-1], dp[i-1][j]);
>>    }
>>}
>>```
>>
>### 길찾기 경우의수
>>dp를 이용하여 계산  
>### 냅섹문제
>>(이전 무게의 가중치)와 (현재 아이템의 가중치 + 남은 무게는 dp참고) 최대값 비교
>>
>>```java
>>// 아이템 개수
>>for (int i=0; i<n; i++){
>>    // 무게마다
>>    for (int j=0; j<=k; j++){
>>    	// 첫번째 아이템의 경우
>>	if (i==0 && j - itemList.get(i).weight >= 0)
>>	    dp[i][j] = itemList.get(i).value;
>>	// 2번째 아이템 부터
>>	else if (i > 0){
>>	    dp[i][j] = dp[i-1][j];
>>	    if (j - itemList.get(i).weight >= 0)
>>		dp[i][j] = Math.max(dp[i][j], itemList.get(i).value + dp[i-1][j - itemList.get(i).weight]);
>>		}
>>    }
>>}
>>```

## 최단거리
>### 벨만포드
>>모든간선을 전부 확인 (음수 간선이 있어도 최적의 해 탐색)  
>>v(정점)-1번 반복하여 최단거리 계산  
>>1번더 즉 v번 탐색하면 음수 간선 순환 존재여부 확인 가능  
>>```java
>>static boolean bellman_ford(){
>>        for (int cnt=1; cnt<minCost.length; cnt++){
>>            for (int start=1; start<minCost.length; start++){
>>                for (Node node : map[start]){
>>                    int dst = node.dst;
>>                    int cost = node.cost;
>>                    if (minCost[start] != Integer.MAX_VALUE && minCost[dst] > minCost[start] + cost) {
>>                        if (cnt == minCost.length-1)
>>                            return true;
>>                        minCost[dst] = minCost[start] + cost;
>>                    }
>>                }
>>            }
>>        }
>>        return false;
>>}
>>```
>### 플루드이드와샬
>>모든 정점에서 모든 도착점에 대한 최단거리 계산  
>>(시작 ~ 도착) 과 (시작 ~ 경유) + (경유 ~ 도착) 비교  
>>경유-시작-도착 순으로 3중 for문  
>>```java
>>static void floyd_warshall(){
>>        for (int via=1; via<minCost.length; via++){
>>            for (int start=1; start<minCost.length; start++){
>>                for (int dst=1; dst<minCost.length; dst++){
>>                    if (minCost[start][via]!=Integer.MAX_VALUE && minCost[via][dst]!=Integer.MAX_VALUE && minCost[start][dst] > minCost[start][via] + minCost[via][dst])
>>                        minCost[start][dst] = minCost[start][via] + minCost[via][dst];
>>                }
>>            }
>>        }
>>}
>>```
>>

## 문자열 알고리즘
>### KMP 알고리즘
>>
>>failure 함수 : 찾을 문자열을 갖고 접두사와 접미사를 비교해 얼마나 같은지 기록해 두는 함수
>>```java
>>static int[] failure(String pattern){
>>	int[] failure = new int[pattern.length()];
>>	for (int i=1, j=0; i<failure.length; ){
>>	    if (pattern.charAt(i) == pattern.charAt(j))
>>		failure[i++] = ++j;
>>	    else if (j == 0)
>>		i += 1;
>>	    else
>>		j = failure[j-1];
>>	}
>>
>>	return failure;
>>}
>>```
>>kmp : failure 함수를 갖고 실패시 해당 값만큼 이동하여 탐색
>>```java
>>for (int i=0, j=0; i<sentence.length(); ){
>>    // Log
>>    // System.out.println("i:" + i + " j:"+j);
>>    if (sentence.charAt(i) == pattern.charAt(j)){
>>    	i += 1;
>>    	if (j == pattern.length()-1){
>>	        indexList.add(i-j);
>>	        j = failure[j];
>>	    }
>>	    else{
>>	        j += 1;
>>	    }
>>    }
>>    else {
>>        if (j>0)
>>            j = failure[j - 1];
>>	      else
>>	          i += 1;
>>    }
>>}
>>```

## 누적합
>### IMOS
>>매 쿼리마다 일일히 갱신하지 않고 입장(+1)과 퇴장(-1)만 기록해 두고 추후에 한번에 계산(누적합)하여 시간절약  
>>
>>>#### 1차원의 경우 
>>>|   |   |   |   |   |   |
>>>|---|---|---|---|---|---|
>>>|0|**1**|**1**|**1**|0|0|
>>>
>>> ↑ 위를 표현하고 싶다면 ↓
>>> 
>>>|   |   |   |   |   |   |
>>>|---|---|---|---|---|---|
>>>|0|**1**|**0**|**0**|-1|0|  
>>>
>>>오른쪽으로 누적합을 계산
>>>
>>>|   |   |   |   |   |   |
>>>|---|---|---|---|---|---|
>>>|0|**1**|**1**|**1**|0|0|
>>
>>
>>>#### 2차원의 경우
>>>|   |   |   |   |   |   |
>>>|---|---|---|---|---|---|
>>>|0|**1**|**1**|**1**|0|0|
>>>|0|**1**|**1**|**1**|0|0|
>>>|0|**1**|**1**|**1**|0|0|
>>>|0|0|0|0|0|0|
>>>
>>>↑ 위를 표현한 방식은 ↓
>>>|   |   |   |   |   |   |
>>>|---|---|---|---|---|---|
>>>|0|**1**|**0**|**0**|-1|0|
>>>|0|**0**|**0**|**0**|0|0|
>>>|0|**0**|**0**|**0**|0|0|
>>>|0|-1|0|0|1|0|
>>>
>>>오른쪽으로 누적합 계산 후 아래로 누적합 계산하면 된다.
>>>
>>>|   |   |   |   |   |   |
>>>|---|---|---|---|---|---|
>>>|0|**1**|**1**|**1**|0|0|
>>>|0|**0**|**0**|**0**|0|0|
>>>|0|**0**|**0**|**0**|0|0|
>>>|0|-1|-1|-1|0|0|
>>>
>>>↑오른쪽으로 누적합 계산한 모습
>>>
>>>|   |   |   |   |   |   |
>>>|---|---|---|---|---|---|
>>>|0|**1**|**1**|**1**|0|0|
>>>|0|**1**|**1**|**1**|0|0|
>>>|0|**1**|**1**|**1**|0|0|
>>>|0|0|0|0|0|0|
>>>
>>>↑세로로 누적합 계산한 모습


