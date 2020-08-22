---
title: Task Scheduler
tags: [LeetCode]
---

[621. Task Scheduler](https://leetcode.com/problems/task-scheduler/)
#### Solution  
1. The tasks which appear more times is the bottle neck. 

1. We store the number of times each task appeared. Use heapq to sort it.

1. Use the top tasks to fill in the time units we need between two same tasks.

1. When there is not enough tasks, None is appended.

1. We need to update the number of times of each tasks, to avoid affecting step 3, we keep all tasks that need to be updated in a list,
and update them in the end of this loop.
 
```python
def leastInterval(self, tasks, n):
    """
    :type tasks: List[str]
    :type n: int
    :rtype: int
    """
    # Get the number of chars
    times = [(0, '')] * 26
    for task in tasks:
        times[ord(task) - ord('A')] = (times[ord(task) - ord('A')][0] - 1,
                                       task)
    heapq.heapify(times) 
    res = []
    while len(times) != 0:
        top = heapq.heappop(times)
        top_time = top[0]
        top_char = top[1]
        while top_time != 0:
            top_time += 1
            res.append(top_char)
            skip = 0
            temp = []
            while skip != n:
                if len(times) != 0:
                    next_t = heapq.heappop(times)
                    if next_t[0] < 0:
                        temp.append(next_t)
                        res.append(next_t[1])
                        skip += 1
                        continue
                res += [None] * (n - skip)
                skip += n - skip
            for tmp in temp:
                heapq.heappush(times, (tmp[0] + 1, tmp[1]))

    tail = 0
    for i in range(len(res)-1, -1, -1):
        if not res[i]:
            tail += 1
        else:
            break
    return len(res) - tail
```
#### Solution - O(N)
[Solution](https://leetcode.com/problems/task-scheduler/discuss/476819/Python-O(-n-)-sol.-based-on-dictionary-95%2B-With-explanation)

1. Build a dictionary for tasks

1. Make `max_occ - 1` groups, groups size = n+1. Fill each group with uniform iterleaving as even as possible

1. At last, execute for the last time of max_occ jobs

```python
def leastInterval(self, tasks: List[str], n: int) -> int:
        
    if n == 0:
        return len(tasks)

    task_occ_dict = Counter( tasks )
    
    # max occurrence among tasks
    max_occ = max( task_occ_dict.values() )
    
    # number of tasks with max occurrence
    number_of_taks_of_max_occ = sum( ( 1 for task, occ in task_occ_dict.items() if occ == max_occ ) )
    
    # Make (max_occ-1) groups, each groups size is (n+1) to meet the requirement of cooling
    # Fill each group with uniform iterleaving as even as possible
    
    # At last, execute for the last time of max_occ jobs
    intervl_for_schedule = ( max_occ-1 )*( n+1 ) + number_of_taks_of_max_occ
    
    # Minimal length is original length on best case.
    # Otherswise, it need some cooling intervals in the middle
    return max( len(tasks), intervl_for_schedule)
```