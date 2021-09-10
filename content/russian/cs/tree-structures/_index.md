---
title: Деревья поиска
editors:
- Сергей Слотин
weight: 9
---

Дерево — одна из наиболее распространенных структур данных в программировании.

Деревья состоят из набора вершин (узлов, нод) и ориентированных рёбер (ссылок) между ними. Вершины связаны таким образом, что от какой-то одной вершины, называемой *корневой* (вершина 8 на рисунке), можно дойти до всех остальных единственным способом.

![Деревья принято рисовать корнем вверх](/img/bst.svg)

Связанные определения:
- **Родитель** вершины $v$ — вершина, из которой есть прямая ссылка в $v$. 
- **Дети** (дочерние элементы, сын, дочь) вершины $v$ — вершины, в которые из $v$ есть прямая ссылка.
- **Предки** — родители родителей, их родители, и так далее.
- **Потомки** — дети детей, их дети, и так далее.
- **Лист** (терминальная вершина) — вершина, не имеющая детей.
- **Поддерево** — вершина дерева вместе со всеми её потомками.
- **Степерь** вершины — количество её детей.
- **Глубина** вершины — расстояние от корня до неё.
- **Высота** дерева — максимальная из глубин всех вершин.

Деревья чаще всего представляются в памяти как динамически создаваемые структуры с явными указателями на своих детей, либо как элементы массива связанные отношениями, неявно определёнными их позициями в массиве.

Деревья также используются в [контексте графов](/cs/trees).

## Бинарные деревья поиска

Бинарное дерево поиска (англ. *binary search tree*, BST) — дерево, для которого выполняются следующие свойства:

- У каждой вершины не более двух детей.
- Все вершины обладают *ключами*, на которых определена операция сравнения (например, целые числа или строки).
- У всех вершин *левого* поддерева вершины $v$ ключи *не больше*, чем ключ $v$.
- У всех вершин *правого* поддерева вершины $v$ ключи *больше*, чем ключ $v$.
- Оба поддерева — левое и правое — являются двоичными деревьями поиска.

Более общим понятием являются обычные (не бинарные) деревья поиска — в них количество детей может быть больше двух, и при этом в «более левых» поддеревьях ключи должны быть меньше, чем «более правых». Пока что мы сконцентрируемся только на двоичных, потому что они проще.

Чаще всего бинарные деревья поиска хранят в виде структур — по одной на каждую вершину — в которых записаны ссылки (возможно, пустые) на правого и левого сына, ключ и, возможно, какие-то дополнительные данные.

```cpp
struct Node {
    int x;
    Node *l, *r;
};
```

Как можно понять по названию, основное преимущество бинарных деревьев *поиска* в том, что в них можно легко производить поиск элементов:

```cpp
bool find(Node *v, int x) {
    if (!v)
        return false;
    if (v->x == x)
        return true;
    return (v->x < x) ? find(v->l, x) : find(v->r, x);
}
```

Эта функция — как и многие другие основные, например, вставка или удаление элементов — работает в худшем случае за высоту дерева. Высота бинарного дерева в худшем случае может быть $O(n)$ («бамбук»), поэтому в эффективных реализациях поддерживаются некоторые инварианты, гарантирующую *среднюю* глубину вершины $O(\log n)$ и соответствующую стоимость основных операций. Такие деревья называются *сбалансированными*.