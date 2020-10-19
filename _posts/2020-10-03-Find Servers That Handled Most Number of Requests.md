---
title: Find Servers That Handled Most Number of Requests
tags: [LeetCode]
---

[1606. Find Servers That Handled Most Number of Requests](https://leetcode.com/problems/find-servers-that-handled-most-number-of-requests/)
You have k servers numbered from 0 to k-1 that are being used to handle multiple requests simultaneously. Each server has infinite computational capacity but cannot handle more than one request at a time. The requests are assigned to servers according to a specific algorithm:

The ith (0-indexed) request arrives.  
If all servers are busy, the request is dropped (not handled at all).  
If the (i % k)th server is available, assign the request to that server.  
Otherwise, assign the request to the next available server (wrapping around the list of servers and starting from 0 if necessary). For example, if the ith server is busy, try to assign the request to the (i+1)th server, then the (i+2)th server, and so on.  
You are given a strictly increasing array arrival of positive integers, where arrival[i] represents the arrival time of the ith request, and another array load, where load[i] represents the load of the ith request (the time it takes to complete). Your goal is to find the busiest server(s). A server is considered busiest if it handled the most number of requests successfully among all the servers.  

Return a list containing the IDs (0-indexed) of the busiest server(s). You may return the IDs in any order.

#### Solution  
1. Notice that we need to take care of both ids and times, we can consider to use multiple heaps to optimize.

1. When it comes to loop, we can divide into `before` and `after`.

1. Use 3 heaps in total.

```python
def busiestServers(self, k, arrival, load):
    """
    :type k: int
    :type arrival: List[int]
    :type load: List[int]
    :rtype: List[int]
    """
    handled_tasks = [0] * k
    server_free_time = [(0, i) for i in range(k)]
    heapq.heapify(server_free_time)
    
    free_servers_after, free_servers_before = [], []         
    for i, a in enumerate(arrival):
        target_server_id = i % k
        
        # loop back
        if target_server_id == 0:
            free_servers_after = free_servers_before
            free_servers_before = []
         
        # use heap to store IDs
        while server_free_time and server_free_time[0][0] <= a:
            tmp = heapq.heappop(server_free_time)
            if tmp[1] >= target_server_id:
                heapq.heappush(free_servers_after, tmp[1])
            else:
                heapq.heappush(free_servers_before, tmp[1])
        
        queue = free_servers_after if free_servers_after else free_servers_before
        if not queue:
            continue
        
        using_server = heapq.heappop(queue)
        handled_tasks[using_server] += 1
        heapq.heappush(server_free_time, (a + load[i], using_server))
        
    max_handled = max(handled_tasks)
    return [i for i, num in enumerate(handled_tasks) if num == max_handled]
```