![[state machine diagram.png]]

A **state machine** or **finite state machine (FSM)**, is one model for computation in which a machine can be in exactly **one of a finite number of states at a time**. 
The state machine **transitions** between states in response to **inputs**. 

A state machine can be fully defined by:
- list of states
- initial state
- the input conditions which trigger each transition

The state machine may perform a number of **actions** during a transition. 

##### Classification
Moore and Mealy machines are two different forms of FSM. 

###### **Moore machines**: 
The outputs are determined only by the current state.  

An example of a SR flip-flop state machine is shown below. 
![[截圖 2024-12-02 上午11.22.20.png]]
Notice that:
- a circle for each of the various states a circuit can be in
- labels in the circles to show the state number and the associated output for that state
- an arrow for each possible transition between states
- labels on the arrows to show the required conditions to transit between states

A state table representation of the state machine is:
![[截圖 2024-12-02 上午11.23.50.png]]

A Karnaugh map representation (state encoded as boolean value) of the state machine is: 
![[截圖 2024-12-02 上午11.26.55.png]]
We can then name the next state to be $Q^+$ and represent it as a function of current state and inputs $Q^+ = f(Q, S, R)$. 

Now the boolean function can be obtained from Karnaugh map grouping:
$$Q^+ = Q\overline{R}+S$$
This is referred as **characteristic equation**. 

###### **Mealy machines**: 
The outputs are determined by both the current state and the inputs. It is more general, as Moore is appropriate when output depends only on state. 

![[截圖 2024-12-02 上午11.30.27.png]]
Notice that: 
- State circles are labelled only with state names
- Outputs are written next to inputs on the arrows



##### Benefits of using state machines

- A state machine can break down a **complex procedure or task** into **simple, independent units**. 
- For documentation purpose, it clearly **define the intended behaviours** including action, outputs, etc. 
- With a finite amount of states, it’s easy to track which condition or transition caused some output.
- State machines are very maintainable; each state is independent so changes can be made to one state without affecting the behaviour of others.

##### Logic gate implementation
![[截圖 2024-12-06 上午11.49.44.png]]
A finite state machine has the diagram as above. It is expressed as the boolean algebra below:
![[截圖 2024-12-06 上午11.51.31.png]]a counter which counts 0, 2, 5 then 6. 

##### Code Implementations

![[State machine example]]

###### Enum

An enumerated type or `enum` in C allows you to declare a new type which has a finite number of defined values. 
The syntax is:
```c
enum identifier {
	VALUE0,
	VALUE1, 
	... 
	VALUEn
}
```

Just as with structs and unions, anonymous `enums` can be declared. Internally it is treated as integers, zero-based. But it should not be relied on. 

With typedef (see [[Type and memory manipulation]]), an example of an `enum` can be declared as:
```c
typedef enum {
    RED,
    GREEN,
    BLUE 
} colour;

colour my_colour = RED;

int main() {  
if (my_colour == RED) {
    printf("My colour is red.\n");
	}
}
```

The reason for using an `enum` type is that it is essentially a switch-case structure. This is useful in implementing a state machine with finite amount of states (`enum`). 

###### Switch-case statement

A switch-case statement is an alternative to using a large number of if-then-else statements. It has the declaration of:
```c
switch (expression) { 
	case VALUE0:
		// statements
		break; 
	case VALUE1:    
		// statements        
		break; 
	... 
	
	default:
		// statements
		break;
```



