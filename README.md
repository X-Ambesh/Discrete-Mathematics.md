#### Q1. Create a class SET. Create member functions to perform the following SET operations: a. is member: check whether an element belongs to the set or not and return value as true/false. b. powerset: list all the elements of the power set of a set. c. subset: check whether one set is a subset of the other or not. d. union and intersection of two Sets. e. complement: assume universal set as per the input elements from the user. f. set difference and symmetric difference between two sets. g. cartesian product of sets. Write a menu driven program to perform the above functions on an instance of the SET class.


```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

class SET {
private:
    vector<int> elements;

public:
   
    void inputSet() {
        int n, x;
        cout << "Enter number of elements: ";
        cin >> n;
        elements.clear();
        cout << "Enter elements:\n";
        for (int i = 0; i < n; i++) {
            cin >> x;
            if (find(elements.begin(), elements.end(), x) == elements.end())
                elements.push_back(x);
        }
    }

    void display() {
        cout << "{ ";
        for (int x : elements)
            cout << x << " ";
        cout << "}\n";
    }

    bool isMember(int x) {
        return find(elements.begin(), elements.end(), x) != elements.end();
    }

    void powerSet() {
        int n = elements.size();
        int total = 1 << n;

        cout << "Power Set:\n";
        for (int i = 0; i < total; i++) {
            cout << "{ ";
            for (int j = 0; j < n; j++) {
                if (i & (1 << j))
                    cout << elements[j] << " ";
            }
            cout << "}\n";
        }
    }

    bool isSubset(SET s2) {
        for (int x : elements) {
            if (!s2.isMember(x))
                return false;
        }
        return true;
    }

    SET setUnion(SET s2) {
        SET result = *this;
        for (int x : s2.elements) {
            if (!result.isMember(x))
                result.elements.push_back(x);
        }
        return result;
    }

    SET intersection(SET s2) {
        SET result;
        for (int x : elements) {
            if (s2.isMember(x))
                result.elements.push_back(x);
        }
        return result;
    }

    SET complement(SET universal) {
        SET result;
        for (int x : universal.elements) {
            if (!isMember(x))
                result.elements.push_back(x);
        }
        return result;
    }

    SET difference(SET s2) {
        SET result;
        for (int x : elements) {
            if (!s2.isMember(x))
                result.elements.push_back(x);
        }
        return result;
    }

    SET symmetricDifference(SET s2) {
        SET diff1 = difference(s2);
        SET diff2 = s2.difference(*this);
        return diff1.setUnion(diff2);
    }

    void cartesianProduct(SET s2) {
        cout << "Cartesian Product:\n";
        for (int x : elements) {
            for (int y : s2.elements) {
                cout << "(" << x << ", " << y << ") ";
            }
            cout << endl;
        }
    }
};

int main() {
    SET A, B, U;
    int choice, x;

    cout << "Enter Set A:\n";
    A.inputSet();

    cout << "Enter Set B:\n";
    B.inputSet();

    cout << "Enter Universal Set:\n";
    U.inputSet();

    do {
        cout << "\n--- MENU ---\n";
        cout << "1. Check Membership\n";
        cout << "2. Power Set\n";
        cout << "3. Subset Check\n";
        cout << "4. Union\n";
        cout << "5. Intersection\n";
        cout << "6. Complement\n";
        cout << "7. Difference (A - B)\n";
        cout << "8. Symmetric Difference\n";
        cout << "9. Cartesian Product\n";
        cout << "0. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
            cout << "Enter element: ";
            cin >> x;
            if (A.isMember(x))
                cout << "Element is in Set A\n";
            else
                cout << "Element is NOT in Set A\n";
            break;

        case 2:
            A.powerSet();
            break;

        case 3:
            if (A.isSubset(B))
                cout << "A is subset of B\n";
            else
                cout << "A is NOT subset of B\n";
            break;

        case 4: {
            SET result = A.setUnion(B);
            cout << "Union: ";
            result.display();
            break;
        }

        case 5: {
            SET result = A.intersection(B);
            cout << "Intersection: ";
            result.display();
            break;
        }

        case 6: {
            SET result = A.complement(U);
            cout << "Complement of A: ";
            result.display();
            break;
        }

        case 7: {
            SET result = A.difference(B);
            cout << "A - B: ";
            result.display();
            break;
        }

        case 8: {
            SET result = A.symmetricDifference(B);
            cout << "Symmetric Difference: ";
            result.display();
            break;
        }

        case 9:
            A.cartesianProduct(B);
            break;

        case 0:
            cout << "Exiting...\n";
            break;

        default:
            cout << "Invalid choice!\n";
        }

    } while (choice != 0);

    return 0;
}
```

---

#### Q2. Create a class RELATION, use Matrix notation to represent a relation. Include member functions to check if the relation is Reflexive, Symmetric, Anti-symmetric, Transitive. Using these functions check whether the given relation is: Equivalence or Partial Order relation or None


```cpp
#include <iostream>
using namespace std;

class RELATION {
    int n;
    int matrix[10][10];

public:
    void input() {
        cout << "Enter number of elements: ";
        cin >> n;

        cout << "Enter relation matrix (0 or 1):\n";
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                cin >> matrix[i][j];
            }
        }
    }

    bool isReflexive() {
        for (int i = 0; i < n; i++) {
            if (matrix[i][i] != 1)
                return false;
        }
        return true;
    }

    bool isSymmetric() {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] != matrix[j][i])
                    return false;
            }
        }
        return true;
    }

    bool isAntisymmetric() {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (i != j && matrix[i][j] == 1 && matrix[j][i] == 1)
                    return false;
            }
        }
        return true;
    }

    bool isTransitive() {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 1) {
                    for (int k = 0; k < n; k++) {
                        if (matrix[j][k] == 1 && matrix[i][k] != 1)
                            return false;
                    }
                }
            }
        }
        return true;
    }

    void checkRelationType() {
        bool reflexive = isReflexive();
        bool symmetric = isSymmetric();
        bool antisymmetric = isAntisymmetric();
        bool transitive = isTransitive();

        cout << "\nProperties:\n";
        cout << "Reflexive: " << (reflexive ? "Yes" : "No") << endl;
        cout << "Symmetric: " << (symmetric ? "Yes" : "No") << endl;
        cout << "Anti-symmetric: " << (antisymmetric ? "Yes" : "No") << endl;
        cout << "Transitive: " << (transitive ? "Yes" : "No") << endl;

        if (reflexive && symmetric && transitive) {
            cout << "\nRelation is an Equivalence Relation.\n";
        }
        else if (reflexive && antisymmetric && transitive) {
            cout << "\nRelation is a Partial Order Relation.\n";
        }
        else {
            cout << "\nRelation is None.\n";
        }
    }
};

int main() {
    RELATION r;
    r.input();
    r.checkRelationType();
    return 0;
}
```

---

#### Q3 Write a Program that generates all the permutations of a given set of digits, with or without repetition. 


> 1. Without Repetition (each digit used once)
```cpp
#include <iostream>
#include <vector>
using namespace std;

void permute(vector<int>& arr, int l, int r) {
    if (l == r) {
        for (int x : arr)
            cout << x << " ";
        cout << endl;
        return;
    }

    for (int i = l; i <= r; i++) {
        swap(arr[l], arr[i]);
        permute(arr, l + 1, r);
        swap(arr[l], arr[i]); 
    }
}

int main() {
    int n;
    cout << "Enter number of digits: ";
    cin >> n;

    vector<int> arr(n);
    cout << "Enter digits:\n";
    for (int i = 0; i < n; i++)
        cin >> arr[i];

    cout << "\nPermutations without repetition:\n";
    permute(arr, 0, n - 1);

    return 0;
}
```

> 2. With Repetition (digits can repeat)

```cpp
#include <iostream>
#include <vector>
using namespace std;

void permuteWithRepetition(vector<int>& arr, vector<int>& result, int index, int n) {
    if (index == n) {
        for (int x : result)
            cout << x << " ";
        cout << endl;
        return;
    }

    for (int i = 0; i < arr.size(); i++) {
        result[index] = arr[i];
        permuteWithRepetition(arr, result, index + 1, n);
    }
}

int main() {
    int n, k;
    cout << "Enter number of digits: ";
    cin >> n;

    vector<int> arr(n);
    cout << "Enter digits:\n";
    for (int i = 0; i < n; i++)
        cin >> arr[i];

    cout << "Enter length of permutation: ";
    cin >> k;

    vector<int> result(k);

    cout << "\nPermutations with repetition:\n";
    permuteWithRepetition(arr, result, 0, k);

    return 0;
}
```

---

#### Q4 For any number n, write a program to list all the solutions of the equation x1 + x2 + x3 + ...+ xn = C, where C is a constant (C<=10) and x1, x2, x3,...,xn are nonnegative integers, using brute force strategy.

```cpp
#include <iostream>
using namespace std;

int x[20]; 

void solve(int index, int n, int C) {
    if (index == n) {
        int sum = 0;
        for (int i = 0; i < n; i++)
            sum += x[i];

        if (sum == C) {
            for (int i = 0; i < n; i++)
                cout << x[i] << " ";
            cout << endl;
        }
        return;
    }

    for (int i = 0; i <= C; i++) {
        x[index] = i;
        solve(index + 1, n, C);
    }
}

int main() {
    int n, C;
    cout << "Enter number of variables (n): ";
    cin >> n;
    cout << "Enter constant (C <= 10): ";
    cin >> C;

    solve(0, n, C);

    return 0;
}
```

#### Q5 Write a Program to evaluate a polynomial function. (For example store f(x) = 4n2 + 2n + 9 in an array and for a given value of n, say n = 5, compute the value of f(n)).

```cpp
#include <iostream>
#include <cmath>
using namespace std;

int main() {
    int degree, n;

    cout << "Enter degree of polynomial: ";
    cin >> degree;

    int coeff[degree + 1];

    cout << "Enter coefficients (from highest degree to constant term):\n";
    for (int i = 0; i <= degree; i++) {
        cin >> coeff[i];
    }

    cout << "Enter value of n: ";
    cin >> n;

    int result = 0;

    for (int i = 0; i <= degree; i++) {
        result += coeff[i] * pow(n, degree - i);
    }

    cout << "Value of polynomial = " << result << endl;

    return 0;
}
```

---

#### Q4 Write a Program to check if a given graph is a complete graph. Represent the graph using the Adjacency Matrix representation.

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;
    cout << "Enter number of vertices: ";
    cin >> n;

    int adj[n][n];

    cout << "Enter the adjacency matrix:\n";
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < n; j++) {
            cin >> adj[i][j];
        }
    }

    bool isComplete = true;

    for(int i = 0; i < n; i++) {
        for(int j = 0; j < n; j++) {
            if(i == j && adj[i][j] != 0) {
                isComplete = false;
                break;
            }
            if(i != j && adj[i][j] != 1) {
                isComplete = false;
                break;
            }
        }
        if(!isComplete) break;
    }

    if(isComplete)
        cout << "The graph is a Complete Graph.\n";
    else
        cout << "The graph is NOT a Complete Graph.\n";

    return 0;
}
```

---

#### Q7 Write a Program to accept a directed graph G and compute the in-degree and outdegree of each vertex.

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;

    cout << "Enter number of vertices: ";
    cin >> n;

    int adj[n][n];

    cout << "Enter adjacency matrix:\n";
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < n; j++) {
            cin >> adj[i][j];
        }
    }

    int indegree[n] = {0};
    int outdegree[n] = {0};

    for(int i = 0; i < n; i++) {
        for(int j = 0; j < n; j++) {
            if(adj[i][j] == 1) {
                outdegree[i]++;
                indegree[j]++;
            }
        }
    }

    cout << "\nVertex\tIn-degree\tOut-degree\n";
    for(int i = 0; i < n; i++) {
        cout << i << "\t" << indegree[i] << "\t\t" << outdegree[i] << endl;
    }

    return 0;
}
```

---
