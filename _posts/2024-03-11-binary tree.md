---
title: 이진 트리
date: 2024-03-11 18:56:00 +09:00
categories: [Data Structure]
use_math: true
---

## **이진 트리**
![](/assets/img/data structure/binary tree/binary tree.png)

**이진 트리는 모든 노드가 2개 이하의 자식 노드를 가지고 있는 트리**를 말한다. 즉, 모든 노드의 차수가 2이하이다. 

---

## 이진 트리의 종류 
### **Full Binary Tree** 
![](/assets/img/data structure/binary tree/Full Binary Tree.png)

>
모든 노드의 차수가 0 혹은 2인 트리
>

---

### **Degenerate Binary Tree**
![](/assets/img/data structure/binary tree/Degenerate Binary Tree.png)

>
모든 내부 노드의 차수가 1인 트리
>

---

### **Skewed Binary Tree (편향 이진 트리)**
![](/assets/img/data structure/binary tree/Left Skewed Binary Tree.png)

Left Skewed Binary Tree

![](/assets/img/data structure/binary tree/Right Skewed Binary Tree.png)

Right Skewed Binary Tree

> 
모든 내부 노드의 차수가 1이고, 노드가 한쪽 방향으로 편향된 트리.
왼쪽으로 치우친 Left Skewed Binary Tree와 오른쪽으로 치우친 Right Skewed Binary Tree가 있다.
>

---

### **Complete Binary Tree (완전 이진 트리)**
![](/assets/img/data structure/binary tree/Complete Binary Tree.png)

>
높이가 K인 트리의 레벨 1부터 레벨 K-1까지는 노드가 모두 채워져 있고, 레벨 K의 노드는 왼쪽부터 빠짐없이 채워져 있는 트리
>

---

### **Perfect Binary Tree (포화 이진 트리)**
![](/assets/img/data structure/binary tree/Perfect Binary Tree.png)

>
모든 레벨의 노드가 채워져 있는 트리
>

높이가 $k$인 Perfect Binary Tree의 노드 개수는
$\displaystyle\sum_{i=0}^{k - 1}2^i=2^k-1$개이다.

---

## **이진 트리의 높이**
N개의 노드로 구성된 이진 트리가 있을 때, 트리의 높이를 가장 높거나 낮게 만들려면 트리를 어떻게 구성해야 할까?
높이를 가장 높게 만들려면 레벨당 최대한 적은 노드가 들어가야 하므로 편향 이진 트리를 만들면 된다. 반대로 높이를 가장 낮게 만들려면 레벨 당 최대한 많은 노드가 들어가야 하므로 완전 이진 트리를 만들면 된다. 이때 높이는 어떻게 구할 수 있을까?

편향 이진 트리의 높이를 구하는 방법은 간단하다. $N$개의 노드를 일렬로 배열한 구조이므로 높이도 $N$이다.

다음으로 완전 이진 트리의 높이를 구해보자. 높이가 $h$인 트리가 가질 수 있는 노드의 최대 개수는 $2^h-1$이고, 포화 이진 트리의 노드의 개수와 같다. 
우리는 $N$개의 노드로 완전 이진 트리를 만들 것이므로 $N\le2^h-1$이 성립한다. 양변에 로그를 취한 후, 수식을 정리하면 $h\ge log_2(N+1)$이 되고, h는 최솟값이면서 정수여야 하므로 $h = \lceil log_2(N+1)\rceil$이 성립한다.

>
노드가 N개인 이진 트리의 최대 높이는 $N$, 최소 높이는 $\lceil log_2(N+1)\rceil$이다.
>

---

## **이진 트리 구현**
### **배열을 이용한 이진 트리 표현**
![](/assets/img/data structure/binary tree/array binary tree 1.png)


배열을 이용해서 간단하게 트리를 구현할 수 있다. 위 트리에서 알파벳은 트리의 데이터, 초록색 숫자는 배열에 저장되는 index이다.

아래 순서대로 트리를 구현하면 된다.
>
1. 노드의 개수가 $N$개일 때, $2^N$개의 배열을 만든다. (Right Skewed Binary Tree일 때 최대 개수)
2. 각 노드의 번호에 대응하는 index에 트리의 데이터를 대입한다.
>

노드 번호를 정하는 규칙은 다음과 같다.
>
1. 루트 노드의 번호는 1번이다.
2. 부모 노드의 번호가 $n$번이면 왼쪽 자식 노드의 번호는 $n\ast2$ , 오른쪽 자식 노드의 번호는 $n*2+1$번이다.
3. 자식 노드의 번호가 $n$번이면 부모 노드의 번호는 $n/2$번이다.

하지만 배열로 트리를 구현할 때 치명적인 단점이 있다. 바로 특정한 경우에 메모리 효율이 매우 떨어진다는 것이다. 아래 트리를 보자.
![](/assets/img/data structure/binary tree/array binary tree 2.png)


Right Skewed Binary Tree를 배열로 구현한 것이다. 메모리는 $2^N$개가 할당되지만, 실제로는 $N$개의 메모리만 사용한다. $2^N - N$개의 메모리가 낭비되는 것이다. 적어보일 수 있지만 계산해보면 절대 적지 않다. 트리의 데이터 타입이 int일 때 $N = 30$이면, 약 4기가의 메모리가 낭비된다.
따라서, 완전 이진 트리 혹은 포화 이진 트리를 구현할 때는 배열로 구현하는 게 효과적이지만, 트리의 종류를 알 수 없을 때는 최악의 경우를 상정하고 링크를 이용해서 구현하는 것이 효과적일 수 있다.

---

### **배열을 이용한 이진 트리 구현**

```java
class Tree<E> {
	private int nodeCnt;
	private E[] tree;
	
	public void initTree(int nodeCnt) {
		tree = (E[]) new Object[1 << nodeCnt];
	}
	
	public void setTree(int idx, E data) {
		tree[idx] = data;
	}
}
```

---

### **링크를 이용한 이진 트리 표현**

![](/assets/img/data structure/binary tree/link binary tree 1.png)

각 노드가 왼쪽 자식의 주소, 오른쪽 자식의 주소, 데이터를 가지고 있다. 특정 방향의 자식 노드가 없는 경우에는 null을 저장한다. 루트 노드의 주소만 알면 트리의 모든 노드에 접근할 수 있다. 

![](/assets/img/data structure/binary tree/link binary tree 2.png)

배열로 구현했을 때 메모리 낭비가 생겼던 트리이다. 링크를 이용해서 구현하면 메모리 낭비 없이 트리를 구현할 수 있다. 하지만 링크를 이용한 트리도 단점은 있다.
노드 번호로 데이터를 탐색할 때 배열을 이용해서 구현한 트리는 O(1)에 찾을 수 있지만, 링크를 이용해서 구현한 트리는 루트 노드부터 목표 노드까지 순회해야 하기 때문에 편향 이진 트리의 경우 O(N), 완전 이진 트리의 경우 O(logN)의 시간복잡도를 가진다.

---

### **링크를 이용한 이진 트리 구현**

```java
class Tree<E> {
	private Node<E> root = new Node<>(null);
	
	private class Node<E> {
		E data;
		Node<E> left, right;
		
		Node(E data) {
			this.data = data;
			left = right = null;
		}
	}
}
```

---

## **이진 트리 순회**
트리를 순회한다는 것은 트리의 모든 노드를 한 번씩 방문한다는 것이다. 큐, 스택과 같은 선형 자료구조의 데이터를 순회하는 방법은 하나밖에 없었다. 하지만 트리는 여러 가지 방법으로 순회할 수 있다. 

이진 트리를 순회하는 방법은 서브트리의 루트노드를 몇 번째로 방문하는지에 따라 전위 순회, 중위 순회, 후위 순회로 나뉜다. 루트 노드를 첫 번째로 방문하면 전위 순회, 두 번째로 방문하면 중위 순회, 세 번째로 방문하면 후위 순회이다.

---

### **전위 순회 (Preorder)**
![](/assets/img/data structure/binary tree/preorder.png)

초록색 번호는 노드간 이동 순서, 빨간색 번호는 실제 방문 순서이다. 
전위 순회 방식은 아래와 같다.

>
1. A를 루트 노드로 하는 서브트리의 루트 노드에 방문한 후 B로 이동한다. 
2. B를 루트 노드로 하는 서브트리의 루트 노드에 방문한 후 D로 이동한다.
3. D를 루트 노드로 하는 서브트리의 루트 노드에 방문하고 왼쪽 자식 노드를 방문한다. 왼쪽 자식 노드가 없으므로 오른쪽 자식 노드를 방문한다. 오른쪽 자식 노드도 없으므로 B로 되돌아 간다.
>

위의 1, 2, 3번과 같은 방법을 트리에 재귀적으로 적용하면 전위 순회를 할 수 있다. 중위, 후위 순회도 루트 노드의 방문 순서만 다를 뿐 나머지 방법은 동일하다.

>
루트 노드 $\rightarrow$ 왼쪽 자식 노드 $\rightarrow$ 오른쪽 자식 노드
>

---

### **중위 순회 (Inorder)**
![](/assets/img/data structure/binary tree/inorder.png)


>
왼쪽 자식 노드 $\rightarrow$ 루트 노드 $\rightarrow$ 오른쪽 자식 노드
>

---

### **후위 순회 (Postorder)**
![](/assets/img/data structure/binary tree/postorder.png)


>
왼쪽 자식 노드 $\rightarrow$ 오른쪽 자식 노드 $\rightarrow$ 루트 노드
>

---

### **재귀를 이용한 전위, 중위, 후위 순회 구현**

```java
class Tree {
	private Node root;
	
	private class Node {
		char data;
		Node left, right;
		
		Node(char data) {
			this.data = data;
			left = right = null;
		}
	}
	
	public Node getRoot() {
		return root;
	}
	
	// 임의의 트리로 알아서 초기화
	public void init() {
		Node A = new Node('A');
		Node B = new Node('B');
		Node C = new Node('C');
		Node D = new Node('D');
		Node E = new Node('E');
		Node F = new Node('F');
		
		root = A;
		A.left = B;
		A.right = C;
		B.left = D;
		B.right = E;
		C.left = F;
	} 
	
	public void preorder(Node curRoot) {
		if(curRoot != null) {
			System.out.print(curRoot.data + " ");
			preorder(curRoot.left);
			preorder(curRoot.right);
		}
	}
	
	public void inorder(Node curRoot) {
		if(curRoot != null) {
			inorder(curRoot.left);
			System.out.print(curRoot.data + " ");
			inorder(curRoot.right);
		}
	}
	
	public void postorder(Node curRoot) {
		if(curRoot != null) {
			postorder(curRoot.left);
			postorder(curRoot.right);
			System.out.print(curRoot.data + " ");
		}
	}
}
```

---

### **스택을 이용한 전위, 중위, 후위 순회 구현**

```java
class Tree {
	private Node root;
	
	private class Node {
		char data;
		Node left, right;
		
		Node(char data) {
			this.data = data;
			left = right = null;
		}
	}
	
    public Node getRoot() {
		return root;
	}
    
	// 임의의 트리로 알아서 초기화
	public void init() {
		Node A = new Node('A');
		Node B = new Node('B');
		Node C = new Node('C');
		Node D = new Node('D');
		Node E = new Node('E');
		Node F = new Node('F');
		
		root = A;
		A.left = B;
		A.right = C;
		B.left = D;
		B.right = E;
		C.left = F;
	} 
	
	public void preorder() {
		Stack<Node> s = new Stack<>();
		Node node = root;
		s.push(node);
		
		while(!s.isEmpty()) {
			node = s.pop();
			System.out.print(node.data + " ");
			
			if(node.right != null) {
				s.push(node.right);
			}
			
			if(node.left != null) {
				s.push(node.left);
			}
		}
	}
	
	public void inorder() {
		Stack<Node> s = new Stack<>();
		Node node = root;
		s.push(root);
		
		while(!s.isEmpty()) {
			if(node.left != null) {
				node = node.left;
				s.push(node);
				continue;
			}
			
			node = s.pop();
			System.out.print(node.data + " ");
			
			if(node.right != null) {
				node = node.right;
				s.push(node);
			}
		}
	}
	
	public void postorder() {
		Stack<Node> s = new Stack<>();
		Stack<Node> visit = new Stack<>();
		Node node = root;
		s.push(root);
		
		while(!s.isEmpty()) {
			node = s.pop();
			visit.push(node);
			
			if(node.left != null) {
				s.push(node.left);
			}
			
			if(node.right != null) {
				s.push(node.right);
			}
		}
		
		while(!visit.isEmpty()) {
			System.out.print(visit.pop().data + " ");
		}
	}
}
```

---

### **레벨 순회 (Levelorder)**
![](/assets/img/data structure/binary tree/levelorder.png)

레벨 순회는 순회의 기준이 루트 노드의 탐색 순서가 아니라 레벨인 순회 방법이다. 레벨 순회는 다음과 같은 규칙을 따른다.

>
1. 레벨이 낮은 노드부터 방문한다. 
2. 레벨이 같다면 왼쪽 노드부터 방문한다.
>

---

### **레벨 순회 구현**
```java
class Tree {
	private Node root;
	
	private class Node {
		char data;
		Node left, right;
		
		Node(char data) {
			this.data = data;
			left = right = null;
		}
	}
	
	// 임의의 트리로 알아서 초기화
	public void init() {
		Node A = new Node('A');
		Node B = new Node('B');
		Node C = new Node('C');
		Node D = new Node('D');
		Node E = new Node('E');
		Node F = new Node('F');
		
		root = A;
		A.left = B;
		A.right = C;
		B.left = D;
		B.right = E;
		C.left = F;
	} 
	
	public void levelorder() {
		Queue<Node> q = new LinkedList<>();
		Node node = root;
		q.add(node);
		
		while(!q.isEmpty()) {
			int cnt = q.size();
			
			for(int i = 0; i < cnt; i++) {
				node = q.poll();
				System.out.print(node.data + " ");
				
				if(node.left != null) {
					q.add(node.left);
				}
				
				if(node.right != null) {
					q.add(node.right);
				}
			}
		}
	}
}
```

---

## **스레드 이진 트리**
![](/assets/img/data structure/binary tree/threaded binary tree.png)

스레드 이진 트리는 이진 트리를 순회할 때, 재귀나 스택을 사용하지 않고 순회할 수 있게 만든 트리이다.
N개의 노드로 이진 트리를 만들면 링크는 총 $2*N$개 존재한다. 간선의 개수가 $N-1$개이므로 $N+1$개의 노드가 null 링크인 것을 알 수 있다. 이 null 링크에 스레드를 대입함으로서 더 빠르게 이진 트리를 순회할 수 있게 한다. 중위 순회할 때의 스레드 이진 트리에 대해 알아보자.

중위 순회 결과가 $D -> B -> E -> A -> F -> C$ 인 이진 트리가 있다고 가정한다.
먼저 left 링크가 null일 경우이다. left에 중위 선행자(inorder predecessor)를 저장한다. 중위 선행자는 중위 순회의 결과에서 자신 직전에 방문하는 노드를 말한다. B의 중위 선행자는 D이고, F의 중위 선행자는 A이다.
다음으로 right 링크가 null일 경우이다. right에 중위 후속자(inorder successor)을 저장한다. 중위 후속자는 중위 순회의 결과에서 자신 직후에 방문하는 노드를 말한다. E의 중위 후속자는 A이고, F의 중위 후속자는 C이다.
그렇다면 D의 left와 C의 right는 어떻게 처리할까? Dummy노드를 만들고 그 노드를 가리키게 한다. Dummy노드의 left는 root노드를, right는 자기 자신을 가리키고 있다. 

inorder successor만 있어도 순회가 가능하므로, inorder successor만 저장되어 있다고 가정하겠다.

---

### **스레드 이진 트리 구현**

```java
class Tree {
	private Node root;
	
	private class Node {
		char data;
		Node left, right;
		boolean isThread; // 노드의 right가 thread면 true, childNode면 false
		
		Node(char data) {
			this.data = data;
			left = right = null;
			isThread = false;
		}
		
		private void makeThread(Node thread) {
			this.right = thread;
			this.isThread = true;
		}
	}
	
	// 임의의 트리로 알아서 초기화
	public void init() {
		Node A = new Node('A');
		Node B = new Node('B');
		Node C = new Node('C');
		Node D = new Node('D');
		Node E = new Node('E');
		Node F = new Node('F');
		
		root = A;
		A.left = B;
		A.right = C;
		B.left = D;
		B.right = E;
		C.left = F;
		
		D.makeThread(B);
		E.makeThread(A);
		F.makeThread(C);
	} 
	
	
	private Node getNextNode(Node node) {
		if(node.isThread == true || node.right == null) {
			return node.right;
		}
		
		node = node.right;
		
		while(node.left != null) {
			node = node.left; 
		}
		
		return node;
	}
	
	public void inorder() {
		Node node = root;
		
		while(node.left != null) {
			node = node.left;
		}

		do {
			System.out.print(node.data + " ");
			node = getNextNode(node);
		} while(node != null);
	}
}
```