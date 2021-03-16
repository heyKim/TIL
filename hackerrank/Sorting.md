# Sorting

## Mark and Toys
* DIFFICULTY: EASY
```python
def maximumToys(prices, k):
    sorted_item= sorted(prices)
    max_count=0
    sum_item=0
    for i in sorted_item:
        sum_item+=i
        max_count+=1
        if sum_item>k:
            return max_count-1
```