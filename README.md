## Задачи, которые попадались мне на собесах

### 1. Найти все пары элементов массива, дающих в сумме исходное число за линейное время

Исходные данные:
```c++
vector<int> nums = { -3, 0, 1, 3, 4, 8 };
int target = 5;
```
Решение:
```c++
vector<pair<int, int>> twoSum(vector<int>&array, int target) {
    unordered_set<int>mySet;
    vector<pair<int, int>>result;

    for (int i = 0; i < array.size(); ++i) {
        int numToFind = target - array[i];
        if (mySet.find(numToFind) != mySet.end()) {
            result.push_back(std::make_pair(array[i], numToFind));
        }
        else {
            mySet.insert(array[i]);
        }
    }

    return result;
};
```

Данная задача спокойно решается двойным обходом, но проблема в том, что такой подход имеет сложность O(n^2)

В случае же исп-я хэш таблицы алгоритм будет иметь сложность O(n) - т.е. линейную.

На каждой итерации проверяем, есть ли нужный (numToFind) эл-т в таблице, если такой есть, создаем пару и пушим в результирующий массив, если нет, просто добавляем.

Операция поиска в хэш таблице обладает сложностью O(1) и поэтому всего за один обход находим искомые эл-ты.

### Та же задача, но теперь нужно вернуть не эл-ты а их индексы

Так как теперь нужно вернуть пару индексов исп-ем unordered_map (тоже хэш таблица)

```c++
vector<pair<int, int>> twoSum(vector<int>& array, int target) {
    unordered_map<int, int> myMap;
    vector<pair<int, int>> res;

    for (int i = 0; i < array.size(); ++i) {
        int numToFind = target - array[i];

        if (myMap.find(numToFind) != myMap.end()) {
            res.push_back(std::make_pair(myMap[numToFind], i));
        }

        myMap[array[i]] = i; 
    }
    return res;
}
```

### 2. Развернуть список 

Исходные данные:
```c++
list<int>myList = { 1, 2, 3, 4, 5 };
```

Список состоит из эл-тов, каждый эл-т по своей структуре содержит указатель на предыдущий и следующий эл-т.

Решается с помощью двух итераторов на начало и конец списка, по сути нужно просто поменять значения двух эл-тов на которые указывают итераторы 

```c++
list<int> reverseList(list<int>&p_list) {
    auto it1 = p_list.begin();
    auto it2 = prev(p_list.end()); // не на .end(), т.к. итератор end всегда указывает на позицию следующую за последним эл-том

    // distance вычисляет кол-во эл-тов между двумя итераторами
    while (distance(it1, it2) > 0) {
        int tmp = *it1;
        *it1 = *it2;
        *it2 = tmp;

        ++it1;
        --it2;
    }
    return p_list;
};
```

### 3. Написать реализацию бинарного дерева с методами вставки, поиска и вывода в консоль

Бинарное дерево - это набор структур (Nodes), сама структура содержит в себе какие то данные (int data) и два указателя на левого и правого потомка 

```c++
struct Node {
    int data;
    Node* left;
    Node* right;
}
```

Решение 

```c++
struct Node
{
	int num;
	Node* right;
	Node* left;

	Node(int n) : num(n), right(nullptr), left(nullptr) {};

	// Вывести в консоль все эл-ты inOrder
	void inOrder() {
		if (left != nullptr) {
			left->inOrder();
		}

		cout << num << " ";

		if (right != nullptr) {
			right->inOrder();
		}
	};

	// Вставка
	void insert(int n) {
		if (n > num) {
			if (right == nullptr) {
				right = new Node(n);
			}
			else {
				right->insert(n);
			}
		}
		else {
			if (left == nullptr) {
				left = new Node(n);
			}
			else {
				left->insert(n);
			}
		}
	}

	// Поиск
	Node* search(int n) {
		if (n == num) {
			return this;
		}
		else if (n < num && left != nullptr) {
			left->search(n);
		}
		else if (n > num && right != nullptr) {
			right->search(n);
		}
		else {
			return nullptr;
		}
	};
};
```

