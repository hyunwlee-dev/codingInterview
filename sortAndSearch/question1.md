```
package codingInterview.sortAndSearch;

/**
 *  정렬된 병합: 정렬된 배열A와 배열B가 주어진다.
 *  A의 끝에는 B를 전부 넣을 수 있을 만큼 충분한 여유 공간이 있다.
 *  B와 A를 정렬된 상태로 병합하는 메서드를 작성하라.
 *
 *  Hint
 *  배열의 끝에서 앞으로 움지이면서 작업해 보라.
 *
 *  Solution
 *  배열 A의 끝부분에 충분한 여유 공간이 있으므로 추가로 공간을 할당할 필요는 없다.
 *  어떤 원소를 A의 앞에 삽입해야 할 경우 삽입할 공간을 만들기 위해 기존 원소들을 뒤로 옮겨야 한다는 문제가 있다.
 *  그래서 여유공간이 있는 배열 뒤쪽부터 삽입하는 것이 좋다.
 */
public class Question1 {
    public static void main(String[] args) {
        int[] a = {1, 3, 5, 7, 8, 11, 23, 34, 0, 0, 0, 0, 0, 0, 0};
        int[] b = {2, 4, 5, 7};
        merge(a, b, 8, 4); /* merge to a */
        for (int i : a)
            System.out.print(i + " ");
    }

    private static void merge(int[] a, int[] b, int lastA, int lastB)
    {
        int idxA = lastA - 1;
        int idxB = lastB - 1;
        int current = lastA + lastB - 1;
        while (idxB >= 0)
        {
            if (idxA >= 0 && a[idxA] > b[idxB])
                a[current--] = a[idxA--];
            else
                a[current--] = b[idxB--];
        }
    }
}
```