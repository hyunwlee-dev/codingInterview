```
package codingInterview.sortAndSearch;

/**
 *  드문드문 탐색: O(n) 빈문자열 일 경우 문자열을 찾는데 최악의 경우 O(n)시간이 들 수 있다.
 *  빈 문자열이 섞여 있는 정렬된 문자열 배열이 주어졌을 때, 특정 문자열의 위치를 찾는 메서드를 작성하라
 *  ball, {"at", "", "", "", "ball", "", "", "car", "", "", "dad", "", ""};
 *  return (4);
 *
 */
public class Question5 {
    public static void main(String[] args) {
        //                0    1   2   3     4      5  6     7    8   9    10    11  12
        String[] sArr = {"at", "", "", "", "ball", "", "", "car", "", "", "dad", "", ""};
        //----------------------------------------------------------------------------------
        //              start  1   2   3     4      5  mid   7    8   9    10    11  end
        //----------------------------------------------------------------------------------
        //              start  1   2   3     4     lt  mid   rt   8   9    10    11  end
        //              start  1   2   3     lt     5  mid   7    rt  9    10    11  end
        //              start  1   2   lt    4      5  mid   7    8   rt   10    11  end
        //              start  1   lt  3     4      5  mid   7    8   9    rt    11  end
        //              start  lt  2   3     4      5  mid   7    8   9    10    rt  end
        //              s(lt)  1   2   3     4      5  mid   7    8   9    10    11  e(rt)
        System.out.println(Solution.binarySearch(sArr, "ball"));
    }

    static class Solution {
        public static int binarySearch(String[] sArr, String value) // 예외처리
        {
            if (sArr == null || value == null || value.isEmpty())
                return (-1);
            return (binarySearch(sArr, value, 0, sArr.length - 1));
        }

        public static int binarySearch(String[] sArr, String value, int start, int end)
        {
            if (start > end)
                return (-1);
            if (start == end)
            {
                if (value.equals(sArr[start]))
                    return (start);
                return (-1);
            }
            int mid = (start + end) / 2;
            if (sArr[mid].isEmpty())    // string의 isEmpty()함수에 대해 처음알았네
            {
                int left = mid - 1;
                int right = mid + 1;
                while (true)            // sArr[mid]에서 가장 가까운 문자열을 찾는다 (빈 문자열 제외)
                {
                    if (left < start && right > end)
                        return (-1);
                    if (left >= start && !value.isEmpty())
                    {
                        mid = right;
                        break;
                    }
                    if (right <= end && !value.isEmpty())
                    {
                        mid = left;
                        break;
                    }
                    --left;
                    ++right;
                }
            }
            if (sArr[mid].compareTo(value) == 0)
                return (mid);
            else if (sArr[mid].compareTo(value) > 0)
                return (binarySearch(sArr, value, start, mid));
            else
                return (binarySearch(sArr, value, mid + 1, end));
        }
    }
}
```