---
title: 백준 2210 숫자판 점프
categories:
- Problem
---
### 문제 요약

1. 5*5 크기의 숫자판이 있다.
2. 인접해있는 네 방향으로 다섯번 이동하면서 숫자를 이어 붙여 6자리 수를 만든다.
3. 만들 수 있는 수들의 개수를 구하여라.

### 문제 풀이

1. 시간 제한이 넉넉하기 때문에 그냥 bfs로 모든 경우의 수를 구했다.
2. 모든 경우의 수를 string 벡터에 넣어서 나중에 중복을 제거했다.

### 전체 코드

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <queue>
#include <algorithm>
using namespace std;
int map[5][5];
int dir[4][2] = { {-1,0},{1,0},{0,-1},{0,1} };
vector<string> bfs(int row, int col) {
	vector<string> vec;
	queue<pair<pair<int,int>,pair<string,int>>> q;
	//result += to_string(map[row][col]);
	q.push(make_pair(make_pair(row, col),make_pair(to_string(map[row][col]),0)));
	while (!q.empty()) {
		//cout << cnt << endl;
		int temp_row = q.front().first.first;
		int temp_col = q.front().first.second;
		string temp_str = q.front().second.first;
		int cnt = q.front().second.second;
		q.pop();
		if (cnt==5) {
			vec.push_back(temp_str);
			//cout << temp_str << endl;
			continue;
		}
		for (int i = 0; i < 4; i++) {
			int nrow = temp_row + dir[i][0];
			int ncol = temp_col + dir[i][1];
			if (nrow < 0 || nrow >= 5 || ncol < 0 || ncol >= 5) {
				continue;
			}
			//visit 필요없음 거쳤던 칸 다시 거쳐도 되므로.
			string nstr = temp_str + to_string(map[nrow][ncol]);
			q.push(make_pair(make_pair(nrow, ncol), make_pair(nstr, cnt + 1)));
		}
	}
	return vec;

}
int main() {
	vector<string> result;
	for (int i = 0; i < 5; i++) {
		for (int j = 0; j < 5; j++) {
			cin >> map[i][j];
		}
	}
	for (int i = 0; i < 5; i++) {
		for (int j = 0; j < 5; j++) {
			vector<string> temp_vec = bfs(i, j);
			for (int k = 0; k < temp_vec.size(); k++) {
				result.push_back(temp_vec[k]);
			}
		}
	}
	int total = 1;
	//cout << result.size() << endl;
	sort(result.begin(), result.end());
	for (int i = 1; i < result.size(); i++) {
		if (result[i] != result[i - 1]) {
			total++;
		}
	}
	cout << total << endl;
	

}
```