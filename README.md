# Make-a-Maze

https://www.acmicpc.net/problem/2665

# 구현 방법
```
 i)   이차원 배열 length를 생성한다. length[i,j]에 저장되는 값은 (i,j)까지 오는데 지나온 막힌 칸의 최소 개수이다.
 
 ii)  BFS를 이용하여 (0,0)에서 시작하여 (7,7) 까지 도착하는 모든 경로를 탐색한다.
 
 iii) 모든 경로를 탐색하면서 length[i,j]에 최소값을 저장해나간다.
 
 iv)  모든 경로의 탐색이 끝이 나면 length[n-1,n-1]에 흰 방으로 바꾸어야 할 최소 검은 방의 개수가 저장된다.
```

# 구현 코드
```cpp
#include<stdio.h>
#include<stdlib.h>
#include<limits.h>
#include<queue>
using namespace std;
int **length, **maze, n, i, j;
queue<int> qx;
queue<int> qy;
void check(int x, int y, int xp, int yp)
{
	if (x + xp >= 0 && x + xp < n && y + yp >= 0 && y + yp < n)
	{
		if (maze[x + xp][y + yp] == 0)
		{
			if (length[x + xp][y + yp] > length[x][y] + 1)
			{
				length[x + xp][y + yp] = length[x][y] + 1;
				qx.push(x + xp);
				qy.push(y + yp);
			}
		}
		else
		{
			if (length[x + xp][y + yp] > length[x][y])
			{
				length[x + xp][y + yp] = length[x][y];
				qx.push(x + xp);
				qy.push(y + yp);
			}
		}
	}
}
int main()
{
	int pos[2];
	char c;
	scanf("%d", &n);

	length = (int**)malloc(sizeof(int*) * n);
	maze = (int**)malloc(sizeof(int*) * n);

	for (i = 0; i < n; i++)
	{
		length[i] = (int*)malloc(sizeof(int) * n);
		maze[i] = (int*)malloc(sizeof(int) * n);
	}

	for (i = 0;i < n;i++)
	{
		while (getchar() != '\n');
		for (j = 0;j < n;j++)
		{
			scanf("%c", &c);
			maze[i][j] = (int)c - '0';
			length[i][j] = INT_MAX;
		}
	}

	j = 0;
	length[0][0] = 0;
	qx.push(0);
	qy.push(0);
	
	while (qx.empty()==false)
	{
		pos[0] = qx.front();
		pos[1] = qy.front();
		qx.pop();
		qy.pop();
		
		check(pos[0], pos[1], -1, 0);
		check(pos[0], pos[1], 1, 0);
		check(pos[0], pos[1], 0, -1);
		check(pos[0], pos[1], 0, 1);
	}
	printf("%d\n", length[n-1][n-1]);
	return 0;
}
```
