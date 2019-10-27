# Special Topic: Fenwick Tree/Binary Indexed Tree

## Code Template
```
  private static int lowbit(int x) { return x & (-x); }
  
  class FenwickTree {    
    private int[] sums;    
    
    public FenwickTree(int n) {
      sums = new int[n + 1];
    }
 
    public void update(int i, int delta) {    
      while (i < sums.length) {
          sums[i] += delta;
          i += lowbit(i);
      }
    }
 
    public int query(int i) {       
      int sum = 0;
      while (i > 0) {
          sum += sums[i];
          i -= lowbit(i);
      }
      return sum;
    }    
  };

```

## Related Problems
- <a href='https://leetcode.com/problems/range-sum-query-2d-mutable/'>308. Range Sum Query 2D - Mutable</a>

## Reference
- https://www.youtube.com/watch?v=WbafSgetDDk
