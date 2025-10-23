# Lv3. 봉인된 주문
> [프로그래머스 > 2025 코드챌린지 2차 예선 > 봉인된 주문](https://school.programmers.co.kr/learn/courses/30/lessons/389481) <br />
> 유형: 수학, 구현, 문자열

## 문제 풀이


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
