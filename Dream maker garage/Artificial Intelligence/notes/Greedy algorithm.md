Greedy search is not optimal, it can mislead the result to a longer path. The reason is it forgot the previous step cost from initial state this current state. 



```python
def greedy(items):
	length = len(items)
	items_list = []
	items_list.append(items[length-1])
	weights = item[length-1][1][1]
	for i in range(length-1, -1, -1):
		if item[i][1][1] + weight <= max_weight:
			items_list.append(item[i])
			weights += item[i][1][1]
	return items_list
```
