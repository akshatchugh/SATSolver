A program to give a valid solution to a satisfiability problem if there exists one.

How to run:-

	1) Run satSolver.rkt
	
	2) There are two ways to give the command to check if there exists a solution or not:-
	(Here we will consider the problem (a1 || a2) && (~a3 || a2) && (a4 || ~a1 || a3) as an example)
		a) Type (dpll (parseExp (list '(1 2) '(-3 2) '(4 -1 3))))
		b) Type (dpll (And (Or (Var 1) (Var 2)) (Or (Not (Var 3)) (Var 2)) (Or (Var 4) (Not (Var 1)) (Var 3))))
	
	3) This will output #t or #f depending on if it is satisfiable. (returns #t for our example)
	
	4) If it returns #t, type `assign` and it will provide a solution. If some variables are not assigned any value,
	then their value can be considered to be either true or false. (gives 2->#t and 4->#t for our example)  
