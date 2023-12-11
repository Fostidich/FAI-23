Some problems don't demand a path to the solution; instead, they solely prioritize finding the solution itself.
# Local search
A local search algorithm operates by searching from a state to the neighbors, without keeping track of the path.<br>
These searches can also solve optimization problems, in which the aim is to find the best solution according to an objective function.
## Hill-climbing search
The hill-climbing search algorithm moves, on each iteration, to the neighboring state with the highest value. It terminates when it reaches a peak where no neighbor has a higher value.<br>
This search is a greedy local search, since it chooses the next state without thinking ahead about where to go after.<br>
Hill-climbing can get stuck if it finds a non-solution state that has no neighbor with higher value. This can happen in case of 
- local maxima, that are non-global peaks with higher value than all neighbors,
- ridges, where many local maxima are found in a sequence,
- plateaus, flat areas in the state-space landscape.

Optimizations can be acquired if we allow sideways moves. The hope is that, upon reaching a plateau, it may actually be a shoulder (a flat plateau with available uphills) from which we can proceed. If we do not apply a limit to the number of sideways moves, the algorithm can wander on the plateau forever.<br>
Many variants of the hill-climbing search are found in the "stochastic hill-climbing", "first-choice hill-climbing" and "random-restart hill-climbing".