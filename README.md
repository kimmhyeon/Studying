# 요약정리

## dp
### 최장 증가 부분 수열(LIS, Longest Increasing Subsequence)
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

