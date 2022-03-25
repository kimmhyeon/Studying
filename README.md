# 알고리즘 요약정리



## DP
### 최장 증가 부분 수열 (LIS, Longest Increasing Subsequence)
이전 dp와 참고하여 더 크면 1증가하여 부여

https://www.acmicpc.net/problem/11053
```
for (int i = 0; i < arr.length; i++){
	length[i] = 1;
    for (int j = 0; j < i; j++){
        if(arr[i] > arr[j]){
            dp[i] = max(dp[i], dp[j] + 1);
        }        
    }
}
```

### 길찾기 경우의수
dp를 이용하여 계산

### 냅섹문제
(이전 무게의 가중치)와 (현재 아이템의 가중치 + 남은 무게는 dp참고) 최대값 비교

https://www.acmicpc.net/problem/12865
```
// 아이템 개수
for (int i=0; i<n; i++){
    // 무게마다
    for (int j=0; j<=k; j++){
    	// 첫번째 아이템의 경우
	if (i==0 && j - itemList.get(i).weight >= 0)
	    dp[i][j] = itemList.get(i).value;
	// 2번째 아이템 부터
	else if (i > 0){
	    dp[i][j] = dp[i-1][j];
	    if (j - itemList.get(i).weight >= 0)
		dp[i][j] = Math.max(dp[i][j], itemList.get(i).value + dp[i-1][j - itemList.get(i).weight]);
	}
    }
}
```

## 문자열 알고리즘
### KMP 알고리즘
https://www.acmicpc.net/problem/1786

failure 함수 : 찾을 문자열을 갖고 접두사와 접미사를 비교해 얼마나 같은지 기록해 두는 함수
```
static int[] failure(String pattern){
	int[] failure = new int[pattern.length()];
	for (int i=1, j=0; i<failure.length; ){
	    if (pattern.charAt(i) == pattern.charAt(j))
		failure[i++] = ++j;
	    else if (j == 0)
		i += 1;
	    else
		j = failure[j-1];
	}

	return failure;
}
```
kmp : failure 함수를 갖고 실패시 해당 값만큼 이동하여 탐색
```
for (int i=0, j=0; i<sentence.length(); ){
    // Log
    // System.out.println("i:" + i + " j:"+j);
    if (sentence.charAt(i) == pattern.charAt(j)){
	i += 1;
	if (j == pattern.length()-1){
	    indexList.add(i-j);
	    j = failure[j];
	}
	else{
	    j += 1;
	}
    }
    else {
	if (j>0)
	    j = failure[j - 1];
	else
	    i += 1;
    }
}
```




