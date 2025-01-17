7장 분할 정복
================

## 01. 도입
#### &nbsp;분할 정복(Divide & Conquer)은 가장 유명한 알고리즘 디자인 패러다임으로, 각개 격파라는 말로 간단히 설명할 수 있다. 문제를 둘 이상의 부분 문제로 나눈 뒤 각 문제에 대한 답을 재귀 호출을 이용해 계산하고, 각 부분 문제의 답으로부터 전체 문제의 답을 계산해 낸다. 분할 정복이 일반적인 재귀 호출과 다른 점은 문제를 한 조각과 나머지 전체로 나누는 대신 거의 같은 크기의 부분 문제로 나누는 것이다. 분할 정복을 사용하는 알고리즘들은 대개 세 가지의 구성 요소를 갖고 있다.
#### 1. 문제를 더 작은 문제로 분할하는 과정(Divide)
#### 2. 각 문제에 대해 구한 답을 원래 문제에 대한 답으로 병합하는 과정(Merge)
#### 3. 더이상 답을 분할하지 않고 곧장 풀 수 있는 매우 작은 문제(Base case)
#### 분할 정복은 많은 경우 같은 작업을 더 빠르게 처리해 준다.

* ### 예제: 수열의 빠른 합과 행렬의 빠른 제곱
#### &nbsp;1부터 n까지의 합을 재귀 호출을 이용해 계산하는 recursiveSum() 함수를 작성했었다. 여기서 분할 정복을 이용해 똑같은 일을 하는 fastSum() 함수를 만들어 본다. 편의상 n은 짝수라고 가정한다. 1부터 n까지 합을 구할 때 (1 + 2 + ... + (n/2)) + ((n/2 + 1) + ... + n)으로 나타낼 수 있다. 첫 번째 부분은 fastSum(n / 2)로 나타낼 수 있지만, 두 번째 부분 문제는 그렇지 않다. 두 번째 부분을 바꿔보면 (n/2 + 1) + (n/2 + 2) + ... + (n/2 + n/2) = (n/2) * (n/2) + (1 + 2 + 3 + ... + (n/2)) = (n/2)^2 + fastSum(n/2)로 나타낼 수 있다. 따라서 fastSum(n) = 2 * fastSum(n/2) + (n/2)^2 (n이 짝수 일때) 로 나타낼 수 있다.
```c++
// 필수 조건: n은 자연수
// 1 + 2 + 3 + ... + n을 반환한다.
int fastSum(int n) {
  // 기저 사례
  if(n == 1) return 1;
  if(n % 2 == 1) return fastSum(n - 1) + n;
  return 2 * fastSum(n/2) + (n/2) * (n/2);
}
```

* ### 시간 복잡도 분석
#### &nbsp;fastSum()과 recursiveSum()은 반복문이 없기 때문에 순전히 함수가 호출되는 횟수에 비례한다. recursiveSum()은 n번 함수의 호출이 필요하다. 하지만 fastSum()은 호출될 때마다 최소한 두 번에 한 번 꼴로 n이 절반으로 줄어든다. 그러니 fastSum()의 호출 횟수가 훨씬 적으리란 것을 쉽게 예상할 수 있다. fastSum()의 알고리즘 실행 시간은 O(lgn)이 된다.

* ### 행렬의 거듭제곱
#### &nbsp;n * n 크기의 행렬 A가 주어질 때 거듭제곱(power)을 계산하는 알고리즘 자체는 어려울 것이 없지만, m이 매우 클 때 A^m을 구하는 것은 꽤나 시간이 오래 걸리는 작업이다. 행렬의 곱셈에는 O(n^3)번의 연산이 필요하다. 여기서 분할 정복을 이용하면 눔깜짝할 새에 이 값을 구할 수 있다. A^m을 구하는데 필요한 m개의 조각을 절반으로 나눈다.
```c++
// 정방행렬을 표현하는 SquareMatrix 클래스가 있다고 가정한다.
class SquareMatrix;
// n*n 크기의 항등 행렬(identity matrix)을 반환하는 함수
SquareMatrix identity(int n);
// A^m을 반환한다.
SquareMatrix pow(const SquareMatrix& A, int m) {
  // 기저 사례: A^0 = I
  if(m == 0) return identity(A.size());
  if(m % 2 > 0) return pow(A, m - 2) * A;
  SquareMatrix half = pow(A, m / 2);
  // A^m = (A^(m/2) * (A^(m/2))
  return half * half;
}
```

* ### 나누어 떨어지지 않을 때의 분할과 시간 복잡도
#### &nbsp;m이 홀수일 때, A^m = A*A^(m-1)로 나누지 않고, 좀더 절반에 가깝게 나누는 게 좋지 않을까라는 생각을 할 수도 있습니다. 그러나 이 문제에서 이 방식은 오히려 알고리즘을 더 느리게 만든다. 좀더 절반에 가깝게 나눠서 계산하는 경우는 결국 m-1번 곱셈을 하는 것과 다를바가 없다. 반면 원래 하던 방식으로 하면 시간 복잡도는 O(lgn)이 된다.

* ### 예제: 접합 정렬과 퀵 정렬
#### &nbsp;정렬 알고리즘 중 가장 널리 쓰이는 것이 병합 정렬(Merge sort)와 퀵 정렬(Quick sort)인데, 이 두 알고리즘은 모두 분할 정복 패러다임을 기반으로 해서 만들어진 것이다. 같은 문제를 해결하는 알고리즘이라도 어떤 식으로 분할하느냐에 따라 다른 알고리즘이 될 수 있으며, 이들 모두는 분할 정복 패러다임을 사용한 알고리즘이라고 할 수 있다.

* ### 시간 복잡도 분석
#### &nbsp;병합 정렬과 퀵 정렬은 본질적으로 비슷한 형태의 알고리즘이기 때문에, 시간 복잡도를 비슷한 방법으로 분석할 수 있다. 병합 정렬의 수행 시간은 병합 과정에 의해 지배된다. 그런데 아래 단계로 내려갈수록 부분 문제의 수는 두 배로 늘고 각 부분 문제의 크기는 바능로 줄어들기 때문에, 한 단계 내에서 모든 병합에 필요한 총 시간은 O(n)으로 항상 일정하다. 단계의 수에 n을 곱하면 병합 정렬에 필요한 전체 시간을 얻을 수 있다. 문제의 크기는 항상 거의 절반으로 나누어 지기 때문에, 필요한 단게의 수는 O(lgn)이 된다. 따라서 병합 정렬의 시간 복잡도는 O(nlgn)이다. 퀵 정렬의 경우 대부분의 시간을 차지하는 것은 주어진 문제를 두 개의 부분 문제로 나누는 파티션 과정이다. 파티션에는 주어진 수열의 길이에 비례하는 시간이 걸린다. 사실 병합 정렬에서의 병합 과정과 다를 것이 없다. 하지만 최악의 경우에는 O(n^2)이 된다. 그래서 대부분의 퀵 정렬 구현은 가능한 한 절반에 가까운 분할을 얻기 위해 좋은 기준을 뽑는 다향한 방법들을 사용한다.

* ### 예제: 카라츠바의 빠른 곱셈 알고리즘
#### &nbsp;수백, 수만 자리는 되는 큰 숫자들을 다룰 때 주로 사용된다.
```c++
// num[]의 자릿수 올림을 처리한다.
void normalize(vector<int>& num) {
  num.push_back(0);
  // 자리수 올림을 처리한다.
  for(int i = 0; i < num.size(); ++i) {
    if(num[i] < 0) {
      int borrow = (abs(num[i]) + 9) / 10;
      num[i + 1] -= borrow;
      num[i] += borrow * 10;
    } else {
      num[i + 1] += num[i] / 10;
      num[i] %= 10;
    }
  }
  while(num.size() > 1 && num.back() == 0) num.pop_back();
}
// 두 긴 자연수의 곱을 반환한다.
// 각 배열에는 각 수의 자릿수가 1의 자리에서부터 시작해 저장되어 있다.
// 예: multiply({3, 2, 1}, {6, 5, 4}) = 123 * 456 = 56088 = {8, 8, 0, 6, 5}
vector<int> multiply(const vector<int>& a, const vector<int>& b) {
  vector<int> c(a.size() + b.size() + 1, 0);
  for(int i = 0; i < a.size(); ++i)
    for(int j = 0; j < b.size(); ++j)
      c[i + j] += a[i] * b[j];
  normalize(c);
  return c;
}
```
#### 두 정수의 길이가 모두 n이라고 할 때 O(n^2)이다. 이것보다 빠른 알고리즘이 카라츠바 알고리즘이다. 만약 a, b가 256자리 수라면 a = a1 * 10^128 + a0와 b = b1 * 10^128 + b0라고 표현할 수 있다. 따라서 a * b = (a1 * 10^128 + a0) * (b1 * 10^128 + b0) = a1 * b1 * 10^256 + (a1 * b0 + a0 * b1) * 10^128 + a0 * b0으로 표현할 수 있다. 큰 정수 두개를 한 번 곱하는 대신, 절반 크기로 나눈 작은 조각을 네 번 곱한다. 이때 카라츠바가 발견한 것은 a * b를 이전과 같이 네 번 대신 세 번의 곱셈으로만 이 값을 계산할 수 있다는 것이다. 다음은 카라츠바 알고리즘의 구현이다.
```c++
// a += b * (10^k);를 구현한다.
void addTo(vector<int>& a, const vector<int>& b, int k);
// a -= b;를 구현한다. a >= b를 가정한다.
void subFrom(vector<int>& a, const vector<int>& b);
// 두 긴 정수의 곱을 반환한다.
vector<int> karatsuba(const vector<int>& a, const vector<int>& b) {
  int an = a.szie(), bn = b.size();
  // a가 b보다 짧을 경우 둘을 바꾼다.
  if(an < bn) return karatsuba(b, a);
  // 기저 사례: a나 b가 비어 있는 경우
  if(an == 0 || bn == 0) return vector<int>();
  // 기저 사례: a가 비교적 짧은 경우 O(n^2)곱셈으로 변경한다.
  if(an <= 50) return multiply(a, b);
  int half = an / 2;
  // a와 b를 밑에서 half 자리와 나머지로 분리한다.
  vector<int> a0(a.begin(), a.begin() + half);
  vector<int> a1(a.begin() + half, a.end());
  vector<int> b0(b.begin(), b.begin() + min<int>(b.size(), half));
  vactor<int> b1(b.begin() + min<int>(b.size(), half), b.end());
  // z2 = a1 * b1
  vector<int> z2 = karatsuba(a1, b1);
  // z0 = a0 * b0
  vector<int> z0 = karatsuba(a0, b0);
  // a0 = a0 + a1; b0 = b0 + b1
  addTo(a0, a1, 0);
  addTo(b0, b1, 0);
  // z1 = (a0 * b0) - z0 - z2;
  vector<int> z1 = karatsuba(a0, b0);
  subFrom(z1, z0);
  subFrom(z1, z2);
  // ret = z0 + z1 * 10^half + z2* 10^(half*2)
  vector<int> ret;
  addTo(ret, z0, 0);
  addTo(ret, z1, half);
  addTo(ret, z2, half + half);
  return ret;
}
```

* ### 시간 복잡도 분석
#### &nbsp;최종 시간 복잡도는 O(n^lg3)이 된다.

## 02. 문제: 쿼드 트리 뒤집기(문제 ID: QUADTREE, 난이도: 하)
#### &nbsp;대량의 좌표 데이터를 메모리 안에 압축해 저장하기 위해 사용하는 여러 기법 중 쿼드 트리(quad tree)란 것이 있습니다. 주어진 공간을 항상 4개로 분할해 재귀적으로 표현하기 때문에 쿼드 트리라는 이름이 붙었는데, 이의 유명한 사용처 중 하나는 검은 색과 흰 색밖에 없는 흑백 그림을 압축해 표현하는 것입니다. 쿼드 트리는 2^N × 2^N 크기의 흑백 그림을 다음과 같은 과정을 거쳐 문자열로 압축합니다.
#### 01. 이 그림의 모든 픽셀이 검은 색일 경우 이 그림의 쿼드 트리 압축 결과는 그림의 크기에 관계없이 b가 됩니다.
#### 02. 이 그림의 모든 픽셀이 흰 색일 경우 이 그림의 쿼드 트리 압축 결과는 그림의 크기에 관계없이 w가 됩니다.
#### 03. 모든 픽셀이 같은 색이 아니라면, 쿼드 트리는 이 그림을 가로 세로로 각각 2등분해 4개의 조각으로 쪼갠 뒤 각각을 쿼드 트리 압축합니다. 이때 전체 그림의 압축 결과는 x(왼쪽 위 부분의 압축 결과)(오른쪽 위 부분의 압축 결과)(왼쪽 아래 부분의 압축 결과)(오른쪽 아래 부분의 압축 결과)가 됩니다. 예를 들어 그림 (a)의 왼쪽 위 4분면은 xwwwb로 압축됩니다.
#### 그림 (a)와 그림 (b)는 16×16 크기의 예제 그림을 쿼드 트리가 어떻게 분할해 압축하는지를 보여줍니다. 이때 전체 그림의 압축 결과는 xxwww bxwxw bbbww xxxww bbbww wwbb가 됩니다. 쿼드 트리로 압축된 흑백 그림이 주어졌을 때, 이 그림을 상하로 뒤집은 그림 을 쿼드 트리 압축해서 출력하는 프로그램을 작성하세요.

* ### 시간 및 메모리 제한
#### &nbsp;프로그램은 1초 안에 실행되어야 하며, 64MB 이하의 메모리를 사용해야만 한다.

* ### 입력
#### &nbsp;첫 줄에 테스트 케이스의 개수 C (C<=50)가 주어집니다. 그 후 C줄에 하나씩 쿼드 트리로 압축한 그림이 주어집니다. 모든 문자열의 길이는 1,000 이하이며, 원본 그림의 크기는 2^20 × 2^20 을 넘지 않습니다.

* ### 출력
#### &nbsp;각 테스트 케이스당 한 줄에 주어진 그림을 상하로 뒤집은 결과를 쿼드 트리 압축해서 출력합니다.

* ### 개인적 풀이
#### &nbsp;문자열 x가 나오면 재귀적인 함수 호출을 통해서 더이상 x가 나오지 않고 4개의 문자열이 나오면 상하를 바꿔주면 된다.

## 03. 풀이: 쿼드 트리 뒤집기
#### &nbsp;단순한 알고리즘에서 시작해서 최적화해 나간다.

* ### 쿼드 트리 압축 풀기
#### &nbsp;문자열 s의 압축을 해제해서 N * N 크기의 배열에 저장하는 함수 decompress()를 구현한다고 한다. 만약 첫 글자가 x라면 decompress()는 s의 나머지 부분을 넷으로 쪼개 재귀 호출한다. 다음과 같은 형의 decompress()를 작성하면 된다.
```c++
char decompressed[MAX_SIZE][MAX_SIZE];
// s를 압축 해제해서 decompressed[y..y + size - 1][x..x + size - 1]구간에 쓴다.
void decompress(const string& s, int y, int x, int size);
```

* ### 압축 문자열 분할하기
#### &nbsp;decompress() 함수에서 '필요한 만큼 가져다 쓰도록' 하게 만든다. s를 통째로 전달하는 것이 아니라, s의 한 글자를 가리키는 포인터를 전달한다.
```c++
char decompressed[MAX_SIZE][MAX_SIZE];
void decompress(string::iterator& it, int y, int x, int size) {
  // 한 글자를 검사할 때마다 반복자를 한 칸 앞으로 옮긴다.
  char head = *(it++);
  // 기저 사례: 첫 글자가 b 또는 w인 경우
  if(head == 'b' || head == 'w') {
    for(int dy = 0; dy < size; ++dy)
      for(int dx = 0; dx < size; ++dx)
        decompressed[y + dy][x + dx] = head;
  } else {
    // 네 부분을 각각 순서대로 압축 해제한다.
    int half = size / 2;
    decompress(it, y, x, half);
    decompress(it, y, x + half, half);
    decompress(it, y + half, x, half);
    decompress(it, y + half, x + half, half);
  }
}
```

* ### 압축 다 풀지 않고 뒤집기
#### &nbsp;곧이 곧대로 압축을 풀기에는 이 문제에서 다루는 그림들은 너무 크다. 2^20 * 2^20크기의 거대한 그림은 1테라바이트나 된다. decompress()의 기저 사례를 생각해 본다. 천체가 한가지 색인 경우는 뒤집어도 똑같다. 아닌경우는 네 부분을 각각 상하로 뒤집은 결과를 얻은 뒤, 이들을 병합해 답을 얻어야 한다. 각각의 네 부분을 상하로 뒤집고, 위 두 조각과 아래 두 조각을 바꾸면 전체가 뒤집힌 그림이 된다.
```c++
string reverse(string::iterator& it) {
  char head = *it;
  ++it;
  if(head == 'b' || head == 'w')
    return string(1, head);
  string upperLeft = reverse(it);
  string upperRight = reverse(it);
  string lowerLeft = reverse(it);
  string lowerRight = reverse(it);
  // 각각 위와 아래 조각들의 위치를 바꾼다.
  return string("x") + lowerLeft + lowerRight + upperLeft + upperRight;
}
```

* ### 내가 짠 코드와 비교해 보기
#### &nbsp;이번에는 코드를 짜지 못했다. 답안을 보니 생각보다 간단한 코드로 구현이 가능하다. iterator를 사용하는 부분은 나중에도 유용하게 사용될 것 같다.

* ### 시간 복잡도 분석
#### &nbsp;reverse()함수는 한 번 호출될 때마다 주어진 문자열의 한 글자씩을 사용한다. 따라서 함수가 호출되는 횟수는 문자열의 길이에 비례하므로 O(n)이 된다.

## 07. 문제: 울타리 잘라내기(문제 ID: FENCE, 난이도: 중)
#### &nbsp;너비가 같은 N개의 나무 판자를 붙여 세운 울타리가 있습니다. 시간이 지남에 따라 판자들이 부러지거나 망가져 높이가 다 달라진 관계로 울타리를 통째로 교체하기로 했습니다. 이 때 버리는 울타리의 일부를 직사각형으로 잘라내 재활용하고 싶습니다. 그림 (b)는 (a)의 울타리에서 잘라낼 수 있는 많은 직사각형 중 가장 넓은 직사각형을 보여줍니다. 울타리를 구성하는 각 판자의 높이가 주어질 때, 잘라낼 수 있는 직사각형의 최대 크기를 계산하는 프로그램을 작성하세요. 단 (c)처럼 직사각형을 비스듬히 잘라낼 수는 없습니다. 판자의 너비는 모두 1이라고 가정합니다.

* ### 시간 및 메모리 제한
#### &nbsp;프로그램은 1초 안에 실행되어야 하며, 64MB 이하의 메모리를 사용해야만 한다.

* ### 입력
#### &nbsp;첫 줄에 테스트 케이스의 개수 C (C≤50)가 주어집니다. 각 테스트 케이스의 첫 줄에는 판자의 수 N (1≤N≤20000)이 주어집니다. 그 다음 줄에는 N개의 정수로 왼쪽부터 각 판자의 높이가 순서대로 주어집니다. 높이는 모두 10,000 이하의 음이 아닌 정수입니다.

* ### 출력
#### &nbsp;각 테스트 케이스당 정수 하나를 한 줄에 출력합니다. 이 정수는 주어진 울타리에서 잘라낼 수 있는 최대 직사각형의 크기를 나타내야 합니다.

* ### 개인적 풀이
#### &nbsp;반복문을 돌면서 직접 모든 크기를 계산해보는 풀이방법만 떠오른다. 분할 정복을 이용해서 문제를 푸는 방법이 떠오르지 않는다.

## 05. 풀이: 울타리 잘라내기

* ### 무식하게 풀 수 있을까?
#### &nbsp;각 판자 높이의 배열 h[]가 주어졌을 때 l번 판자부터 r번 판자까지 잘라내서 사각형을 만든다고 한다. 이때 사각형의 넓이는 다음과 같다.
> &nbsp;(r - l + 1) * min(h[i])
#### 이떄 문제를 풀 수 있는 가장 간단한 방법은 2중 for문으로 가능한 모든 l과 r을 순회하며 위 값을 계산하는 것이다. 다음 코드가 이 방법의 구현을 보여준다.
```c++
// 판자의 높이를 담은 배열 h[]가 주어질 때 사각형의 최대 너비를 반환한다.
int bruteForce(const vector<int>& h) {
  int ret = 0;
  int N = h.size();
  // 가능한 모든 left, right 조합을 순회한다.
  for(int left = 0; left < N; ++left) {
    int minHeight = h[left];
    for(int right = left; right < N; ++right) {
      minHeight = min(minHeight, h[right]);
      ret = max(ret, (rigth - left + 1) * minHeight);
    }
  }
  return ret;
}
```

* ### 분할 정복 알고리즘의 설계
#### &nbsp;분할 정복 알고리즘을 설계하기 위해서는 문제를 어떻게 분할할지를 가장 먼저 결정해야 한다. n개의 판자를 절반으로 나눠 두 개의 부분 문제를 만든다. 그러면 우리가 찾는 최대 직사각형은 다음 세 가지 중 하나에 속한다.
#### 01. 가장 큰 직사각형을 왼쪽 부분 문제에서만 잘라낼 수 있다.
#### 02. 가장 큰 직사각형을 오른쪽 부분 문제에서만 잘라낼 수 있다.
#### 03. 가장 큰 직사각형은 왼쪽 부분 문제와 오른쪽 부분 문제에 걸쳐 있다.

* ### 양쪽 부분 문제에 걸친 경우의 답
#### &nbsp;이 직사각형은 반드시 부분 문제 경계에 있는 두 판자를 포함한다. 따라서 확장을 할 때 더 높은 쪽의 판자를 포함하게끔 확장해야 한다.
```c++
// 각 판자의 높이를 저장하는 배열
vector<int> h;
// h[left..right]구간에서 찾아낼 수 있는 가장 큰 사각형의 넓이를 반환한다.
int solve(int left, int right) {
  // 기저 사례: 판자가 하나밖에 없는 경우
  if(left == right) return h[left];
  // [left, mid], [mid + 1, right]의 두  구간으로 문제를 분할한다.
  int mid = (left + right) / 2;
  // 분할한 문제를 각개격파
  int ret = max(solve(left, mid), solve(mid + 1, right));
  // 부분 문제 3: 두 부분에 모두 걸치는 사각형 중 가장 큰 것을 찾는다.
  int lo = mid, hi = mid + 1;
  int height = min(h[lo], h[hi]);
  // [mid, mid + 1]만 포함하는 너비 2인 사각형을 고려한다.
  ret = max(ret, height * 2);
  // 사각형이 입력 전체를 덮을 때까지 확장해 나간다.
  while(left < lo || hi < right) {
    // 항상 높이가 더 높은 쪽으로 확장한다.
    if(hi < right && (lo == left || h[lo - 1] < h[hi + 1])) {
      ++hi;
      height = min(height, h[hi]);
    } else {
      --lo;
      height = min(height, h[lo]);
    }
    // 확장한 후 사각형의 넓이
    ret = max(ret, height * (hi - lo + 1));
  }
  return ret;
}
```

* ### 정당성 증명
#### &nbsp;어떤 사각형 R이 우리가 이 과정을 통해 찾은 최대 직사각형보다 더 크다고 가정해 본다. 우리가 고려한 사각형 중 R과 너비가 같은 사각형이 반드시 하나 있다. 이 사각형을 R'라고 한다. 너비가 같기 때문에 R이 더 넓기 위해서는 높이가 반드시 R'보다 높아야 한다. 또한 R과 R'은 경계에 있는 두 개의 판자를 포함하므로 항상 겹치는 부분이 있다. 이때 R'의 높이를 결정하는 가장 낮은 판자 A를 생각해 본다. R에 포함된 모든 판자들은 당연히 A보다 높아야 한다. 하지만 알고리즘에 의하면 A를 선택하는 경우는 없다. 그러므로 R이 R'보다 높다는 우리의 가정이 모순이다.

* ### 시간 복잡도 분석
#### &nbsp;주어진 n크기의 배열을 n/2크기의 배열 두개로 나눈 뒤 이들을 각각 해결한다. 두 부분에 걸쳐 있는 사각형을 찾는 작업이 전체 시간 복잡도를 결정한다. 따라서 이 과정의 시간 복잡도는 O(n)이다. 문제를 항상 절반으로 나누어서 재귀 호출하고, O(n) 시간의 후처리를 하기 때문에 시간 복잡도는 O(nlgn)이다.

* ### 선형 시간 알고리즘
#### &nbsp;다른 방법으로도 문제를 풀 수 있다.

## 07. 문제: 팬미팅(문제 ID: FANMEETING, 난이도: 상)
#### &nbsp;가장 멤버가 많은 아이돌 그룹으로 기네스 북에 올라 있는 혼성 팝 그룹 하이퍼시니어가 데뷔 10주년 기념 팬 미팅을 개최했습니다. 팬 미팅의 한 순서로, 멤버들과 참가한 팬들이 포옹을 하는 행사를 갖기로 했습니다. 하이퍼시니어의 멤버들은 우선 무대에 일렬로 섭니다. 팬 미팅에 참가한 M명의 팬들은 줄을 서서 맨 오른쪽 멤버에서부터 시작해 한 명씩 왼쪽으로 움직이며 멤버들과 하나씩 포옹을 합니다. 모든 팬들은 동시에 한 명씩 움직입니다. 아래 그림은 행사 과정의 일부를 보여줍니다. a~d는 네 명의 하이퍼시니어 멤버들이고, 0~5는 여섯 명의 팬들입니다. 하지만 하이퍼시니어의 남성 멤버들이 남성 팬과 포옹하기가 민망하다고 여겨서, 남성 팬과는 포옹 대신 악수를 하기로 했습니다. 줄을 선 멤버들과 팬들의 성별이 각각 주어질 때 팬 미팅이 진행되는 과정에서 하이퍼시니어의 모든 멤버가 동시에 포옹을 하는 일이 몇 번이나 있는지 계산하는 프로그램을 작성하세요.

* ### 시간 및 메모리 제한
#### &nbsp;프로그램은 10초 안에 실행되어야 하며, 64MB 이하의 메모리를 사용해야만 한다.

* ### 입력
#### &nbsp;첫 줄에 테스트 케이스의 개수 C (C≤20)가 주어집니다. 각 테스트 케이스는 멤버들의 성별과 팬들의 성별을 각각 나타내는 두 줄의 문자열로 구성되어 있습니다. 각 문자열은 왼쪽부터 오른쪽 순서대로 각 사람들의 성별을 나타냅니다. M은 해당하는 사람이 남자, F는 해당하는 사람이 여자임을 나타냅니다. 멤버의 수와 팬의 수는 모두 1 이상 200,000 이하의 정수이며, 멤버의 수는 항상 팬의 수 이하입니다.

* ### 출력
#### &nbsp;각 테스트 케이스마다 한 줄에 모든 멤버들이 포옹을 하는 일이 몇 번이나 있는지 출력합니다.

* ### 개인적 풀이
#### &nbsp;여성 맴버들은 성별 관계없이 모두와 포옹을 하고, 남성 멤버들은 남성 팬과는 악수, 여성 팬과는 포옹을 한다. 문제는 모든 멤버가 동시에 포옹을 하는 경우의 횟수를 알아내야 한다. 모든 멤버가 포옹을 하는 경우는 남성 멤버에 의해서 결정된다. 이때 문제를 다르게 바꿔보면 주어진 문자열(멤버 성별)을 포함하는지 찾으면 된다. 다만 문자가 M인 경우에는 F로 강제하고 F인 경우에는 어떤 문자열이 와도 상관 없다.

## 07. 풀이: 팬미팅
#### &nbsp;가장 간단한 방법은 단순히 팬미팅 과정을 하나하나 시뮬레이션하는 것이다. 팬의 수를 N, 멤버의 수를 M이라고 할 때 이 방법은 O(NM - M^2)에 해당하는 시간(N-M번 확인을 하는데 확인할 때 M만큼 걸리기 때문에)을 필요로 하는데 N과 M은 각각 20만에 달하는 수라는 것을 생각해 보면 시간 안에 풀기 어렵다.

* ### 곱셈으로의 변형
#### &nbsp;이 문제를 빠르게 푸는 방법은 두 큰 수의 곱셈으로 이 문제를 바꾸는 것이다. 일렬로 선 사람들의 성별을 긴 정수로 표시한다. 각 자릿수는 한 사람의 성별을 나타내며, 남성은 1, 여성은 0으로 표시한다. 그러면 남성과 남성이 만났을 때 두 자릿수의 곱은 1이 된다. 이 외의 경우에는 두 자릿수의 곱이 항상 0이 된다.

* ### 카라츠바 알고리즘을 이용한 구현
```c++
int hugs(const string& members, const string& fans) {
  int N = members.size(), M = fans.size();
  vector<int> A(N), B(M);
  for(int i = 0; i < N; ++i) A[i] = (members[i] == 'M');
  for(int i = 0; i < M; ++i) B[M - i - 1] = (fans[i] == 'M');
  // karatsuba 알고리즘
  vector<int> C = karatsuba(A, B);
  int allHugs = 0;
  for(int i = N - 1; i < M; ++i)
    if(C[i] == 0)
      ++ allHugs;
  return allHugs;
}
```