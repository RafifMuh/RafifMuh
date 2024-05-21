#include <stdio.h>
#include <stdlib.h>

struct node {
    struct node *prev;
    int data;
    struct node *next;
};

typedef struct node node;

node *pHead = NULL;

node *nodeBaru() {
    node *pNew = (node*)malloc(sizeof(node));
    if (pNew == NULL) {
        printf("Error");
        exit(EXIT_FAILURE);
    }
    return pNew;
}

void insertLast(node *pNew) {
    printf("Masukkan Bilangan: ");
    scanf("%d", &pNew->data);
    
    if (pHead == NULL) {
        pNew->prev = pNew;
        pNew->next = pNew;
        pHead = pNew;
    } else {
        pNew->prev = pHead->prev;
        pNew->next = pHead;
        pHead->prev->next = pNew;
        pHead->prev = pNew;
    }
}

void View() {
    node *pWalker = pHead;

    if (pHead == NULL) {
        printf("Data kosong\n");
    } else {
        printf("\nLinked List:\n");
        do {
            printf("[%d] Alamat: %p\n", pWalker->data, (void*)pWalker);
            pWalker = pWalker->next;
        } while (pWalker != pHead);
    }
    printf("\n");
}

void sortList() {
    if (pHead == NULL || pHead->next == pHead) {
        return;
    }

    int count = 0;
    node *temp = pHead;
    do {
        count++;
        temp = temp->next;
    } while (temp != pHead);

    node **array = (node **)malloc(count * sizeof(node *));
    temp = pHead;
    for (int i = 0; i < count; i++) {
        array[i] = temp;
        temp = temp->next;
    }

    for (int i = 0; i < count - 1; i++) {
        for (int j = 0; j < count - i - 1; j++) {
            if (array[j]->data > array[j + 1]->data) {
                node *tempNode = array[j];
                array[j] = array[j + 1];
                array[j + 1] = tempNode;
            }
        }
    }

    for (int i = 0; i < count; i++) {
        array[i]->next = array[(i + 1) % count];
        array[i]->prev = array[(i - 1 + count) % count];
    }

    pHead = array[0];

    free(array);
}

int main() {
    node *pNew;
    int pilih;

    do {
        View();
        printf("\n\n");
        printf("\n====================================== \n\tDOUBLE LINKED LIST\n ======================================\n");
        printf("\n1. masukkan data");
        printf("\n2. urutkan sesuai nilai data");
        printf("\n3. Exit");

        printf("\nPilihan: ");
        scanf("%d", &pilih);
        
        switch (pilih) {
            case 1:
                pNew = nodeBaru();
                insertLast(pNew);
                break;
            case 2:
                sortList();
                break;
            case 3:
                printf("\n");
                break;
            default:
                printf("\nPilihan tidak valid");
                break;
        }
    } while (pilih != 3);

    printf("\n");
    return 0;
}
