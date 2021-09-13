# Elvin's C Quiz
`This is a succinct quiz to test your C language programming performance.`


***Rookie Warning : NO EASY SHIT HERE***

#### 1. Pseudo Random Number Generator
```
/** in 32bit or 64bit environment **/
#include <stdlib.h>
#include <stdio.h>

#define RANGE 4

int main(int argc, char** argv) {
    int count[RANGE] = {0};

    for (int i = 0; i < 1000000; i++) {
        int r = rand() % 8;

        r %= RANGE;

        count[r]++;
    }

    for (int i = 0; i < RANGE; i++)
        printf("%d ", count[i]);
    putchar('\n');
    return 0;
}

```
`Q1`: What does this program do?

`Q2`: What is the most likely distribution of the output line?

`Q3`: If 3 and then 5 are set to the RANGE macro, what distribution would them be?

*Code Snippet Changed*

> From

```
        int r = rand() % 8;

        r %= RANGE;
```
> To

```
        int r = rand() % RANGE;
```
`Q4`: What is the distribution now for 4, 3 and 5?

`Q5`: Can you explain the difference?

#### 2. Linked List

```
struct linked_list{
    struct linked_list *next;
    int val;
};
/*
 * This function finds one and delete one, then returns the modified list
 */
struct linked_list* delete_one_by_val(struct linked_list* head, int val) {
    if (head->val != val && NULL != head)
        return delete_one_by_val(head->next, val);
    else if (NULL != head)
        return head->next;

    return NULL;
}
```

`Q1`: How many bug(s) have you found and where are they?

*Hint: Consider calling this function frequently during same runtime.*

`Q2`: Can you modified this function so it can find all the matching node and delete them?
