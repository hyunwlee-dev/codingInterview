```
package codingInterview.sortAndSearch;

/**
 *  [크기를 모르는 정렬된 원소 탐색]
 *  배열과 비슷하지만 size메서드가 없는 Listy라는 자료구조가 있다.
 *  여기에는 i 인덱스에 위치한 원소를 O(1)시간에 알 수 있는 elementAt(i) 메서드가 존재한다.
 *  만약 i가 배열의 범위를 넘어섰다면 -1을 반환한다.(이 때문에 이 자료구조는 양의 정수만 지원한다.)
 *  양의 정수가 정렬된 Listy가 주어졌을 때, 원소 x의 인덱스를 찾는 알고리즘을 작성하라.
 *  만약 x가 여러 번 등장한다며ㄴ 아무거나 반환하면 된다.
 *
 *
 *  Solution)
 *  배열의 size를 알려주지 않는 자료구조 Listy
 *  사용할 수 있는것은 elementAt(i)함수
 *
 *  BinarySearch를 하기위해서는 배열의 size가 필요한데
 *  O(n) 선형시간을 사용해서 size를 구하는 것보다 더 효율적인 방법이 없을까?
 *
 *  있다!
 *  ⭐️
 *  범위를 1, 2, 4, 8, 16과 같이 기하급수적으로 커지는 것이 낫다.
 *  이렇게 크기를 키운다면, 리스트의 길이가 n일 때 O(log n) 시간 안에 길이를 구할 수 있다.
 *
 *  num = 1일 때, num이 n보다 커질 때까지 num은 2배씩 증가한다.
 *  n보다 커지기 위해서 num을 몇 번이나 증가시켜야 하는가?
 *  2^k = n
 *  log n만큼
 *
 *  ⭐️
 *  binarySearch를 하는 도중 middle이 -1이라면, 오른쪽 탐색할 필요가 없다.
 */
public class Question4 {
    static class Listy
    {
        int[] arr;

        public Listy(int[] arr)
        {
            this.arr = arr;
        }

        public int elementAt(int i)
        {
            if (i < 0 || this.arr.length <= i)
                return (-1);
            else
                return (this.arr[i]);
        }

        public int search(int i)
        {
            return (search(this.arr, i, 0, arr.length - 1));
        }

        private int search(int[] arr, int i, int start, int end)
        {
            if (start > end)
                return (-1);
            if (start == end)
            {
                if (arr[start] == i)
                    return (start);
                return (-1);
            }
            int mid = (start + end) / 2;
            if (arr[mid] == i)
                return (mid);
            else if (arr[mid] > i)
                return (search(arr, i, start, mid));
            else
                return (search(arr, i, mid + 1, end));
        }
    }
    public static void main(String[] args) {
        Listy listy = new Listy(new int[]{1, 2, 3, 4, 5, 6});
//        Listy listy = new Listy(new int[]{12, 2324, 323434, 4245636, 53467547, 647635736});
//        System.out.println(listy.elementAt(-1));
//        System.out.println(listy.elementAt(5));
//        System.out.println(listy.elementAt(-1));
//        int[] arr = new int[]{1, 2, 3, 4, 5, 6};
        System.out.println(new Solution().search(listy, 1));
        System.out.println(new Solution().search(listy, 2));
        System.out.println(new Solution().search(listy, 3));
        System.out.println(new Solution().search(listy, 4));
        System.out.println(new Solution().search(listy, 5));
        System.out.println(new Solution().search(listy, 6));
        System.out.println(new Solution().search(listy, 7));
    }

    static class Solution {
        int search(Listy list, int value) {
            int idx = 1;
            // 1, 2, 4, 8, 16, 32, 64
            while (list.elementAt(idx - 1) != -1 && list.elementAt(idx - 1) < value)
                idx *= 2;
            return (binarySearch(list, value, idx / 2, idx));
        }

        int binarySearch(Listy list, int value, int low, int high)
        {
            if (low > high)
                return (-1);
            if (low == high)
            {
                if (list.elementAt(low) == value)
                    return (low);
                return (-1);
            }
            int mid = (low + high) / 2;
            if (list.elementAt(mid) == value)
                return (mid);
            else if (list.elementAt(mid) > value || list.elementAt(mid) == -1)
                return (binarySearch(list, value, low, mid));
            else
                return (binarySearch(list, value, mid + 1, high));
        }
    }

}
```