---
title: Throne Inheritance
tags: [LeetCode]
---

[1600. Throne Inheritance](https://leetcode.com/problems/throne-inheritance/)
#### Solution  
```python
def __init__(self, kingName):
    """
    :type kingName: str
    """
    self.king = kingName
    self.relation = collections.defaultdict(list)
    self.parent = {kingName: None}
    self.deadp = set()

def birth(self, parentName, childName):
    """
    :type parentName: str
    :type childName: str
    :rtype: None
    """
    self.relation[parentName].append(childName)
    self.parent[childName] = parentName

def death(self, name):
    """
    :type name: str
    :rtype: None
    """
    self.deadp.add(name)
    

def getInheritanceOrder(self):
    """
    :rtype: List[str]
    """
    res, visited = [], set()
    
    if self.king not in self.deadp:
        res.append(self.king)
        
    curr = self.king
    while curr:
        has_child = False
        for p in self.relation[curr]:
            if p in visited:
                continue
            else:
                visited.add(p)
                if p not in self.deadp:
                    res.append(p)
                curr = p
                has_child = True
                break
        if has_child:
            continue
        
        curr = self.parent[curr]
    return res
```