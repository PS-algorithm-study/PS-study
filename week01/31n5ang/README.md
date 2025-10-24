# Lv3. 봉인된 주문
> [프로그래머스 > 2025 코드챌린지 2차 예선 > 봉인된 주문](https://school.programmers.co.kr/learn/courses/30/lessons/389481) <br />
> 유형: 수학, 구현, 문자열

## 문제 풀이
주문서의 차례는 다음과 같이 증가한다.
- 1: `a`
- 2: `b`
- ..
- ..
- 26: `z`
- 27: `aa`
- ..
- ..
- 52: `az`
- 53: `ba`
- ...

알파벳의 개수는 26개이고, 이전 자리수가 모두 `z`로 채워질때 현재 자리수의 알파벳이 증가한다. 그러므로 26진수와 같다. 금지된 주문서만큼 순서도 앞으로 당겨지므로, `n`번 째 주문서를 찾아야할 때 `ban[i]`이 영향을 줄 수 있는지 판단하고, 영향을 준다면 찾아야할 `n`번째 주문서는 원래의 `n+1`번째 주문서가 된다. 

예를 들어 `n=5` 일때 원래의 주문서는 `e`이다. 하지만 `b`주문서를 금지한다면, `n`번째 주문서가 아닌 `n+1`주문서가 `n`번째가 되므로, `f`가 된다.

영향을 주는 금지된 주문서만큼 `n`순서를 `x`만큼 앞당긴 순서를 `y`라고 할 때, `y`의 값으로 원래 주문서 문자열로 변환하는 과정에서 주의해야할 점이 있다. `y`를 무턱대고 26진법을 활용해 변환해버리면, `a`와 `z`의 값이 모호해질 수 있다. 그러므로 **첫번째 주문서는 `0`이 아닌 `1`임을 생각해야 한다.**

## 최종 코드
```java
import java.util.*;

class Solution {
    public String solution(long n, String[] bans) {
        Arrays.sort(bans);
        List<Long> banList = new ArrayList<>();
        for (String x : bans) {
            long order = 0;
            long p = 1;
            for (int i = x.length() - 1; i >= 0; i--) {
                order += p * (x.charAt(i) - 'a' + 1);
                p *= 26;
            }
            banList.add(order);
        }
        Collections.sort(banList);
        long pulled = 0;
        long baned = 0;
        for (long ban : banList) {
            if (ban - pulled <= n) {
                baned++;
            } else {
                break;
            }
            pulled++;
        }
        
        long resOrder = n + baned;
        StringBuilder sb = new StringBuilder();
        while (resOrder > 0) {
            resOrder--;
            int alpha = (int)(resOrder % 26);
            sb.append((char)('a' + alpha));
            resOrder /= 26;
        }
        
        return sb.reverse().toString();
    }
}
```
