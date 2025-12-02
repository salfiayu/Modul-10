
# <h1 align="center">Laporan Praktikum Struktur Data<br> Modul 10 Tree (Bagian Pertama) </h1>
<p align="center">Salfi Ayu Ramadhani / 103112430224</p>

## Dasar Teori

Tree adalah struktur data non-linear yang merepresentasikan data secara hierarkis dengan simpul (node) yang saling terhubung melalui relasi induk‑anak (parent‑child). Node paling atas disebut root, node tanpa anak disebut leaf. Setiap node (kecuali root) memiliki satu parent, tapi bisa memiliki satu atau lebih child. Tree memungkinkan representasi hubungan kompleks, seperti folder, silsilah, atau organisasi. Jenis umum adalah Binary Tree, di mana tiap node maksimal memiliki dua anak, dan konsep kedalaman (depth) serta tinggi pohon (height) sering digunakan.

## Guided

### Soal 1

```cpp
#include <iostream>
using namespace std;

struct Node
{
    int data;
    Node *kiri, *kanan;
};

Node *buatNode(int nilai)
{
    Node *baru = new Node();
    baru->data = nilai;
    baru->kiri = baru->kanan = NULL;
    return baru;
}

Node *insert(Node *root, int nilai)
{
    if (root == NULL)
        return buatNode(nilai);
    
    if (nilai < root->data)
        root->kiri = insert(root->kiri, nilai);
    else if (nilai > root->data)
        root->kanan = insert(root->kanan, nilai);

    return root;
}

Node *search(Node *root, int nilai)
{
    if (root == NULL || root->data == nilai)
        return root;

    if (nilai < root->data)
        return search(root->kiri, nilai);

    return search(root->kanan, nilai);
}

Node *nilaiTerkecil(Node *node)
{
    Node *current = node;
    while (current && current->kiri != NULL)
        current = current->kiri;

        return current;
}

Node *hapus(Node *root, int nilai)
{
    if (root == NULL)
        return root;

    if (nilai < root->data)
        root->kiri = hapus(root->kiri, nilai);
    else if (nilai > root->data)
        root->kanan = hapus(root->kanan, nilai);
    else
    {
        if (root->kiri == NULL)
        {
            Node *temp = root->kanan;
            delete root;
            return temp;
        }
        else if (root->kanan == NULL){
            Node *temp = root->kiri;
            delete root;
            return temp;
        }
        Node *temp = nilaiTerkecil(root->kanan);
        root->data = temp->data;
        root->kanan = hapus(root->kanan, temp->data);
    }
    return root;
}

Node *update(Node *root, int Lama, int baru)
{
    if (search(root, Lama) != NULL)
    {
        root = hapus(root, Lama);
        root = insert(root, baru);
        cout << "Data " << Lama << " diupdate menjadi " << baru << endl;
    }
    else
    {
        cout << "Data " << Lama << " tidak ditemukan!" << endl;
    }
    return root;
}

void preOrder(Node *root)
{
    if (root != NULL)
    {
        cout << root->data << " ";
        preOrder(root->kiri);
        preOrder(root->kanan);
    }
}

void inOrder(Node *root)
{
    if (root != NULL)
    {
        inOrder(root->kiri);
        cout << root->data << " ";
        inOrder(root->kanan);
    }
}

void postOrder(Node *root)
{
    if (root != NULL)
    {
        postOrder(root->kiri);
        postOrder(root->kanan);
        cout << root->data << " ";
    }
}

int main()
{
    Node *root = NULL;

    cout << "=== 1. INSERT DATA ===" << endl;
    root = insert(root, 10);
    insert(root, 5);
    insert(root, 20);
    insert(root, 3);
    insert(root, 7);
    insert(root, 15);
    insert(root, 25);
    cout << "Data berhasil dimasukan.\n" << endl;

    cout << "=== 2. TAMPILKAN TREE (TRAVELSAL) ===" << endl;
    cout << "PreOrder : ";
    preOrder(root);
    cout << endl;
    cout << "InOrder : ";
    inOrder(root);
    cout << endl;
    cout << "PostOrder : ";
    postOrder(root);
    cout << "\n" << endl;

    cout << "=== 3. TEST SEARCH ===" << endl;
    int cari1 = 7, cari2 = 99;
    cout << "Cari " << cari1 << ": " << (search(root,cari1) ? "Ketemu" : "Tidak Aada") << endl;
    cout << "Cari " << cari2 << ": " << (search(root,cari2) ? "Ketemu" : "Tidak Aada") << endl;
    cout << endl;

    cout << "=== 4. TEST UPDATE ===" << endl;
    root = update(root, 5, 8);
    cout << "Hasil Order setelah update: ";
    cout << endl;
    cout << endl;

    cout << "PreOrder : ";
    preOrder(root);
    cout << endl;
    cout << "InOrder : ";
    inOrder(root);
    cout << endl;
    cout << "PostOrder : ";
    postOrder(root);
    cout << "\n" << endl;

    cout << "== 5. TEST DELETE ===" << endl;
    cout << "Menghapus angka 20..." << endl;
    root = hapus(root, 20);

    cout << "PreOrder : ";
    preOrder(root);
    cout << endl;
    cout << "InOrder : ";
    inOrder(root);
    cout << endl;
    cout << "PostOrder : ";
    postOrder(root);
    cout << "\n" << endl;

    return 0;
}
```

> Screenshoot 
> ![Screenshot Soal 1](https://github.com/salfiayu/Modul-10/blob/main/Modul%2010/screenshoot/Screenshot%20(180).png)


Program ini berfungsi untuk mempraktikkan konsep dasar pengelolaan data dinamis menggunakan pointer dalam Doubly Linked List.

---

## Unguided

### Soal 1

Buatlah ADT Binary Search Tree menggunakan Linked list sebagai berikut di dalam file
“bstree.h”:
Buatlah implementasi ADT Binary Search Tree pada file “bstree.cpp” dan cobalah hasil
implementasi ADT pada file “main.cpp”

```cpp
#include <iostream>
using namespace std;

typedef int infotype;

struct Node {
    infotype info;
    Node *left;
    Node *right;
};

typedef Node* address;

address alokasi(infotype x){
    address p = new Node;
    p->info = x;
    p->left = nullptr;
    p->right = nullptr;
    return p;
}

void insertNode(address &root, infotype x){
    if(root == nullptr){
        root = alokasi(x);
    } else if(x < root->info){
        insertNode(root->left, x);
    } else {
        insertNode(root->right, x);
    }
}

void inOrder(address root){
    if(root){
        inOrder(root->left);
        cout << root->info << " - ";
        inOrder(root->right);
    }
}

int main(){
    cout << "Hello World!" << endl;

    int data[] = {4,2,6,1,3,5,7};
    address root = nullptr;

    for(int x : data) insertNode(root, x);

    cout << "In-order: ";
    inOrder(root);
    cout << endl;

    return 0;
}

```

> Screenshoot  
> ![Screenshot Soal 1](https://github.com/salfiayu/Modul-10/blob/main/Modul%2010/screenshoot/Screenshot%20(183).png)

Program C++ ini membuat Binary Search Tree (BST) dari array {4,2,6,1,3,5,7}. Fungsi insertNode menambahkan elemen ke BST sesuai aturan kiri lebih kecil, kanan lebih besar, sedangkan inOrder menampilkan elemen secara terurut naik. Outputnya adalah 1 - 2 - 3 - 4 - 5 - 6 - 7 -. Tidak dibuat file .h karena saya jadikan 1.

---

### Soal 2

Buatlah fungsi untuk menghitung jumlah node dengan fungsi berikut.
➢ fungsi hitungJumlahNode( root:address ) : integer
/* fungsi mengembalikan integer banyak node yang ada di dalam BST*/
➢ fungsi hitungTotalInfo( root:address, start:integer ) : integer
/* fungsi mengembalikan jumlah (total) info dari node-node yang ada di dalam BST*/
➢ fungsi hitungKedalaman( root:address, start:integer ) : integer
Gambar 10-15 Output

STRUKTUR DATA 89
/* fungsi rekursif mengembalikan integer kedalaman maksimal dari binary tree */

```cpp
#include <iostream>
using namespace std;

typedef int infotype;

struct Node {
    infotype info;
    Node *left;
    Node *right;
};

typedef Node* address;

address alokasi(infotype x){
    address p = new Node;
    p->info = x;
    p->left = nullptr;
    p->right = nullptr;
    return p;
}

void insertNode(address &root, infotype x){
    if(root == nullptr){
        root = alokasi(x);
    } else if(x < root->info){
        insertNode(root->left, x);
    } else {
        insertNode(root->right, x);
    }
}

void inOrder(address root){
    if(root){
        inOrder(root->left);
        cout << root->info << " - ";
        inOrder(root->right);
    }
}

int hitungJumlahNode(address root){
    if(!root) return 0;
    return 1 + hitungJumlahNode(root->left) + hitungJumlahNode(root->right);
}

int hitungTotalInfo(address root){
    if(!root) return 0;
    return root->info + hitungTotalInfo(root->left) + hitungTotalInfo(root->right);
}

int hitungKedalaman(address root){
    if(!root) return 0;
    int L = hitungKedalaman(root->left);
    int R = hitungKedalaman(root->right);
    return 1 + max(L,R);
}

int main(){
    cout << "Hello World!" << endl;

    int data[] = {4,2,6,1,3,5,7};
    address root = nullptr;

    for(int x : data) insertNode(root, x);

    cout << "In-order: ";
    inOrder(root);
    cout << endl;

    cout << "Kedalaman : " << hitungKedalaman(root)-1 << endl;
    cout << "Jumlah node : " << hitungJumlahNode(root) << endl;
    cout << "Total : " << hitungTotalInfo(root) << endl;

    return 0;
}

```

> Sreenshoot 
> ![Screenshot Soal 2](https://github.com/salfiayu/Modul-10/blob/main/Modul%2010/screenshoot/Screenshot%20(184).png)

Program ini membuat Binary Search Tree (BST) dari array {4,2,6,1,3,5,7} dan menampilkan elemen secara in-order. Selain itu, program menghitung kedalaman pohon, jumlah node, dan total nilai semua node. Fungsi insertNode menempatkan elemen sesuai aturan BST, inOrder menampilkan elemen terurut, hitungJumlahNode menghitung total node, hitungTotalInfo menjumlahkan semua nilai node, dan hitungKedalaman menentukan tinggi pohon. Output menunjukkan elemen terurut, kedalaman, jumlah node, dan total nilai node.

### Soal 3

Print tree secara pre-order dan post-order.

```cpp
#include <iostream>
using namespace std;

typedef int infotype;

struct Node {
    infotype info;
    Node *left;
    Node *right;
};

typedef Node* address;

address alokasi(infotype x){
    address p = new Node;
    p->info = x;
    p->left = nullptr;
    p->right = nullptr;
    return p;
}

void insertNode(address &root, infotype x){
    if(root == nullptr){
        root = alokasi(x);
    } else if(x < root->info){
        insertNode(root->left, x);
    } else {
        insertNode(root->right, x);
    }
}

void inOrder(address root){
    if(root){
        inOrder(root->left);
        cout << root->info << " - ";
        inOrder(root->right);
    }
}

void preOrder(address root){
    if(root){
        cout << root->info << " - ";
        preOrder(root->left);
        preOrder(root->right);
    }
}

void postOrder(address root){
    if(root){
        postOrder(root->left);
        postOrder(root->right);
        cout << root->info << " - ";
    }
}

void printTreeBook() {
    cout << "        6" << endl;
    cout << "       / \\" << endl;
    cout << "      4   7" << endl;
    cout << "     / \\" << endl;
    cout << "    2   5" << endl;
    cout << "   / \\" << endl;
    cout << "  1   3" << endl;
}

int main(){
    cout << "=== Soal 3 ===" << endl;

    int data[] = {6,4,7,2,5,1,3};
    address root = nullptr;

    for(int x : data) insertNode(root, x);

    cout << "Pre-order : ";
    preOrder(root);
    cout << endl;

    cout << "In-order  : ";
    inOrder(root);
    cout << endl;

    cout << "Post-order: ";
    postOrder(root);
    cout << endl << endl;

    cout << "Tree Diagram:" << endl;
    printTreeBook();
    cout << endl;

    return 0;
}

```

> Sreenshoot 
> ![Screenshot Soal 3](https://github.com/salfiayu/Modul-10/blob/main/Modul%2010/screenshoot/Screenshot%20(185).png)

Program ini membuat Binary Search Tree (BST) dari array {6,4,7,2,5,1,3} dan menampilkan elemen dengan tiga jenis traversal: pre-order, in-order, dan post-order. Fungsi insertNode menempatkan elemen sesuai aturan BST, sedangkan preOrder, inOrder, dan postOrder menampilkan elemen sesuai urutan traversal masing-masing. Selain itu, program menampilkan diagram pohon secara manual menggunakan printTreeBook untuk memvisualisasikan struktur BST.

## Referensi

1. https://bds.telkomuniversity.ac.id/struktur-data-tree-konsep-jenis-dan-aplikasinya/ (diakses 02-12-2025)

