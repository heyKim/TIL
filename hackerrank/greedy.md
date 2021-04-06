# Greedy

## Marc's Cakewalk
```python
def marcsCakewalk(calorie):
    sorted_calorie= sorted(calorie, reverse=True)
    sum=0
    for idx,n in enumerate(sorted_calorie):
        print(2**idx, n)
        sum+=2**idx*n
    return sum
```