# Simulated Annealing

Simulated annealing (SA) is a probabilistic technique for approximating the global optimum of a given function. Specifically, it is a metaheuristic to approximate global optimization in a large search space. It is often used when the search space is discrete (e.g., all tours that visit a given set of cities). For problems where finding an approximate global optimum is more important than finding a precise local optimum in a fixed amount of time, simulated annealing may be preferable to alternatives such as gradient descent.

This work implementing simulated annealing to solve the Traveling Salesman Problem (TSP) between US state capitals. Briefly, the TSP is an optimization problem that seeks to find the shortest path passing through every city exactly once. In our example the TSP path is defined to start and end in the same city (so the path is a closed loop).

**function** SIMULATED-ANNEALING(_problem_,_schedule_) __returns__ a solution state 
&emsp;__inputs__: _problem_, a problem  
&emsp;&emsp;&emsp;&emsp;_schedule_, a mapping from time to "temperature"  

&emsp;_current_ &larr; MAKE\-NODE(_problem_.INITIAL\-STATE)  
&emsp;__for__ _t_ = 1 __to__ &infin;  __do__  
&emsp;&emsp;&emsp;_T_ &larr; _schedule(t)_  
&emsp;&emsp;&emsp;__if__ _T_ = 0 __then return__ _current_  
&emsp;&emsp;&emsp;_next_ &larr; a randomly selected successor of _current_  
&emsp;&emsp;&emsp;_&Delta;E_ &larr; _next_.VALUE - _current_.VALUE  
&emsp;&emsp;&emsp;__if__ _&Delta;E_ > 0 __then__ _current_ &larr; _next_  
&emsp;&emsp;&emsp;__else__ _current_ &larr; _next_ only with probability e<sup>_&Delta;E_/_T_</sup>

# Genetic Algorithm

__function__ GENETIC-ALGORITHM(_population_, FITNESS\-FN) __returns__ an individual  
&emsp;__inputs__: _population_, the initial random population of individuals  
&emsp;&emsp;&emsp;&emsp;FITNESS\-FN, a function that measures the fitness of an individual  

&emsp;__repeat__  
&emsp;&emsp;&emsp;_population_ &larr; [MUTATE(RECOMBINE(SELECT(2, _population_, FITNESS\-FN)))  
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;__for__ _i_ __in__ _population_]  
&emsp;__until__ some individual is fit enough, or enough time has elapsed  
&emsp;__return__ the best individual in _population_, according to FITNESS\-FN  

------

__function__ SELECT(ρ, _population_, FITNESS\-FN) __returns__ a set of ρ individuals  
&emsp;_selection_ &larr; a uniform random sample of 2 * ρ individuals from _population_  
&emsp;__return__ the top ρ individuals in _selection_, ranked by FITNESS\-FN  

------

__function__ RECOMBINE(_x_, _y_) __returns__ an individual  
&emsp;__inputs__: _x_,_y_, parent individuals  

&emsp;_n_ &larr; LENGTH(_x_)  
&emsp;_crossover_ &larr; random integer from 0 to _n_  
&emsp;__return__ APPEND(_x_\[0:_crossover_\], _y_\[_crossover_: _n_\])  

------

__function__ GENETIC-ALGORITHM(_population_,FITNESS\-FN) __returns__ an individual  
&emsp;__inputs__: _population_, a set of individuals  
&emsp;&emsp;&emsp;&emsp;FITNESS\-FN, a function that measures the fitness of an individual  

&emsp;__repeat__  
&emsp;&emsp;&emsp;_new\_population_ &larr; empty set  
&emsp;&emsp;&emsp;__for__ _i_ = 1 to SIZE(_population_) __do__  
&emsp;&emsp;&emsp;&emsp;&emsp;_x_ &larr; RANDOM-SELECTION(_population_,FITNESS\-FN)  
&emsp;&emsp;&emsp;&emsp;&emsp;_y_ &larr; RANDOM-SELECTION(_population_,FITNESS\-FN)  
&emsp;&emsp;&emsp;&emsp;&emsp;_child_ &larr; REPRODUCE(_x_,_y_)  
&emsp;&emsp;&emsp;&emsp;&emsp;__if__ (small random probability) __then__ _child_ &larr; MUTATE(_child_)  
&emsp;&emsp;&emsp;&emsp;&emsp;add _child_ to _new\_population_  
&emsp;&emsp;&emsp;_population_ &larr; _new\_population_  
&emsp;__until__ some individual is fit enough, or enough time has elapsed  
&emsp;__return__ the best individual in _population_, according to FITNESS\-FN  

------

__function__ REPRODUCE(_x_, _y_) __returns__ an individual  
&emsp;__inputs__: _x_,_y_, parent individuals  

&emsp;_n_ &larr; LENGTH(_x_); _c_ &larr; random number from 1 to _n_  
&emsp;__return__ APPEND(SUBSTRING(_x_, 1, _c_),SUBSTRING(_y_, _c_+1, _n_))  

-------

## Reference

1. [AIMA](http://aima.cs.berkeley.edu/)