---
title: Best Team With No Conflicts
tags: [LeetCode, Dynamic Programming]
---

[1626. Best Team With No Conflicts](https://leetcode.com/problems/best-team-with-no-conflicts/)
You are the manager of a basketball team. For the upcoming tournament, you want to choose the team with the highest overall score. The score of the team is the sum of scores of all the players in the team.

However, the basketball team is not allowed to have conflicts. A conflict exists if a younger player has a strictly higher score than an older player. A conflict does not occur between players of the same age.

Given two lists, scores and ages, where each scores[i] and ages[i] represents the score and age of the ith player, respectively, return the highest overall score of all possible basketball teams.

#### Solution - DP  
[Solution](https://leetcode.com/problems/best-team-with-no-conflicts/discuss/899475/Fairly-easy-DP)
1. Sort the players by their age in the descending order.

1. For any player i, we can choose any player from 0 to i-1 as long as that player has higher score than this i-th player.

1. `dp[i]` stores the maximum score that can be obtained when i-th player is included and all other players are between indices 0 and i-1.

```c++
class Solution {
public:
    int bestTeamScore(vector<int>& scores, vector<int>& ages) {
        vector<pair<int, int>> players;
        int n = scores.size();
        for (int i=0; i<n; i++) {
            players.push_back({ages[i], scores[i]});
        }
        sort(players.begin(), players.end(), greater<>());
        
        int ans = 0;
        vector<int> dp(n);
        for (int i=0; i<n; i++) {
            int score = players[i].second;
            dp[i] = score;
            for (int j=0; j<i; j++) {
                if (players[j].second >= players[i].second) { // age of j is certainly >= i, so only important part to check 
													          //  before we add i and j in the same team is the score.
                    dp[i] = max(dp[i], dp[j] + score);
                }
            }
            ans = max(ans, dp[i]);
        }
        return ans;
    }
};
```