```
package codingInterview.sortAndSearch;

import java.io.FileNotFoundException;
import java.io.FileReader;
import java.util.Scanner;

/**
 *  빠트린 정수:
 *  음이 아닌 정수 40억개로 이루어진 입력 파일이 있다.
 *  이 파일에 없는 정수를 생성하는 알고리즘을 작성하라.
 *  단, 메모리는 1GB만 사용할 수 있다.
 *
 *  Solution)
 *  총 2^32(40억)개의 정수가 존재하므로 음이 아닌 정수는 2^31개가 있을 것이다.
 *  따라서 long이 아니라 int형이라고 가정하면 입력 파일에는 중복된 정수가 포함되어 있을 것이다.
 *
 *  ⭐ 외우자 ⭐
 *  int == 2^5 == 32bit == 2^32숫자를 표현할 수 있음 == 40억개
 *  양의 정수 2^31이니 int형으로 가정하면 중복된 숫자들이 포함되어 있다.
 *
 *  1GB == 1000 MB == 1000 000 KB == 1000 000 000 B == 8000 000 000 bit
 *  제공된 메모리는 1GB, 즉 비트로는 80억 비트가 있으므로, 모든 정수값을 메모리의 각 비트로 대응시킬 수 있다.
 *  그 논리는 다음과 같다.
 *  1. 40억 비트의 크기인 비트 벡터(bit vector(BV))를 만든다.
 *  비트 벡터란 int형(혹은 다른 자료형) 배열의 각 비트에 불린(boolean)값을 압축해서 저장하는 배열을 뜻한다.
 *  각 int는 32개의 불린값을 저장할 수 있다.
 *
 *  2. BV를 0으로 초기화한다.
 *
 *  3. 파일의 모든 숫자를 읽어 나가면서 BV.set(num, 1)을 호출한다.
 *  4. 이제 0번째 인덱스부터 BV를 다시 훑는다.
 *  5. 값이 0인 첫 번째 인덱스를 반환한다.
 *
 */
public class Question7 {
    static int enough = 20;                                                 // 20억개 없는 정수를 찾으려면 한 세월이라 내가 값을 지정해줌
    static long numberOfInts = ((long) Integer.MAX_VALUE) + 1;              // 2,147,483,648 // 2진수로 표현 시 2^31, 20억개
    static byte[] bitfield = new byte[(int) (numberOfInts / 8)];            // bitfield.length 268435456    => 8로 왜 나눴을까요? => 자료형 bit[] 배열은 없군요. => 자료형 byte[]로 처리하기 위해서요.
    static String filename = "/Users/hyunwlee/goinfre/hyunwlee.txt";
    public static void main(String[] args) throws FileNotFoundException {
        new Solution().findOpenNumber();
    }

    static class Solution {
        void findOpenNumber() throws FileNotFoundException {
            Scanner in = new Scanner(new FileReader(filename));
            while (in.hasNextInt()) {
                int n = in.nextInt();
                /**
                 *  OR 연산을 사용해서 bitfield에 상응하는 숫자를 찾아 n번째
                 *  위치에 값을 넣는다 (예를 들어 숫자 10은 byte 배열에서 두 번째 인덱스의 2번째 비트의 위치를 의미한다.)
                 */
                bitfield[n / 8] |= 1 << (n % 8);
            }
            for (int i = 0; i < bitfield.length / 1000; i++)
            {
                for (int j = 0; j < 8; j++)
                {
                    /**
                     *  각 바이트의 비트를 개별적으로 확인한다.
                     *  만약 0번째 비트가 찾고자 하는 비트라면 그에 상응하는 값을 출력한다.
                     */
                    if ((bitfield[i] & (1 << j)) == 0)
                    {
                        System.out.println(i * 8 + j);
//                        return ;
                    }
                }
                --enough;
                if (enough < 0)
                    return;
            }
        }
    }
}
```