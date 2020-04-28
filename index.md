
> 不该少于两万五，不，肯定超过两万八。 -- 陀氏《卡拉马佐夫兄弟》

# 首页暂时不放东西，给你们看看我的漂亮代码

> 这是前一阵帮别人做的控制原理的作业题

```

#include <stdio.h>
#include <stdlib.h>

#define ITERATION 1000

/*
 * Note:
 * matrix utils maybe compromised if their size does no match.
 **/

typedef struct matrix {
  unsigned int scale;
  double **cell;
} m_t;

m_t* m_new(unsigned int scale) {
  //allocate
  m_t *new;

  new = malloc(sizeof(m_t));
  new->scale = scale;
  new->cell = malloc(sizeof(double*) * scale);

  while (scale--) {
    new->cell[scale] = malloc(sizeof(double) * new->scale);
  }

  return new;
}
m_t* m_read() {
  //read first integer
  int scale;
  m_t *new;

  scanf("%d", &scale);
  new = m_new(scale);

  //read value
  unsigned int i, j;

  for (i = 0; i < scale; i++) {
    for (j = 0; j < scale; j++) {
      //read each value
      scanf("%lf", new->cell[i] + j);
    }
  }
  return new;
}
void m_delete(m_t *m) {
  //deallocate
  while (m->scale--) {
    free(m->cell[m->scale]);
  }
  free(m->cell);
  free(m);
}


void m_I(m_t *m) {
  //matrix I initializer
  unsigned int i, j;
  for (i = 0; i < m->scale; i++) {
    for (j = 0; j< m->scale; j++) {
      if (i == j)
        m->cell[i][j] = 1.;
      else
        m->cell[i][j] = 0.;
    }
  }
}
void m_0(m_t *m) {
  //matrix I initializer
  unsigned int i, j;
  for (i = 0; i < m->scale; i++) {
    for (j = 0; j< m->scale; j++) {
      m->cell[i][j] = 0.;
    }
  }
}
void m_add(m_t *m1, m_t *m2, m_t * m_desc) {
  // check if their size is mismatched
  if (m1->scale != m2->scale || m1->scale != m_desc->scale)
    return;

  unsigned int i, j;


  //add m1, m2 to m_desc
  for (i = 0; i < m1->scale; i++) {
    for (j = 0; j< m1->scale; j++) {
      m_desc->cell[i][j] = m1->cell[i][j] + m2->cell[i][j];
    }
  }
}
void m_multi(m_t *m_l, m_t *m_r, m_t *m_desc) {
  //fused multiple & add: m := m + mLeft * mRight
  unsigned int i, j;
  for (i = 0; i < m_desc->scale; i++) {
    for (j = 0; j< m_desc->scale; j++) {
      // for each cell
      unsigned int k;

      //purge value
      m_desc->cell[i][j] = 0.;

      for (k = 0; k < m_desc->scale; k++) {
        // add value to desc
        m_desc->cell[i][j] += m_l->cell[i][k] * m_r->cell[k][j];
      }
    }
  }
}
void m_scalar(m_t *m, m_t *m_desc, double scalar) {
  // matrix scalar multiplication.
  unsigned int i, j;

  for (i = 0; i < m->scale; i++) {
    for (j = 0; j< m->scale; j++) {
      // for each cell
      m_desc->cell[i][j] = m->cell[i][j] * scalar;
    }
  }
}
void m_dump(m_t *m) {
  //print matrix
  unsigned int i, j;

  for (i = 0; i < m->scale; i++) {
    for (j = 0; j< m->scale; j++) {
      printf("%lf ", m->cell[i][j]);
    }
    printf("\n");
  }
  printf("\n");
}

int main(void) {
  double t, divisor, scalar;
  m_t *A, *A_k[2], *A_k_scalar, *e_At;
  int i;

  // read t
  scanf("%lf", &t);

  // read matrix A
  A = m_read();

  // allocate matrix A_k slipper
  A_k[0] = m_new(A->scale);
  A_k[1] = m_new(A->scale);

  // initialize first slipper A^k 1
  m_I(A_k[1]);

  // allocate pipline A^k with scalar
  A_k_scalar = m_new(A->scale);

  // allocate matrix e^At and initialize it as I
  e_At = m_new(A->scale);
  m_I(e_At);

  // initialize scalar & divisor
  divisor = 1.;
  scalar = t;

  // main iteration
  for (i = 0; i < ITERATION; i++) {
    // for each term
    // calculate A^k
    m_multi(A_k[(i + 1) & 1], A, A_k[i & 1]);

    //calculate A^k with scalar (term k)
    m_scalar(A_k[i & 1], A_k_scalar, scalar);

    // add A^k to e_At
    m_add(A_k_scalar, e_At, e_At);

    // compute next scalar
    divisor += 1;
    scalar *= t;
    scalar /= divisor;

  }

  // print sum
  printf("Sum of %d loop:\n", ITERATION);
  m_dump(e_At);

  // calculate "ITERATION + 1" th term.
  m_multi(A_k[(i + 1) & 1], A, A_k[i & 1]);
  m_scalar(A_k[i & 1], A_k_scalar, scalar);

  printf("%d th term:\n", ITERATION + 1);
  m_dump(A_k_scalar);

  // deallocate memory
  m_delete(A);
  m_delete(A_k[0]);
  m_delete(A_k[1]);
  m_delete(A_k_scalar);
  m_delete(e_At);
  return 0;
}


```

* [对影评的一些疑问](posts/2020-03-11-mreview.md)
* [散文诗 20200213](posts/2020-02-13-v.md)
* [买猪](posts/2020-02-09-pig.md) 2020-02
* [瘟疫公司](posts/2020-02-02-ncov.md) 2020-02
* [格利高里神父](posts/2020-01-05-hl2.md) 2020-01
* [花朵快快长](posts/2019-12-21-none.md) 2019-12
* [201Q年的一些经历和感想](posts/2019-11-30-q.md) 2019-11
* [An English Essay about English](posts/2019-11-english.md) 2019-11
* [Jupiter and Beyond the Infinite/木星以及超越一切苍穹](posts/2019-11-26-idx.md) 2019-11

#### [小王](index_wang.md)

#### [几首现代诗](index_mverse.md)

#### [Tech Spot](index_tech.md)

#### [几首当代诗（摘抄](contemporary/intro.md)

#### [豆瓣和空间的备份](index_history.md)

#### [计划](posts/plan.md)
