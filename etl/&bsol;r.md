
# Maybe end of testing, or maybe not

> Maybe a new start (of a line), or maybe not

```lisp
;______________________________________Lisp Meta-Circular Evaluator S-Expression
;this code is written in the order it appears on pages 10-13 in the Lisp 1.5 Manual 
;and is translated from the m-expressions into s-expressions

(label mc.equal (lambda (x y)
    (mc.cond
        ((atom x) ((mc.cond ((atom y) (eq x y)) ((quote t) (quote f)))))
        ((equal (car x)(car y)) (equal (cdr x) (cdr y)))
        ((quote t) (quote f)))))

(label mc.subst (lambda (x y z)
    (mc.cond
        ((equal y z) (x))
        ((atom z) (z))
        ((quote t) (cons (subst x y (car z))(subst x y (cdr z)))))))

(label mc.append (lambda (x y)
    (mc.cond 
        ((null x) (y))
        ((quote t) (cons (car x)) (append (cdr x) y)))))

(label mc.member (lambda (x y)
    (mc.cond ((null y) (quote f))
    ((equal x (car y)) (quote t))
    ((quote t) (member x (cdr y))))))

(label mc.pairlis (lambda (x  y  a)
    (mc.cond ((null x) (a)) ((quote t) (cons (cons (car x)(car y))
    (pairlis (cdr x)(cdr y) a)))))

(label mc.assoc (lambda (x a)
    (mc.cond ((equal (caar a) x) (car a)) ((quote t) (assoc x (cdr a))))))

(label mc.sub2 (lambda (a z)
    (mc.cond ((null a) (z)) (((eq (caar a) z)) (cdar a)) ((quote t) (
    sub2 (cdr a) z)))))

(label mc.sublis (lambda (a y)
    (mc.cond ((atom y) (sub2 a y)) ((quote t) (cons (sublis a (car y))))
    (sublis a (cdr y)))))

(label mc.evalquote (lambda (fn x)
    (apply fn x nil)))

(label mc.apply (lambda (fn x a)
    (mc.cond ((atom fn) (
        (mc.cond
            ((eq fn car) (caar x))
            ((eq fn cdr) (cdar x))
            ((eq fn cons) (cons (car x)(cadr x)))
            ((eq fn atom) (atom (car x)))
            ((eq fn eq) (eq (car x)(cadr x)))
            ((quote t) (apply (eval (fn a)x a))))))
        ((eq (car fn) lambda) (eval (caddr fn) (parlis (cadr fn) x a)))
        ((eq (car fn) label) (apply (caddr (fn)x cons (cons (cadr (fn)))
            (caddr fn))a)))))

(label mc.eval (lambda (e a)
    (mc.cond
        ((atom e) (cdr (assoc e a)))
        ((atom (car e)) (mc.cond
            ((eq (car e) quote) (cadr e))
            ((eq (car e) cond) (evcon (cdr e) a))
            ((quote t) (apply (car e) (evlis (cdr e) a) a))))
        ((quote t) (apply (car e) (evlis (cdr e) a) a))))))

(label mc.evcon (lambda (c a)
    (mc.cond 
        ((eval (caar c) a) (eval (cadar c) a))
        ((quote t) (evcon (cdr c) a)))))

(label mc.evlis (lambda (m a)
    (mc.cond
        ((null m) (nil))
        ((quote t) (cons (eval (car m) a) (evlis (cdr m) a)))))))
```
