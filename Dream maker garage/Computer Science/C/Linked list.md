A linked list is a sequence of data structures (nodes) which are connected together via links. Each link contains a connection to another link. Nodes can be added, deleted or moved within the list. 
Each node contains at least one value and one pointer. The pointer always points to the next member of the list. If the pointer is `NULL`, then it is the last node in the list. 

There are three types of linked list. 
![[截圖 2025-01-09 下午4.08.43.png]]

The simple linked list node can be created by
```c
struct Node{
	int data; // node value
	struct Node* next; // pointer to next node
}
```

A simple linked list has the following structure. 
![[截圖 2025-01-09 下午4.11.47.png]]

A doubly linked list has the following structure. 
![[截圖 2025-01-09 下午4.12.18.png]]
 
 A circular linked list has the following structure. ![[截圖 2025-01-09 下午4.13.49.png]]

##### Advantages 
- Values can be added, inserted or deleted from the list easily
- If a value is added or deleted in the middle of an array, all proceeding values must be moved to keep all data contiguous
- The memory size of the linked list is maintained dynamically
##### Disadvantages
- Extra memory space for pointer(s) is required for each node in the list
- Random access is not permitted and elements must be accessed sequentially

##### Scenarios
1. In a queued list where insert priority items is wished (e.g., a message codes from serial communications that required priority)
2. The number of elements in a list is unknown (e.g., ADC measurements that wished to add and move in a queue due to priorities and sample size changes)
3. Can eliminate items in the queue without affecting the priority of the remaining list (e.,g., some sensor readings are not required in certain parts and can be easily removed without affecting the order of the queue)
4. Store adjacent vertices for graphing applications and change or delete any value
5. Manipulate polynomials by storing constants in the nodes of linked list
6. Image viewer where previous and next images are linked
7. Music player where songs are linked for a playlist

#### Memory management
See also [[Memory management]]
##### Memory heap
A heap is an area of pre-served volatile memory that a program can use to store data in some variable amount that won't be known until the program is running. 
Generally the heap size must be set before allocating memory with a size that could change during run time. Hence the **dynamic allocated memory** is used. 

###### Malloc
**Malloc()** is a C library function that allocates the requested memory and returns a pointer to it. It dynamically allocate memory during run time. 
```c
*malloc(size_t size); 
```

Example usage:
```c
newNode = (struct Node*)malloc(sizeof(struct Node)); 
// Dynamically allocate memory the size of Node and name the pointer to that location newNode
```

###### Free 
**Free()** is another C library that deallocates memory that was previously allocated by `malloc()`. The location that is freed is pointed to by \*pointer
```c
free(*pointer); 
```

Example usage:
```c
free(newPointer); 
// Dynamically deallocates the memory that was previously allocated at the location pointed to by the newPointer. 
```


#### Implementation
```c
struct Node{
	int val;
	struct Node *nextPtr; 
}; 

// example of two linked item
struct Node *currentPtr; 
struct Node *nextPtr; 

// define a pointer to the first item in the list
struct Node* headNode = NULL; 
// allocate memory for the first item and save it as the "head" value; 
headNode = (struct Node*)malloc(sizeif(struct Node)); 
```

##### Common issues & cautions
- Must always check for a NULL return on the `malloc()` call and handle the condition (a must!)
- Uncertainty with knowing how much memory remains for new nodes
- Inadvertently allows for another dynamic memory allocation for a totally different use using the same heap (cancels any count that was implemented to determine the remaining memory)
- Much more flash memory required for `malloc()` and `free()` (usually much more, an example: 4.5k more code for dynamic memory allocation)

##### Example
Adding and deletion of a node, using dynamic allocated memory
```c
struct Node
{
	int val;
	struct Node *nextPtr;
};  

// Create two pointers that will be used to search the linked list and add or delete nodes
struct Node *currentPointer;
struct Node *previousPointer;

// create a variable that will be used as a flag to denote that the search found a node that we were looking for
uint8_t nodeFound;

void main(void){
// Create structure pointer to four instanced of nodes in the linked list. The only required nodes to be created are "headNode" to signify the first node in the linked list and "newNode" that will be used to manipulate the items in the list. The "secondNode" and "thirdNode" are created just for demonstration purposes to allow for some initial created nodes and make it easier to understand the linked list concept.

	struct Node* headNode = NULL;
	struct Node* secondNode = NULL;
	struct Node* thirdNode = NULL;
	struct Node* newNode = NULL;

  

// allocate 3 nodes in the heap

	headNode = (struct Node*)malloc(sizeof(struct Node));
	secondNode = (struct Node*)malloc(sizeof(struct Node));
	thirdNode = (struct Node*)malloc(sizeof(struct Node));

  

// initialise all nodes
	headNode -> val = 2;
	headNode -> nextPtr = secondNode;
	secondNode -> val = 3;
	secondNode -> nextPtr = thirdNode;
	thirdNode -> val = 5;
	thirdNode -> nextPtr = NULL;


// create a new node with a value of 4. It is not placed in the list yet. 
	newNode = (struct Node*)malloc(sizeof(struct Node));
	newNode -> val = 4;

// add the node in numerical order in the linked list. Initialize the node pointers.

	currentPointer = headNode;
	previousPointer = headNode;
	nodeFound = 0;

  
  

// step through the linked list and search for the position to place the newNode value of 4 so that the list stays in numerical order.

// zthe algorithm looks at the value in the location of the currentPointer and compares it to the value in newNode. If it is less than the value, the iteration of this search stops. previousPointer is set equal to currentPointer and currentPointer is set equal to the location pointed to by the nextPtr value in that location. So this algorithm just steps through the list. If the currentPointer value is greater than the newNode value, then the newNode value will be placed before the currentPointer. Since previousPointer points to the location before the currentPointer, the nextPtr value of the previousPointer is set equal to the newNode pointer. The newNode pointer is set to point to the currentPointer location.

// For every iteration of the loop, the previousPointer is set equal to the currentPointer and the currentPointer is incremented. So there are pointers to the current node and previous node at all times.

// If the value to be placed is less than the first location in the list, the newNode pointer is set to be the headPointer. If the value to be placed is greater than the last location, this search will fall through without placing the node in the list. nodeFound is used to signify that a node was placed. If this value is 0, then the previousPointer is set to point to the newNode and the newNode pointer is set to NULL.

	while(previousPointer -> nextPtr != NULL){
		if((currentPointer -> val) > (newNode -> val)){
			nodeFound = 1;
			newNode -> nextPtr = currentPointer;
			if(currentPointer != headNode){}
				previousPointer -> nextPtr = newNode;

			else{
				headNode = newNode; 
			}
		break;
		}
	previousPointer = currentPointer;
	currentPointer = currentPointer -> nextPtr;
	}

// Check if a node was found. If not, put the value at the end of the list
	if(!nodeFound){
		previousPointer -> nextPtr = newNode;
		newNode -> nextPtr = NULL; 
		nodeFound = 0;
	}
	nodeFound = 0;

// Delete the node with a value of 3.
//This search is very similar to the previous search. When the node value is found, the previousPointer is set to point to the value pointed by currentPointer. Then the free() function call is used to deallocate the memory that currentPointer was pointing to. If the node to be deleted is the head node, then the headNode is set equal to the location pointed to by the current head node. If the location is pointing to NULL, then it must be the last node, so the previousPointer is set to point to NULL since it will now be the last node.

	currentPointer = headNode;
	previousPointer = headNode;
	
	while(previousPointer -> nextPtr != NULL){
		if((currentPointer -> val) == 3){
			if(currentPointer == headNode){
				headNode = currentPointer -> nextPtr;
			}
			else if(currentPointer -> nextPtr == NULL){
				previousPointer -> nextPtr = NULL;
			}

			else{
				previousPointer -> nextPtr = currentPointer -> nextPtr;
			}

			free(currentPointer);
			break;
		}
	previousPointer = currentPointer;
	currentPointer = currentPointer -> nextPtr;

	}
	while(1);
}
```

