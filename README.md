Kth smallest element in a row-column wise sorted matrix in Java
Here, in this page we will discuss the program to find Kth smallest element in a row-column wise sorted matrix in Java programming language. We will discuss different approaches in this page.

Kth smallest element in a row-column wise sorted matrix in Java
Method Discussed :
Method 1 : Using Heap
Method 2 : Using  Binary Search
Let’s discuss them one by one in brief,

Method 1:
In this method we will check for every element that, if it is present in other rows or not.

Time and Space Complexity :
Time complexity: O(N*M)
Space complexity: O(1)
Method 1 : Code in Java
Run
class Main{
    static class HeapNode
    {
        
        int val;
        int r;
        int c;
         
        HeapNode(int val, int r, int c)
        {
            this.val = val;
            this.c = c;
            this.r = r;
        }
    }
    
    static void minHeapify(HeapNode harr[], int i, int heap_size)
    {
        int l = 2 * i + 1;
        int r = 2 * i + 2;
        int min = i;
         
        if(l < heap_size && r<heap_size && harr[l].val < harr[i].val && harr[r].val < harr[i].val){
                HeapNode temp=harr[r];
                harr[r]=harr[i];
                harr[i]=harr[l];
                harr[l]=temp;
                minHeapify(harr ,l,heap_size);
                minHeapify(harr ,r,heap_size);
            }
              if (l < heap_size  && harr[l].val < harr[i].val){
                HeapNode temp=harr[i];           
                harr[i]=harr[l];
                harr[l]=temp;
                minHeapify(harr ,l,heap_size);
            }
    }
    
    public static int kthSmallest(int[][] mat,int n, int k)
    {
        
        if (k < 0 && k >= n * n)
            return Integer.MAX_VALUE;
        
        HeapNode harr[] = new HeapNode[n];
         
        for(int i = 0; i < n; i++)
        {
            harr[i] = new HeapNode(mat[0][i], 0, i);
        }
         
        HeapNode hr = new HeapNode(0, 0, 0);
         
        for(int i = 1; i <= k; i++)
        {
           
            hr = harr[0];
            
            int nextVal = hr.r < n - 1 ? mat[hr.r + 1][hr.c] : Integer.MAX_VALUE;
            
            harr[0] = new HeapNode(nextVal, hr.r + 1, hr.c);
            
            minHeapify(harr, 0, n);
        }
    
        return hr.val;
    }
     
    // Driver code
    public static void main(String args[])
    {
        int mat[][] = { { 10, 20, 30, 40 },
                        { 15, 25, 35, 45 },
                        { 25, 29, 37, 48 },
                        { 32, 33, 39, 50 } };
                         
        int res = kthSmallest(mat, 4, 6);
         
        System.out.print("6th smallest element is "+ res);
    }
}
Output :
6th smallest element is 29
Method 2:
This approach uses binary search to iterate over possible solutions. We know that 

answer >= mat[0][0]
answer <= mat[N-1][N-1]
So we do a binary search on this range and in each  iteration determine the no of elements greater than or equal to our current middle element. The elements greater than or equal to current element can be found in O( n logn ) time using binary search.

Method 2 : Code in Java
Run
class Main {

  static int getElementsGreaterThanOrEqual(int num, int n, int mat[][])
  {
    int ans = 0;
 
    for (int i = 0; i < n; i++)
    {
      if (mat[i][0] > num) {
        return ans;
      }
       
      if (mat[i][n - 1] <= num) {
        ans += n;
        continue;
      }
    
      int greaterThan = 0;
      for (int jump = n / 2; jump >= 1; jump /= 2) {
        while (greaterThan + jump < n &&
               mat[i][greaterThan + jump] <= num) {
          greaterThan += jump;
        }
      }
 
      ans += greaterThan + 1;
    }
    return ans;
  }
 
  static int kthSmallest(int mat[][], int n, int k)
  {
    int l = mat[0][0], r = mat[n - 1][n - 1];
 
    while (l <= r) {
      int mid = l + (r - l) / 2;
      int greaterThanOrEqualMid = getElementsGreaterThanOrEqual(mid, n, mat);
 
      if (greaterThanOrEqualMid >= k)
        r = mid - 1;
      else
        l = mid + 1;
    }
    return l;
  }
 
  public static void main(String args[]) {
    int mat[][] = {
      { 10, 20, 30, 40 },
      { 15, 25, 35, 45 },
      { 25, 29, 37, 48 },
      { 32, 33, 39, 50 },
    };
    System.out.println("6th smallest element is " + kthSmallest(mat, 4, 6));
  }
 
}
Output :
6th smallest element is 29
