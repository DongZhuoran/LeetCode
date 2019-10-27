# Special Topic: Segment Tree

## Code Template
- Template 1
```
class SegmentTreeNode {
        int start, end;
        SegmentTreeNode left, right;
        int sum;
        public SegmentTreeNode(int start, int end, int sum) {
            this.start = start;
            this.end = end;
            this.left = null;
            this.right = null;
            this.sum = sum;
        }
    }
    
    SegmentTreeNode root = null;
    int[] nums;
    
    private SegmentTreeNode buildTree(int start, int end) {
        if (start > end) return null;
        if (start == end) return new SegmentTreeNode(start, end, nums[start]);
        
        int mid = (start + end) / 2;
        SegmentTreeNode left = buildTree(start, mid);
        SegmentTreeNode right = buildTree(mid + 1, end);
        SegmentTreeNode root = new SegmentTreeNode(start, end, left.sum + right.sum);
        root.left = left;
        root.right = right;
        return root;
    }
    
    private void update(SegmentTreeNode root, int i, int val) {
        if (root.start == root.end) {
            root.sum = val;
            return;
        }
        int mid = (root.start + root.end) / 2;
        if (i <= mid)
            update(root.left, i, val);
        else
            update(root.right, i, val);
        root.sum = root.left.sum + root.right.sum;
    }
    
    private int sumRange(SegmentTreeNode root, int i, int j) {
        if (root.start == i && root.end == j) return root.sum;
        
        int mid = (root.start + root.end) / 2;
        if (j <= mid)
            return sumRange(root.left, i, j);
        else if (i > mid)
            return sumRange(root.right, i, j);
        else
            return sumRange(root.left, i, mid) + sumRange(root.right, mid + 1, j);
    }
```

- Template 2
```
class NumArray {
    private int[] tree;
    private int N;

    public NumArray(int[] nums) {
        if (nums.length > 0) {
            N = nums.length;
            tree = new int[N * 2];
            buildTree(nums);
        }
    }
    
    private void buildTree(int[] nums) {
        // Build leaves.
        for (int i = 0; i < N; ++i) {
            tree[N + i] = nums[i];
        }
        // Build parents.
        for (int i = N - 1; i > 0; --i) {
            tree[i] = tree[i << 1] + tree[i << 1 | 1];
        }
    }
    
    public void update(int i, int val) {
        i = i + N;
        tree[i] = val;
        for (int k = i; k > 1; k >>= 1) {
            tree[k >> 1] = tree[k] + tree[k^1];
        }
    }
    
    public int sumRange(int i, int j) {
        int res = 0;
        for (i += N, j += N; i <= j; i >>= 1, j >>= 1) {
            if ((i & 1) == 1) {
                res += tree[i++];
            }
            if ((j & 1) == 0) {
                res += tree[j--];
            }
        }
        return res;
    }
}
```

## Complexity Analysis
Time Complexities:
- Tree Construction : O( n )
- Query in Range : O( Log n )
- Updating an element : O( Log n ).

## Related Problems
- <a href='https://github.com/DongZhuoran/LeetCode/blob/master/problems/307.%20Range%20Sum%20Query%20-%20Mutable.md'>307. Range Sum Query - Mutable</a>
