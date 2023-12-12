*해당 코드는 직접 작성한 코드입니다.<br>
완벽한 코드가 아닌, 공부 내용을 기반으로 작성한 코드입니다.*

피드백 언제든지 환영합니다 😊
 
# 프로그래머스 LV1 문제<br>
[[카카오 인턴] 키패드 누르기](https://school.programmers.co.kr/learn/courses/30/lessons/67256)
<br>

### 문제
스마트폰 전화 키패드의 각 칸에 다음과 같이 숫자들이 적혀 있습니다.
![도움 이미지](kakao_phone1.png)
이 전화 키패드에서 왼손과 오른손의 엄지손가락만을 이용해서 숫자만을 입력하려고 합니다.<br>
맨 처음 왼손 엄지손가락은 * 키패드에 오른손 엄지손가락은 # 키패드 위치에서 시작하며, 엄지손가락을 사용하는 규칙은 다음과 같습니다.<br>
<br>
* 엄지손가락은 상하좌우 4가지 방향으로만 이동할 수 있으며 키패드 이동 한 칸은 거리로 1에 해당합니다.<br>
* 왼쪽 열의 3개의 숫자 1, 4, 7을 입력할 때는 왼손 엄지손가락을 사용합니다.<br>
* 오른쪽 열의 3개의 숫자 3, 6, 9를 입력할 때는 오른손 엄지손가락을 사용합니다.<br>
* 가운데 열의 4개의 숫자 2, 5, 8, 0을 입력할 때는 두 엄지손가락의 현재 키패드의 위치에서 더 가까운 엄지손가락을 사용합니다.<br>
  * 만약 두 엄지손가락의 거리가 같다면, 오른손잡이는 오른손 엄지손가락, 왼손잡이는 왼손 엄지손가락을 사용합니다.<br>
순서대로 누를 번호가 담긴 배열 numbers, 왼손잡이인지 오른손잡이인 지를 나타내는 문자열 hand가 매개변수로 주어질 때, 각 번호를 누른 엄지손가락이 왼손인 지 오른손인 지를 나타내는 연속된 문자열 형태로 return 하도록 solution 함수를 완성해주세요.<br>
순서대로 누를 번호가 담긴 배열 numbers, 왼손잡이인지 오른손잡이인 지를 나타내는 문자열 hand가 매개변수로 주어질 때, 각 번호를 누른 엄지손가락이 왼손인 지 오른손인 지를 나타내는 연속된 문자열 형태로 return 하도록 solution 함수를 완성해주세요.<br>

제한사항
* numbers 배열의 크기는 1 이상 1,000 이하입니다.<br>
* numbers 배열 원소의 값은 0 이상 9 이하인 정수입니다.<br>
* hand는 "left" 또는 "right" 입니다.<br>
  * "left"는 왼손잡이, "right"는 오른손잡이를 의미합니다.<br>
* 왼손 엄지손가락을 사용한 경우는 L, 오른손 엄지손가락을 사용한 경우는 R을 순서대로 이어붙여 문자열 형태로 return 해주세요.<br>

입출력 예
numbers | hand | result
-|-|-
[1, 3, 4, 5, 8, 2, 1, 4, 5, 9, 5] | "right"	| "LRLLLRLLRRL"
[7, 0, 8, 2, 8, 3, 1, 5, 7, 6, 2] | "left" | "LRLLRRLLLRR"
[1, 2, 3, 4, 5, 6, 7, 8, 9, 0] | "right" | "LLRLLRLLRL"



### 문제 풀이

```java
class Solution {

    //손가락의 현재 위치
    private static int[] leftFinger = {3, 0};
    private static int[] rightFinger = {3, 2};
    
    public String solution(int[] numbers, String hand) {
        
        //0, 1, 2, 3, 4, 5, 6, 7, 8, 9 의 위치
        int[][] location = {
            {3, 1}, 
            {0, 0}, {0, 1}, {0, 2}, 
            {1, 0}, {1, 1}, {1, 2}, 
            {2, 0}, {2, 1}, {2, 2}
        };

        //"L" 또는 "R"을 계속 추가할 것 이므로 String(불변) 대신 StringBuilder 이용 
        StringBuilder answer = new StringBuilder();

        for (Integer checkNum : numbers) {

            //Switch Expression으로 반환받은 값을 StringBuilder 객체에 추가
            answer.append(switch(checkNum) {
                case 1, 4, 7 -> { 
                    leftFinger = location[checkNum];
                    yield "L";
                }
                case 3, 6, 9 -> {
                    rightFinger = location[checkNum];
                    yield "R";
                }
                default -> {
                    int[] temp = location[checkNum];
                    int leftCount = Math.abs(temp[0] - leftFinger[0]) + Math.abs(temp[1] - leftFinger[1]);
                    int rightCount = Math.abs(temp[0] - rightFinger[0]) + Math.abs(temp[1] - rightFinger[1]);

                    if (leftCount < rightCount || (leftCount == rightCount && hand.equals("left"))) {
                        leftFinger = location[checkNum];
                        yield "L";
                    } else {
                        rightFinger = location[checkNum];
                        yield "R";
                    }
                }
            });
        }
        
        return answer.toString();
    }
}
```