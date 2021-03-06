---
title: Лекция 11 - Двусвързан Списък
category: новини
tags:
  - лекции
  - материали
---

### Код от час

#### А клас
```c
#include <stdlib.h>
#include <stdio.h>

struct node_t {
  struct node_t* next;
  struct node_t* prev;
  int value;
};

struct list_t {
  struct node_t* head;
  int size;
};

void push_front(struct list_t* list, int value) {
  struct node_t* new_node = malloc(sizeof(struct node_t));
  new_node->value = value;

  new_node->next = list->head; // 1

  new_node->prev = NULL; // 2
  if(list->head != NULL)
    list->head->prev = new_node; // 3

  list->head = new_node; // 4

  list->size++;
}

void push_back(struct list_t* list, int value) {
  if(list->head == NULL) {
    push_front(list, value);
    return;
  }

  struct node_t* new_node = malloc(sizeof(struct node_t));
  new_node->value = value;

  new_node->next = NULL;

  /*if(list->head == NULL) {
    list->head = new_node;
    new_node->prev = NULL;
    return;
  }*/

  struct node_t* curr = list->head;
  while(curr->next != NULL) {
    curr = curr->next;
  }

  curr->next = new_node;
  new_node->prev = curr;

  list->size++;
}

void insert_middle(struct list_t* list, int value) {
  if(list->head == NULL) {
    push_front(list, value);
    return;
  }

  struct node_t* new_node = malloc(sizeof(struct node_t));
  new_node->value = value;

  struct node_t* curr = list->head;
  int counter = 0;
  while(counter < (list->size / 2) - 1) {
    curr = curr->next;
    counter++;
  }

  printf("%d %d\n", counter, curr->value);

  struct node_t* left = curr;
  struct node_t* right = curr->next;

  left->next = new_node;
  right->prev = new_node;

  new_node->next = right;
  new_node->prev = left;
}

void print_list(struct list_t* list) {
  struct node_t* curr = list->head;
  int counter = 1;
  printf("size == %d\n", list->size);

  while(curr != NULL) {
    printf("[%d] %d\n", counter++, curr->value);
    curr = curr->next;
  }
}

int main() {
  struct list_t my_list = {NULL, 0};

  push_front(&my_list, 4);
  push_front(&my_list, 6);
  push_front(&my_list, 7);
  push_front(&my_list, 1);

  push_back(&my_list, 44);
  push_back(&my_list, 66);
  push_back(&my_list, 77);
  push_back(&my_list, 11);

  insert_middle(&my_list, 13);

  print_list(&my_list);
}

```

#### Б клас
```c
#include <stdlib.h>
#include <stdio.h>

struct node_t {
  int value;
  struct node_t* next;
  struct node_t* prev;
};

struct list_t {
  struct node_t* head;
  struct node_t* tail;
};

void push_front(struct list_t* list, int value) {
  struct node_t* new_node = malloc(sizeof(struct node_t));
  new_node->value = value;

  new_node->next = list->head;
  new_node->prev = NULL;

  if(list->head != NULL) {
    list->head->prev = new_node;
  } else {
    list->tail = new_node;
  }
  list->head = new_node;
}

void push_back(struct list_t* list, int value) {
  if(list->head == NULL) {
    push_front(list, value);
    return;
  }

  struct node_t* new_node = malloc(sizeof(struct node_t));
  new_node->value = value;

 // if(list->head == NULL) {
  //  new_node->next = NULL;
  //  new_node->prev = NULL;

  //  list->head = new_node;
   // return;
//  }

  /*struct node_t* old_last = list->head;
  while(old_last->next != NULL) {
    old_last = old_last->next;
  }*/

  /*new_node->next = NULL;
  new_node->prev = old_last;

  old_last->next = new_node;*/

  new_node->next = NULL;
  new_node->prev = list->tail;

  //if(list->tail != NULL)
  list->tail->next = new_node;
  list->tail = new_node;
}

void print_list(struct list_t* list) {
  struct node_t* curr = list->head;
  int counter = 1;
  //("size == %d\n", list->size);

  while(curr != NULL) {
    printf("[%d] %d\n", counter++, curr->value);
    curr = curr->next;
  }
}

int main() {
  struct list_t my_list = {NULL, NULL};

  /*push_front(&my_list, 16);
  push_front(&my_list, 1);
  push_front(&my_list, 6);*/

  push_back(&my_list, 13);
  push_back(&my_list, 23);
  push_back(&my_list, 33);

  print_list(&my_list);

  return 0;
}
```

#### В клас
```c
#include <stdlib.h>
#include <stdio.h>

struct node_t {
  int value;
  struct node_t* next;
  struct node_t* prev;
};

struct list_t {
  struct node_t* head;
  struct node_t* tail;
  int size;
};

void push_front(struct list_t* list, int value) {
  struct node_t* new_node = malloc(sizeof(struct node_t));
  new_node->value = value;

  new_node->prev = NULL;
  if(list->head != NULL) {
    list->head->prev = new_node;
  } else {
    list->tail = new_node;
  }
  new_node->next = list->head;

  list->head = new_node;

  list->size++;
}

void push_back(struct list_t* list, int value) {
  if(!list->head) {
    push_front(list, value);
    return;
  }

  struct node_t* new_node = malloc(sizeof(struct node_t));
  new_node->value = value;

  new_node->next = NULL;
  new_node->prev = list->tail;

  list->tail->next = new_node;
  list->tail = new_node;

  list->size++;
}

void print_list(struct list_t* list) {
  struct node_t* curr = list->head;
  int counter = 1;
  printf("size == %d\n", list->size);

  while(curr != NULL) {
    printf("[%d] %d\n", counter++, curr->value);
    curr = curr->next;
  }
}

void insert_middle(struct list_t* list, int value) {
  struct node_t* new_node = malloc(sizeof(struct node_t));
  new_node->value = value;

  struct node_t* curr = list->head;
  int counter = 1;
  // s == 6; c = 0; i = 1
  // s == 6; c = 1; i = 2
  // s == 6; c = 2; i = 3
  // s == 6; c = 3; i = 4
  // s == 6; c = 4; i = 5
  while(counter < list->size / 2) {
    curr = curr->next;
    counter++;
  }
  //printf("%d %d\n", counter, curr->value);
  new_node->prev = curr;
  new_node->next = curr->next;

  curr->next->prev = new_node;
  curr->next = new_node;

  list->size++;
}

void pop_front(struct list_t* list) {
  if(!list->head) { // list->size == 0
    return;
  }

  if(!list->head->next) { // list->size == 1
    free(list->head);
    list->head = NULL;
    list->size--;
    return;
  }

  list->head = list->head->next;
  free(list->head->prev);
  list->head->prev = NULL;
  list->size--;
}

void pop_back(struct list_t* list) {
  if(list->size < 2) {
    pop_front(list);
    return;
  }

  struct node_t* curr = list->head;
  while(curr->next != NULL) {
    curr = curr->next;
  }

  curr->prev->next = NULL;
  free(curr);
  list->size--;
}

int main() {
  struct list_t my_list = {NULL, NULL, 0};

  push_front(&my_list, 10);
  push_front(&my_list, 7);
  push_front(&my_list, 103);

  push_back(&my_list, 5);
  push_back(&my_list, 12);

  insert_middle(&my_list, 18);

  print_list(&my_list);
  puts("\n\n");

  pop_front(&my_list);
  pop_back(&my_list);

  print_list(&my_list);

  return 0;
}
```
