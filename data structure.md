###### 二分字符串的查找
* 在Java中可以使用String对象compareTo方法比较字符串的先后顺序
``str1.compareTo(str2) ``
>结果如果为负数则说明str1排在str2前面

* 如下程序段中有人名的数据，其比较也有另一种书写方式
`` "Kelly".compareTo("Kevin")  // 结果为-10``
``"Kevin".compareTo("Kelly")  // 结果为10``

```
 public static int find(ArrayList<String> array, String aim) {
int left = 0;
int right = array.size();
while (left<=right)
    {
      int middle = (left+right)/2;
      String middleValue = array.get(middle);
      if(middleValue.equals(aim))
      {
        return middle;
      }

      else  if (middleValue.compareTo(aim)>0)
      {
        right = middle-1;
      }
      else
      {
        left = middle+1;
      }

    }

    return -1;
  }

  public static void main(String[] args) {
    ArrayList<String> array = new ArrayList<>();
    array.add("Allen");
    array.add("Emma");
    array.add("James");
    array.add("Jeanne");
    array.add("Kelly");
    array.add("Kevin");
    array.add("Mary");
    array.add("Natasha");
    array.add("Olivia");
    array.add("Rose");

    int result1 = find(array, "Kelly");
    if (result1 == -1) {
      System.out.println("Kelly 不存在名单中");
    } else {
      System.out.println("Kelly 存在名单中，位置是 " + result1);
    }

    int result2 = find(array, "Edith");
    if (result2 == -1) {
      System.out.println("Edith 不存在名单中");
    } else {
      System.out.println("Edith 存在名单中，位置是 " + result2);
    }
  }
```

###### 数组插入与删除
```
  // 此处需要声明一个数组，作为底层存储
  int[] array = new int[20];
  int size = 0;

  public YKDArrayList() {
  }

  // 获取数组的长度
  public int size() {
    return this.size;
  }

  // 数组获取某个索引值
  public int get(int index) {
    return this.array[index];
  }

  // 添加元素在末尾 
  public void add(int element) {
    //相当于调用传入this.size
    this.add(this.size, element);
  }

  // 添加元素在中间
  public void add(int index, int element) {
    if (index < 0 || index > this.size) {
      return;
    }

    // 元素依次右移
    for (int i = this.size - 1; i >= index; i--) {
      this.array[i + 1] = this.array[i];
    }
    // 插入元素
    this.array[index] = element;
    // 调整size
    this.size++;
  }

  // 删除元素
  public void remove(int index) {

    if (index < 0 || index > this.size - 1) {
      return;
    }

    // 元素依次左移
    for (int i = index; i < this.size - 1; i++) {
      this.array[i] = this.array[i + 1];
    }
    // 删除最后一个元素
    this.array[this.size - 1] = 0;
    // 调整size
    this.size--;
  }

  public static void main(String[] args) {
    YKDArrayList ykdArrayList = new YKDArrayList();
    ykdArrayList.add(1);
    ykdArrayList.add(2);
    ykdArrayList.add(3);
    ykdArrayList.add(4);

    ykdArrayList.add(0, 5);

    ykdArrayList.remove(3);
  }
```

###### 冒泡排序
```
int[] arrary = {2,5,4,3,1};
        for(int i=0;i<arrary.length;i++)
        {
            for(int j=0;j<arrary.length-i-1;j++)
            {
                if (arrary[j]>arrary[j+1])
                {
                    int temp = arrary[j];
                    arrary[j]=arrary[j+1];
                    arrary[j+1]=temp;
                }
            }
        }

           for (int k=0;k< arrary.length;k++)
           {
               System.out.println(arrary[k]);
           }
```
###### 插入排序

```
 for (int i = 1; i < array.length; i++) {
            int tmp = array[i];
            int j;
            for ( j = i; j > 0 && array[j - 1] > tmp; j--) {
                array[j] = array[j - 1];
            }
            array[j] = tmp;
        }
```

###### 汉诺塔问题

```
public static void main(String[] args)
{
  char A='A',B='B',C='C';
  hanoiTower(3,A,B,C);
}
 public static void hannoiTower(int nDisks,char A,char B,char C)
 {
   if(nDisks==1)
   {
     move(nDisks,A,C);
     return ;
   }
   hanoiTower(nDisks-1,A,C,B);
 }

```
###### 归并排序

![第一步](https://style.youkeda.com/img/course/a1/5/4-3.svg)
![第二步](https://style.youkeda.com/img/course/a1/5/4-4.svg)

```
package com.youkeda;

import java.util.Arrays;

public class Sort {

  // 归并排序
  public static int[] mergeSort(int[] array) {
    // 为了方便查看结果，我们将每个数组进行打印
    if (array.length == 1) {
      return array;
    }

    int middle = array.length / 2;
    // 处理 0 到 middle 左侧数组部分
    int[] left = mergeSort(subArray(array, 0, middle));
    // 处理 middle 到 array.length 右侧数组部分
    int[] right = mergeSort(subArray(array, middle, array.length));








    // TODO处理合并问题
    int l = 0;
    int r = 0;
    int index = 0;
    while (l < left.length && r < right.length) {
      array[index] = Math.min(left[l], right[r]);
      index++;
      if (left[l] < right[r]) {
        l++;
      } else {
        r++;
      }
    }

    // 右侧数组已经遍历完成，左侧有剩余
    if (l < left.length) {
      for(int i = l; i < left.length; i++){
        array[index] = left[i];
        index++;
      }
    }

    // 左侧数组已经遍历完成，右侧有剩余
    if(r < right.length){
      for(int i = r; i < right.length; i++){
        array[index] = right[i];
        index++;
      }
    }
    return array;
  }










  // 拷贝原数组的部分内容，从 left 到 right
  public static int[] subArray(int[] source, int left, int right) {
    // 创建一个新数组
    int[] result = new int[right - left];
    // 依次赋值进去
    for (int i = left; i < right; i++) {
      result[i - left] = source[i];
    }
    return result;
  }

  public static void main(String[] args) {
    int[] array = {9, 2, 4, 7, 5, 3};
    // Arrays.toString 可以方便打印数组内容
    System.out.println("raw: " + Arrays.toString(array));
    int[] result = mergeSort(array);
    System.out.println("result: " + Arrays.toString(result));
  }
}


```
###### 快速排序

```
public static void quickSort(int q[],int l,int r)
{
  if(l>=r)
  {
    return;
  }
  int i = l-1,j = r+1;      //创建两个指针指向数组的两端的两侧
  int x = q[(l+r)>>1];      //x为数组中间值

  while(i<j)
  {
    do i++;while(q[i]<x);      //两个指针往中间走
    do j--;while(q[j]>x);
    if(i<j)
    {
      int tmp = q[i];       //交换值
      q[j] = q[i];
      q[i] = tmp;
    }
  }


  quickSort(q,l,j);      //数组
  quickSort(q,j+1,r)      //数组右边开始排序
}
public static int main()
{
  int q[20]={};
  int n = 0;
  scanf("%d",&n);
  for(int i=0;i<n;i++)
  {
    scanf("%d",&q[i]);
  }
  quickSort(q,0,n-1);      //0为数组左端，n-1为数组右端
  
  for(int k=0;k<n;k++)
  {
    printf("%d ",q[k]);
  }
  return 0;
}

```
###### 快速查找数
```
 public static int quickFind(int[] array,int l,int r, int aim) {
    if(l>=r)return array[l];
    int i=l-1,j=r+1;
    int x = array[l+r>>1];
    while(i<j) {
      do i++; while (array[i] < x);
      do j--; while (array[j] > x);
      if (i < j) {
        int tmp = array[i];
        array[i] = array[j];
        array[j] = tmp;
      }
    }
      if (j - l + 1 >=aim) return quickFind(array, l, j, aim);
      else  return quickFind(array, j + 1, r, aim - (j - l + 1));


  }

  public static void main(String[] args) {
    int[] array = {72, 77, 48, 17, 71, 2, 25, 97, 82, 5, 2, 18, 15, 57, 7, 48, 93, 47, 38, 74, 18, 93, 98, 41, 54, 4, 47, 4, 63, 76};
    System.out.println("raw: " + Arrays.toString(array));
    // 目标是倒数第 6 个元素
    int result = quickFind(array, 0,array.length-1,array.length - 5);
    System.out.println("result: " + result);
  }
```


















