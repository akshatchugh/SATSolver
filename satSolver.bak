#lang racket

(require "utilities.rkt")

(define assign #hash())

; Fill in your code here. Should finally define a function
; called dpll which returns true or false. Should additionally
; store the satisfying assignment in the variable assign.

(define (parsed-to-list-sub parse-sub)
  (cond [(Or? parse-sub) (let ([var1 (Or-x parse-sub)]) (cons (if (Var? var1) (Var-lit var1) (- (Var-lit (Not-e var1)))) (parsed-to-list-sub (Or-y parse-sub))))]
        [(Var? parse-sub) (list (Var-lit parse-sub))]
        [(Not? parse-sub) (list (- (Var-lit (Not-e parse-sub))))]))

(define (parsed-to-list parsed)
  (cond [(And? parsed) (cons (parsed-to-list-sub (And-x parsed)) (parsed-to-list (And-y parsed)))]
        [else (list (parsed-to-list-sub parsed))]))                                   

(define (removel formula value)
  (cond [(null? formula) '()]
        [(member value (car formula)) (removel (cdr formula) value)]
        [else (cons (remove* (list (- value)) (car formula)) (removel (cdr formula) value))]))


(define (unit-propag formula-o)
  (define (unit-propag-h formula)
    (cond [(null? formula) formula-o]
          [(= 1 (length (car formula))) (let ([val (caar formula)])
                                          (begin (set! assign (dict-set assign (abs val) (if (> val 0) #t #f)))
                                                 (unit-propag (removel formula-o val))))]
          [else (unit-propag-h (cdr formula))]))
  (unit-propag-h formula-o))

(define (literal-elim formula)
  (define tot (append* formula))
  (define (literal-elim-h full)
    (cond [(null? full) formula]
          [(member (- (car full)) full) (literal-elim-h (remove* (list (car full) (- (car full))) (cdr full)))]
          [else (begin (set! assign (dict-set assign (abs (car full)) (if (> (car full) 0) #t #f)))
                       (literal-elim (removel formula (car full))))]))
  (literal-elim-h tot))
        

(define (dpll parsed)
  (define formula (parsed-to-list parsed))
  (set! assign #hash())
  (define (dpll-h formula)
    (define formula1 (unit-propag formula))
    (define formula2 (literal-elim formula1))
    (cond [(null? formula2) #t]
          [(member '() formula2) (begin (set! assign #hash()) #f)]
          [else (begin (set! assign (dict-set assign (abs (caar formula2)) (if (> (caar formula2) 0) #t #f)))
                       (define assign-m assign)
                       (if (dpll-h (removel formula2 (caar formula2))) #t
                           (begin (set! assign (dict-set assign-m (abs (caar formula2)) (if (> (caar formula2) 0) #f #t)))
                                  (if (dpll-h (removel formula2 (- (caar formula2)))) #t  #f))))]))
  (dpll-h formula))
