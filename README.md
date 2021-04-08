# ShortestPath

## 1. 최단 경로(Shortest Path) 문제
- 주어진 가중치 그래프에서 어느 한 출발점에서 또 다른 도착점까지의 최단 경로를 찾는 문제입니다.
- 최단 경로를 찾는 가장 대표적인 알고리즘으로는 다익스트라 최단 경로 알고리즘이 있습니다.

## 2. 다익스트라 알고리즘
![image](https://user-images.githubusercontent.com/80517119/113986755-8b998380-9888-11eb-99c7-639aadede733.png)
사진 출처 https://namu.wiki/w/다익스트라%20알고리즘
- 주어진 출발점에서 시작합니다.
- 출발점으로부터 최단 거리가 확정되지 않은 점들 중에서 출발점으로부터 가장 가까운 점을 추가하고, 그 점의 최단 거리를 확정합니다.
---
## 3. 예제
- 가중치 -> 2차원 배열에 저장g[][]인 2차원 배열에 저장한다.
- 배열 d[] -> 가장 최단 거리를 저장한다.
- visited[] --> 방문여부를 체크하는 배열이다.
1. 가장 최단 거리를 저장하는 d를 큰 수로 초기화 해주신다. 단, d[start] = 0
    ```
    public static int d[] = {INF,INF,INF,INF,INF,INF,INF,INF,INF,INF};
    public static void dijkstra ( int start){...d[start]=0;... }
    ```
2. 가장 최소 거리를 가는 정점을 반환하는 함수이다.
   ```
   public static int getSmallIndex(){
        int min = Integer.MAX_VALUE;
        int min_Index = -1;
        for(int i =0;i<10;i++){
            if( !visited[i]&& d[i]!=INF){
                if(d[i]<min) {
                    min = d[i];
                    min_Index = i;
                }
            }
        }
        return min_Index;
    }
    ```
    - ```for``` : 아직 방문하지 않았고 ```d[i]```가 ```INF```가 아닌 것 중에서 ```min```보다 ```d[i]```가 더 작을 떄,  ```min```과 그 ```index```를 계속 갱신해준다.

3. 다익스트라를 수행하는 함수
    ```
    public static void dijkstra ( int start){
        visited[start]=true;
        d[start]=0;
        for(int i =0; i<10; i++){
            if(!visited[i]){
                d[i]= g[start][i];
            }//-->1
        }

        for(int i =0;i<9;i++){
            int current = getSmallIndex();
            visited[current] = true;//-->2
            for(int j =0; j<10;j++){
                if(!visited[j]&&g[current][j]!=0){
                    if(d[current]+g[current][j]< d[j]){
                        d[j]=d[current]+g[current][j];
                    }//-->3
                }
            }
        }
    }
    ```
    - 1번의 경우 : 아직 방문하지 않은 정점의 가중치 값을 ```d[i]```에 넣는다.
    - 2번의 경우 :```getsmallIndex()```을 이용해서 가장 최소 거리의 정점을 ```current```에 넣어준다. 또한 ```visited[current]```에 방문표시를 해주고 다시 ```for```을 돌린다.
    - 3번의 경우 :```current```의 자기자신의 가중치(0)를 제외하고 방문하지 않은 점들 중. 다른 점을 거쳐서 가는 가중치보다 바로 가는 가중치가 더 클경우 ```d[j]```을 더 작은 값으로 갱신한다.
---
### 전체코드
![image](https://user-images.githubusercontent.com/80517119/114003741-f8b51500-9898-11eb-93c6-240cdf6db500.png)
   ```
   public class ShortPath {
    public static int INF = 10000000;
    public static int g[][] = {
            {0,12,15,INF,INF,INF,INF,INF,INF,INF},
            {12,0,INF,INF,4,10,INF,INF,INF,INF},
            {15,INF,0,21,INF,INF,7,INF,INF,INF},
            {INF,INF,21,0,INF,INF,INF,25,INF,INF},
            {INF,4,INF,INF,0,3,INF,INF,13,INF},
            {INF,10,INF,INF,3,0,10,INF,INF,INF},
            {INF,INF,7,INF,INF,10,0,19,INF,9},
            {INF,INF,INF,25,INF,INF,19,0,INF,5},
            {INF,INF,INF,INF,13,INF,INF,INF,0,15},
            {INF,INF,INF,INF,INF,INF,9,5,15,0}
    };
    public static boolean visited[]= new boolean[10];//방문한 노드
    public static int d[] = {INF,INF,INF,INF,INF,INF,INF,INF,INF,INF};//최단거리 저장
    //가장 최소 거리를 가지는 정점을 반환한다.
    public static int getSmallIndex(){
        int min = Integer.MAX_VALUE;
        int min_Index = -1;
        for(int i =0;i<10;i++){
            if( !visited[i]&& d[i]!=INF){
                if(d[i]<min) {
                    min = d[i];
                    min_Index = i;
                }
            }
        }
        return min_Index;
    }
    //다익스트라를 수행하는 함수
    public static void dijkstra ( int start){
        visited[start]=true;
        d[start]=0;
        for(int i =0; i<10; i++){
            if(!visited[i]){
                d[i]= g[start][i];
            }//-->1
        }

        for(int i =0;i<9;i++){
            int current = getSmallIndex();
            visited[current] = true;//-->2
            for(int j =0; j<10;j++){
                if(!visited[j]&&g[current][j]!=0){
                    if(d[current]+g[current][j]< d[j]){
                        d[j]=d[current]+g[current][j];
                    }//-->3
                }
            }
        }
    }

    public static void main(String[] arg){

        dijkstra(0);
        for(int i =1;i<10;i++){
            System.out.print(d[i]+" ");
        }
    }
}
```
---
### 실행 결과
![image](https://user-images.githubusercontent.com/80517119/114003653-e2a75480-9898-11eb-8fdd-904556ddd70c.png)

![image](https://user-images.githubusercontent.com/80517119/114003926-200be200-9899-11eb-9ef4-a4adb796ca4a.png)

## 4. 성능비교 
-  for문이 (n-1)번 반복될때 
   * 1번 반복 될때 getsmallIndex()에서 min_index를 찾는데 O(n)시간이 걸린다.
   * min_index에 연결된 점의 수가 최대 (n-1)개 이므로, 각 d[w]를 갱신하는데 걸리는 시간은 O(n)이다.

-> **시간 복잡도는 (n-1)x(O(n)+O(n)) = O(n^2)이다.**

-  우선순위 큐를 이용하면 시간복잡도는 O(logn)로 훨씬 성능이 좋아진다.
---
### 글 마무리
- 우리 A팀은 총 4명이지만 그 중 한현진과 이근영만 과제를 함께 하였습니다. 각자 코드를 짰지만 완성도가 있는 이근영 코드로 과제를 제출합니다. readme작성의 경우 서로 도와가며 완성하였습니다.
