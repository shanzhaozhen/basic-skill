## 6. 数据结构与算法

### 6.1 10 亿个数字里里面找最小的 10 个。



### 6.2 有 1 亿个数字，其中有 2 个是重复的，快速找到它，时间和空间要最优。



### 6.3 2 亿个随机生成的无序整数,找出中间大小的值。



### 6.4 给一个不知道长度的（可能很大）输入字符串，设计一种方案，将重复的字符排重。



### 6.5 遍历二叉树



### 6.6 有 3n+1 个数字，其中 3n 个中是重复的，只有 1 个是不重复的，怎么找出来。



### 6.7 常用的排序算法，快排，归并、冒泡。 快排的最优时间复杂度，最差复杂度。冒泡排序的优化方案。



### 6.8 二分查找的时间复杂度，优势。



### 6.9 一个已经构建好的 TreeSet，怎么完成倒排序。

* 冒泡

  ```java
  //冒泡
  public static void mp(int a[]) {
      int swap = 0;
      for (int i = 0; i < a.length; i++) {
          for (int j = i; j < a.length; j++) {
              if (a[j] a[i]) {
                  swap = a[i];
                  a[i] = a[j];
                  a[j] = swap;
              }
          }
      }
      System.out.println(Arrays.toString(a));
  }
  ```

* 二分法

  ```java
  /**
  	* 不使用递归的二分查找
       *title:commonBinarySearch
       *@param arr
       *@param key
       *@return 关键字位置
       */
  public static int commonBinarySearch(int[] arr,int key) {
      int low = 0;
      int high = arr.length - 1;
      int middle = 0;//定义middle
  
      if(key < arr[low] || key arr[high] || low high) {
          return -1;
      }
      while(low <= high){
          middle = (low + high) / 2;
          if(arr[middle] key){
              //比关键字大则关键字在左区域
              high = middle - 1;
          }else if(arr[middle] < key){
              //比关键字小则关键字在右区域
              low = middle + 1;
          }else{
              return middle;
          }
      }
      return -1;//最后仍然没有找到，则返回-1
  }
  
  /**
  	 * 使用递归的二分查找
  	 *title:recursionBinarySearch
  	 *@param arr 有序数组
  	 *@param key 待查找关键字
  	 *@return 找到的位置
  	 */
  public static int recursionBinarySearch(int[] arr,int key,int low,int high) {
  
      if(key < arr[low] || key arr[high] || low high) {
          return -1;				
      }
      int middle = (low + high) / 2;//初始中间位置
      if(arr[middle] key){
          //比关键字大则关键字在左区域
          return recursionBinarySearch(arr, key, low, middle - 1);
      }else if(arr[middle] < key){
          //比关键字小则关键字在右区域
          return recursionBinarySearch(arr, key, middle + 1, high);
      }else {
          return middle;
      }	
  }
  ```

### 6.10 什么是 B+树，B-树，列出实际的使用场景。



