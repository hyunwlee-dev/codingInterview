```
package codingInterview.sortAndSearch;

import codingInterview.dataStructure.HashMapList;

import java.util.*;

/**
 *  Anagram 묶기: 철자 순서만 바꾼 문자열(anagram)이 서로 인접하도록 문자열 배열을 정렬하는 메서드를 작성하라.
 *  답을 보고 어쩌라고 라는 생각이 들었다.
 *
 *  방법 1) O(n log n)
 *  비교 연산자(comparator)를 변경한 뒤 표준적인 정렬 알고리즘, 즉 병합 정렬이나 퀵 정렬 등을 그대로 사용하는 것이다.
 *
 *  방법 2) 해시테이블 O(n)
 *  해시테이블을 사용해서 정렬된 단어로부터 실제 단어로의 매핑을 만들어 주었다.
 *  사실, 버킷 정렬을 변형한 것과 같다.
 */
public class Question2 {
    static HashMapList<String, String> mapList = new HashMapList<>();
    static class AnagramComparator implements Comparator<String> {
        public String sortChars(String s) {
            char[] content = s.toCharArray();
            return (new String(content));
        }
        @Override
        public int compare(String s1, String s2) {
            return sortChars(s1).compareTo(s2);
        }
    }

    public static void sort(String[] array)
    {
        /* anagram 단어 그룹 생성 */
        for (String s : array)
        {
            String key = sortChars(s);
            mapList.put(key, s);
        }

        /* 해시테이블을 배열로 변환 */
        int idx = 0;
        for (String key : mapList.keySet())
        {
            List<String> list = mapList.get(key);
            for (String t : list)
                array[idx++] = t;
        }
    }

    public static String sortChars(String s)
    {
        char[] content = s.toCharArray();
        Arrays.sort(content);
        return (new String(content));
    }


    public static void main(String[] args) {
        String[] s = {"hello", "ehllo"};
        // 1.방법
//        Arrays.sort(s, new AnagramComparator());
        // 2.방법
        sort(s);
        for (String set : mapList.keySet())
        {
            System.out.print(set + ": ");
            List<String> list = mapList.get(set);
            for (String ll : list)
                System.out.print(ll + " ");
            System.out.println();
        }
    }
}
```