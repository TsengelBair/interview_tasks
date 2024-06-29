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

Так как теперь нужно вернуть пару индексов исп-ем unordered_map (то же хэш таблица)

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

