```
package codingInterview.sortAndSearch;

/**
 *  비트 마스크에 대한 이해 부족..
 *
 *  중복 찾기: 1부터 N(<=32,000)까지의 숫자로 이루어진 배열이 있다. 배열엔 중복된 숫자가 나타날 수 있고, N이 무엇인지는 알 수 없다.
 *  사용 가능한 메모리가 4KB일 때, 중복된 원소를 모두 출력하려면 어떻게 해야 할까?
 *
 *  Solution)
 *  4킬로바이트 = 3,200 bit 주소 공간을 사용할 수 있다.
 *  비트벡터 딱! 맞아 떨어지네
 *  각 비트가 하나의 정수를 나타내도록 만들 수 있다.
 *
 *  비트 벡터를 사용해서, 배열을 순회하면서, 각 원소 v에 상응하는 비트벡터의 비트를 1로 만든다. 그러다가 중복된 원소를 만나면 출력한다.
 */

public class Question8 {
    public static void main(String[] args) {
        Solution s = new Solution();
        int[] array = {1, 1, 9, 2, 3, 2, 4, 5, 6, 7, 7, 8, 8, 9, 10, 1};
        s.checkDuplicates(array);
    }
    static class Solution {
        static class BitSet {
            int[] bitSet;

            public BitSet(int size)
            {
                bitSet = new int[(size >> 5) + 1]; // new int[1001];
            }

            boolean get(int pos)
            {
                int wordNumber = (pos >> 5);    // 32로 나눈다.
                int bitNumber = (pos & 0x1F);   // 32로 나눈 나머지 wow..
                return ((bitSet[wordNumber] & (1 << bitNumber)) != 0);  // 이게 모야..
            }

            void set(int pos)
            {
                int wordNumber = (pos >> 5);
//                System.out.println("wordNumber = " + wordNumber);
                int bitNumber = (pos & 0x1F);
//                System.out.println("bitNumber = " + bitNumber);
                bitSet[wordNumber] |= (1 << bitNumber);
            }
        }

        void checkDuplicates(int[] array)
        {
            BitSet bs = new BitSet(32000);
            for (int i = 0; i < array.length; i++)
            {
                int num = array[i];
                int num0 = num - 1; // bitset은 0에서 시작하고, 숫자는 1에서 시작한다.
                if (bs.get(num0))
                    System.out.println(num);
                else
                    bs.set(num0);
            }
        }

    }
}
```