There is a clever algorithm for computing the Fibonacci numbers in a logarithmic number of steps. Recall the transformation matrix for the Fibonacci numbers:
$$ \begin{bmatrix} 1 & 1 \\ 1 & 0 \end{bmatrix} $$

$Fib(0)$ can be represented as
$$ Fib(0) = (1 , 0) $$

$Fib(1)$ can be represented as
$$ Fib(1) = (1 , 0) \begin{bmatrix} 1 & 1 \\ 1 & 0 \end{bmatrix} = (1, 1) $$

Thus, we can represent $Fib(n)$ as
$$ Fib(n) = (1 , 0) \begin{bmatrix} 1 & 1 \\ 1 & 0 \end{bmatrix}^n $$

To calculate $Fib(n)$, simply calculate the n-th power of the transformation matrix.
and the n-th power of the transformation matrix can be calculated in $O(\log n)$ time using the fast exponentiation of matrix

```scheme
; detailed matrix multiplication omitted
(define (mat-* a b) ...)

(define (mat-ref a i j) ...)

; to simplify the code, we assume that matrix is 2x2
(define (make-mat a b c d) ...)

(define (mat-expt mat n)
  (cond ((= n 0) (make-mat 1 0 0 1))
        ((even? n) (mat-expt (mat-* mat mat) (/ n 2)))
        (else (mat-* mat (mat-expt (mat-* mat mat) (/ (- n 1) 2))))))

(define (fib n)
  (cond ((= n 0) 0)
        (else (mat-ref (mat-* (make-vec 1 0) (mat-expt (make-mat 1 1 1 0) n)) 0 1))))
```
