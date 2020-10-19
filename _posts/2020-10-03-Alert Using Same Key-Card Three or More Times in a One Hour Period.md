---
title: Alert Using Same Key-Card Three or More Times in a One Hour Period
tags: [LeetCode]
---

[1604. Alert Using Same Key-Card Three or More Times in a One Hour Period](https://leetcode.com/problems/alert-using-same-key-card-three-or-more-times-in-a-one-hour-period/)
LeetCode company workers use key-cards to unlock office doors. Each time a worker uses their key-card, the security system saves the worker's name and the time when it was used. The system emits an alert if any worker uses the key-card three or more times in a one-hour period.

You are given a list of strings keyName and keyTime where [keyName[i], keyTime[i]] corresponds to a person's name and the time when their key-card was used in a single day.

Access times are given in the 24-hour time format "HH:MM", such as "23:51" and "09:49".

Return a list of unique worker names who received an alert for frequent keycard use. Sort the names in ascending order alphabetically.

Notice that "10:00" - "11:00" is considered to be within a one-hour period, while "22:51" - "23:52" is not considered to be within a one-hour period.

#### Solution - Sliding Window  
1. Use 4-byte integer to represent the time, sort the time.

1. Check if any time at `i` is within the period of time + 100 at `i-2`.

```python
def alertNames(self, keyName, keyTime):
    """
    :type keyName: List[str]
    :type keyTime: List[str]
    :rtype: List[str]
    """
    def toInt(time):
        return int(time[:2]) * 100 + int(time[3:])
    
    dic = collections.defaultdict(list)
    for name, time in zip(keyName, keyTime):
        dic[name].append(time)
        
    res = []
    for name in dic:
        dic[name] = sorted(dic[name], key=lambda x: toInt(x))
        for i, time in enumerate(dic[name][2:], 2):
            if toInt(dic[name][i]) - toInt(dic[name][i-2]) <= 100:
                res.append(name)
                break
    return sorted(res)
```
#### Solution - Deque  
1. Use 4-byte integer to represent the time.

1. Maintain a dequeue where all items in the queue is within the limited time period. 
When the length of queue > 3, append the user's name.

```python
def alertNames(self, keyName: List[str], keyTime: List[str]) -> List[str]:
    name_to_time = collections.defaultdict(list)
    for name, hour_minute in zip(keyName, keyTime):
        hour, minute = map(int, hour_minute.split(':'))
        time = hour * 60 + minute
        name_to_time[name].append(time)
    names = []    
    for name, time_list in name_to_time.items():
        time_list.sort()
        dq = collections.deque()
        for time in time_list:
            dq.append(time)
            if dq[-1] - dq[0] > 60:
                dq.popleft()
            if len(dq) >= 3:
                names.append(name)
                break
    return sorted(names)
```