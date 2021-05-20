## JobScheduling

일에 대한 효율을 따질 때 가장 중요한 것을 꼭 집어 고르기는 어렵다. 각 기계 별로 능력도 다르고, 기계라는 것은 언제 고장날지 알 수 없는 일이라 실제 일을 할 때 어떤 기계에게 어떤 일을 얼마나 맡길지 완벽하게 결정하는 것은 어려운 일이다. 하지만 주어진 문제인

\* n = [4, 8, 16] (작업의 갯수)

\* m = 2 (기계의 갯수)

/*

또는

\* n = [2, 3, 4, 5]

\* m = 2  

*/

를 따져볼 때에는 각 기계 별 효율과 기계의 고장 등 여러 요소들에 대해 고려하지 않아도 된다. 따라서 단순하게 생각해서, 어떻게 해도 가장 늦게 끝날 일이, 어떻게 하면 가장 빨리 끝날 수 있을까를 고민하면 된다. 

저번에 풀었던 문제인 배낭 문제에서, 부분 배낭 문제와 그냥 배낭 문제의 차이를 고려해 봤을 때 그리디 알고리즘을 사용할 수 없는 경우는 그리디 알고리즘이 현재 상태에서 최적의 수를 선택할 뿐 결과가 최선이 되도록 할 수 없는 경우임을 알았는데, 이 경우에도 그런지 아닌지 따져볼 필요가 있겠다.

문제에는 n이 작업의 갯수를 나타낸다고 적혀있는데, 무슨 뜻인지 잘 모르겠지만 [4, 8, 16]같은 경우 한번에 4개의 일을 한 기계에게 동시에 맡겨야 하는 것인 것 같고, 따라서 1개의 일은 모두 수행 시간이 동일하다고 가정하고 작업 시간으로 변경하여 풀이하기로 하였다. 

(만약 그렇게 푸는 것이 아님을 알게 된다면 나중에 수정하기로 하자.)

### bruteforce

그리디 알고리즘이 내린 결론이 최적의 결과인지 아닌지 구분하는 방법으로, bruteforce 방식으로 최적의 해를 구한 것과 동일한 결과가 나오는가 아닌가를 따져보는 방법이 있다. bruteforce 방식으로는 모든 해를 구해 비교하는 것이기 때문에 확실하게 결론이 최적인 상황을 뽑아낼 수 있기 때문이다.

\* n = [2, 3, 4, 5]

\* m = 2  

먼저 이 문제를 풀어보겠다.

기계 m1과 기계 m2가 있다고 하자. 이 때 m1 이나 m2 둘 중에 한 기계는 반드시 작업 시간 2인 일을 맡아야 한다. m1에게 이 일을 할당하나 m2에게 이 일을 할당하나 결과에는 영향이 가지 않으므로 m1에게 이 일을 맡기는 상황만 고려하기로 하겠다.(물론 내가 bruteforce 알고리즘을 이용해서 코드를 짠다면 이런 점을 고려하여 작동하는 코드가 만들어지지 않고 m1에게 2를 맡기지 않는 상황까지 모두 고려해서 기계 한 대가 2개의 일을 맡는 경우 총 6개의 결과가 나올 것이고, 아래 문제에서는 표본이 적어 그리 많은 시간이 걸리지 않지만 갯수가 많을 경우에는 횟수를 절반만큼 감소시킬 수 있으므로...)

/*

이 때, 기계 m2에게 바로 일을 맡기기 보단, 총 4개의 일이니 한 기계가 2개의 일을 맡는 것 보다 3개의 일을 맡는 것이 더 효율적이진 않는지 확인해야 한다. 5가 가장 오래 걸리는 일이기 때문에 한 기계가(이 경우 2를 m1이 맡기로 하였으므로 이 기계가 m1이 된다.) 9만큼의 시간으로 가장 오래 걸려서 일을 끝마치기 때문에 9 만큼의 시간이 걸린다고 할 수 있겠다.
한 기계가 2개의 일을 맡는 것으로 돌아와서, m1이 2 다음으로 시간이 적게 걸리는 일인 3을 맡을 경우에는 첫 번째 일과 두 번째 일이 모두 m1의 것보다 m2의 것이 길기에 말이 되지 않으므로 , m1이 4를 맡는 상황과 5를 맡는 상황을 비교하면 된다. m1이 4를 맡는 경우에는 m1이 총 2+4, 6의 시간이 걸려 일을 해내게 되고, m2는 3과 5의 일을 맡으므로 총 8의 시간이 걸려 일을 해내게 된다. 따라서 일을 끝마치는데 걸리는 시간은 8이 된다.
m1이 2를 끝마치고 5를 맡았을 때에는 m1은 7의 시간이, m2는 3이 끝나고 4를 맡기 때문에 마찬가지로 7의 시간이 걸리게 되어 총 7의 시간이 걸리게 된다.

*/

가장 적게 걸리는 방법을 찾는 생각을 길게 풀어서 썼지만, 순수하게 모든 경우를 다 고려해 보면

m1이 일을 하나만 맡을 때의 경우는 [2|3+4+5]>12 이고, 두 개를 맡을 때에는 [2+3|4+5]->9, [2+4|3+5]->8, [2+5|3+4]->7 그리고 m1이 일을 세게 맡을 경우에는 [2+3+4|5]->9, [2+3+5|4]->10, [2+4+5|3]->11이 돼서 결론은 [3+5|3+4]->7이 된다.

### greedy algorithm

그리디 알고리즘 같은 경우에는, 일단 순서대로 일을 맡긴 다음 먼저 일이 끝나는 기계에게 바로 다음 일을 부여한다. 물론 일이 가장 빨리 끝난 기계는 빠르게 끝난 만큼 더 일을 하라고 남아있는 일 중에서 가장 작업 시간이 긴 일을 줄 수도 있겠지만 일반적인 경우에는 남아있는 일을 순서대로 부여한다. 

\* n = [2, 3, 4, 5]

\* m = 2  

앞과 마찬가지로 m1과 m2가 있다고 하고,  일이 끝나고 새로운 일을 할 차례가 왔을 때에는 남아있는 일을 순서대로 맡는다고 했을 때, m1이 2, m2가 3을 맡고 나면 m1이 먼저 2를 끝마치고 4를 맡게 되고, m1이 일 4를 1만큼 진행했을 때 m2가 맡은 3이 끝나므로 m2가 남아있는 5를 맡는 방식으로 끝이 나게 된다. 

이 경우 3+5 총 8의 시간이 걸리게 되는데, 앞서 bruteforce 방식으로 진행한 것과 다른 결과, 즉 최적의 결과가 아닌 결과가 나오게 된다.

### 더 나은 방법

bruteforce 방식은 표본이 많아질 수록 크게 영향을 받기 때문에 일반적으로 다른 방식이 존재한다면 다른 방식을 이용하는 것이 좋다. 비록 방금의 상황에서는 greedy algorithm이 최적의 결과를 찾지 못했기 때문에 bruteforce방식을 채용할 이유가 있지만, 표본이 너무 많다면 조금의 오차를 감안하고 greegy algorithm을 이용할 수도 있는 것이다.
하지만 이렇게 표본이 적은데도 오차가 발생하는 것은 문제가 있다. 물론 코드 짜기는 더 복잡해지겠지만 일이 더 빨리 끝난 기계에게 더 오래 걸리는 일을 부여하는 것은 상식이다. 물론 조금 더 오차를 줄일 수 있을 것이다. 하지만 더 좋은 방법은, 간이 정수기를 만들 때 처럼 먼저 오래 걸리는 일을 수행한 다음 적게 걸리는 일을 순차적으로 실행하는 것이다.
예를 들어서, 맨 처음 실행했던 방식은 기계가 m1, m2 두개에 일이 2, 29, 1, 30 이런 식으로 일이 배치되어있을 때, m1이 2->1을 맡고 m2가 30->29를 맡게 되어 총 59만큼의 시간이 걸리게 된다.  가장 적게 걸리는 경우는 31인데, 차이가 심하게 난다고 볼 수 있다.

더 빨리 끝난 기계에게 더 오래 걸리는 일을 부여하는 경우에는 그래도 큰 문제는 생기지 않고 m1이 2->30, m2가 29->1을 맡게 되어 총 32가 되는데, 보면 알겠지만 평균과의 오차를 줄여나갈 기회를 처음부터 날려버렸다. 따라서 더 오래 걸리는 일을 먼저 수행하면 이런 점을 보완할 수 있다.
그렇기에 먼저 일을 내림차순으로 정렬한 다음 처음 방식대로 진행하면 치명적인 상황은 생기지 않게 된다.

### 시간복잡도

작업의 개수를 n개, 기계의 개수를 m대 라고 할 때, bruteforce 방식의 경우 O(nCm)/*문자 2개인건 어떻게 표시하는지 잘 모르겠는데 이게 맞는지 모르겠다.) 만큼의 시간이 걸리고, greegy algorithm 같은 경우에는  그냥 O(m) 만큼 시간이 걸린다.