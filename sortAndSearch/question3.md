```
package codingInterview.sortAndSearch;

/**
 *  난이도: 상
 *  회전된 배열에서 검색: n개의 정수로 구성된 정렬 상태의 배열을 임의의 횟수만큼 회전시켜 얻은 배열이
 *  입력으로 주어진다고 하자.
 *  이 배열에서 특정한 원소를 찾는 코드를 작성하라.
 *  회전시키기 전, 원래 배열은 오름차순으로 정렬되어 있었다고 가정한다.
 *
 *  시간복잡도가 O(log n)으로 나와야 한다.
 *
 *  왼쪽    오른쪽    배열
 *  ----------------------------------------
 *  오름차순 오름차순   오름차순
 *  오름차순 내림차순   제일 작은수가 오른쪽에 위치      [4, 5, 6, 7, 8, 0, 1]
 *  내림차순 오름차순   제일 작은수가 왼쪽이나 중간에 위치 [5, 1, 2, 3, 4]
 *  내림차순 내림차순   내림차순 => (근데 오름차순 배열이 주어지므로 이 경우는 나오지 않는다.)
 *
 */
public class Question3 {
    public static void main(String[] args) {
//        int[] arr = new int[]{10, 15, 20, 0, 5};
//        int[] arr = new int[]{50, 5, 20, 30, 40};
//        int[] arr = new int[]{2, 2, 2, 3, 4, 2};
        int[] arr = new int[]{5, 1, 3};
        System.out.println(new Solution().search(arr, 3));
    }

    static class Solution {
        public int search(int[] nums, int target)
        {
            return (search(nums, target, 0, nums.length - 1));
        }
        public int search(int[] nums, int target, int start, int end)
        {
            if (start > end)
                return (-1);
            if (start == end)
            {
                if (nums[start] == target)
                    return (start);
                return (-1);
            }
            int mid = (start + end) / 2;
            if (nums[start] < nums[mid])
            {
                if (nums[start] <= target && target <= nums[mid])
                    return (search(nums, target, start, mid));
                else
                    return (search(nums, target, mid, end));
            }
            else if (nums[start] > nums[mid])
            {
                if (nums[mid] <= target && target <= nums[end])
                    return (search(nums, target, mid, end));
                else
                    return (search(nums, target, start, mid));
            }
            else if (nums[start] == nums[mid])
            {
                if (nums[start] == target)
                    return (start);
                else if (nums[end] == target)
                    return (end);
                else
                    return (-1);
            }
            return (-1);
        }
    }
}
```