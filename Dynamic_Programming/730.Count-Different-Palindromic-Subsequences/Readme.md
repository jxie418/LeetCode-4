###  730.Count-Different-Palindromic-Subsequences

我们定义dp[i][j]为区间[i,j]内的不同的回文序列的个数。那么怎么区分不同的回文序列呢？可以采用层层剥离的思想，也就是分别考虑回文序列的最外层配对是'a','b','c','d'的四种情况.按照这种思路去分类的话，不同类之间的回文序列肯定是不会有重复的。也就是说dp[i][j]，可以拆分为：[i,j]区间里以字符k为最外层配对的回文数的和，其中k分别为'a','b','c','d'.

dp[i][j]里以'a'为最外层配对的回文数怎么找呢？显然我们要把这个最外层配对找到，这是一定可以确定下来的。如果我们要找到i后面第一个为'a'的位置m，以及j前面第一个为'a'的位置n，并且m<n，显然就有递推的关系dp[i][j] += dp[m+1][n-1]，可以这么理解：拔掉了最外层的配对'a'，里面还有多少个回文数，就说明dp[i][j]有多少以'a'为最外层配对的回文数。同理我们可以找其他最外层配对的情况，也就是'b','c','d'。所以递推关系是：
```cpp
for (int k=0; k<4; k++)
{
  if (next[i][k]<prev[j][k])  //小于号保证了这个配对一定是成双的
    dp[i][j] += dp[next[i][k]+1][prev[j][k]-1];
 }
```
其中```next[i][k]```定义了i位置之后第一个为字符k的位置（可以是i本身），```prev[j][k]```定义了j位置之前第一个为字符k的位置（可以是j本身）。这两个next和prev数组可以预处理得到。

上面的递推关系还有一个疏漏。如果[i,j]区间内没有以字符k的配对，但是有单独的一个字符k存在。这种情况也需要加进dp[i][j]的统计中去。所以完整的递推关系：
```cpp
for (int k=0; k<4; k++)
{
  if (next[i][k]<prev[j][k])  //小于号保证了这个配对一定是成双的
    dp[i][j] += dp[next[i][k]+1][prev[j][k]-1];
  if (next[i][k]<=j)
    dp[i][j] += 1;
 }
```
以上的语句再加上最外层对于i,j的大循环，即是完整的代码。最终的答案是dp[0][N-1]。

本题其实也可以通过定义dp[i][j][k]三维状态数组来实现这个算法，最终输出是```sum{k=0,1,2,3} dp[0][N-1][k]```。但是空间上可能无法通过。