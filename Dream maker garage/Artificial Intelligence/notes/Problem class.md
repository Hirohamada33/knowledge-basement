A code implementation of an abstract concept of a problem. 

A problem can be defined by the following components:

1. Initial state: the initial state of the problem
   `In(state)`
2. Actions: a set of actions for state s
   `Actions(In(s)) = {ActionA, ActionB, ActionC, ...}`
3. Transitions: the result of the action, or successor state 
   `Result(In(s), ActionA) = In(sA)`
4. Goal test: can be explicit set of states, or implicit property (such as `checkmate`)
   `{In(sucess_state)}`
5. Path cost: is the summation of step costs (sum of distances, number of actions executed, etc)
   `c(s, a, s')`
6. Heuristic (optional, only in [[A* search]]): the admissible heuristic values for priortising the search
   

A solution is a sequence of actions leading from initial state to goal state. 

Below is an example of a problem class definition
```python
class Problem(object):
	"""The abstract class for a formal problem. You should subclass
	this and implement the methods actions and result, and possibly
	__init__, goal_test, and path_cost. Then you will create instances
	of your Problem subclass and solve them with the various search functions."""
  

def __init__(self, initial, goal=None):
	"""The constructor specifies the initial state, and possibly a goal
	state, if there is a unique goal. Your subclass's constructor can add
	other arguments."""
	self.initial = initial
	self.goal = goal
  

def actions(self, state):
	"""Return the actions that can be executed in the given
	state. The result would typically be a list, but if there are
	many actions, consider yielding them one at a time in an
	iterator, rather than building them all at once."""
	raise NotImplementedError


def result(self, state, action):
	"""Return the state that results from executing the given
	action in the given state. The action must be one of
	self.actions(state)."""
	raise NotImplementedError


def goal_test(self, state):
	"""Return True if the state is a goal. The default method compares the
	state to self.goal, as specified in the constructor. Override this
	method if checking against a single self.goal is not enough."""
	return state == self.goal

  
def path_cost(self, c, state1, action, state2):
	"""Return the cost of a solution path that arrives at state2 from
	state1 via action, assuming cost c to get up to state1. If the problem
	is such that the path doesn't matter, this function will only look at
	state2. If the path does matter, it will consider c and maybe state1
	and action. The default method costs 1 for every step in the path."""
	return c + 1

  
def h(self, state):
	"""For optimization problems, each state has a heuristic value. Hill-climbing
	and related algorithms try to maximize this value."""
	raise NotImplementedError
```