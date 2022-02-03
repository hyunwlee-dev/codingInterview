```
package codingInterview.sortAndSearch;

import java.io.BufferedReader;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;

/**
 *  난이도: ⭐⭐⭐⭐⭐
 *  Skip.. 아직 나는 멀었구나..
 *-----------------------------------------
 *  1000 * 1000 = 100만
 *  1000 * 1000 * 1000 = 10억
 *  1000 * 1000 * 1000 * 1000 = 1조
 *
 *  1B   == 8
 *  1KB == 8000
 *  1MB == 800만
 *  1GB == 80억
 *
 *  2^10 == 1000
 *  2^20 == 1000 * 1000
 *-----------------------------------------
 *  연관문제: 만약 메모리가 10MB 뿐이라면?
 *
 *  그냥 정수값을 100개씩 나눈다고 생각하자.
 *
 *  rangeSize == 각 블록의 크기를 뜻한다.
 *  arraySize == 전체 블록의 개수를 나타낸다. 음이 아닌 정수가 2^31개가 존재하므로, 2^31 / rangeSize
 *
 *  비트 벡터를 메모리에서 전부 올릴 수 있을 만한 크기의 rangeSize를 구한다.
 *  arraySize = 2^31 == 20억 / rangeSize <= 2^21 == 200만,  10MB 800만
 *  rangeSize >= 2^31 / 2^21 (아.. 이걸 추론하는게 쉽지 않네)
 *  rangeSize >= 2^10
 */
public class Question7_sub {
    static String filename = "/Users/hyunwlee/goinfre/hyunwlee.txt";
    public static void main(String[] args) throws IOException {
        new Solution().findOpenNumber(filename);
    }
    static class Solution {
        int findOpenNumber(String name) throws IOException {
            int rangeSize = (1 << 20); // 2^20 bits (2^17 bytes)

            /* 각 블록안에 있는 숫자의 개수 세기 */
            int[] blocks = getCountPerBlock(filename, rangeSize);

            /* 빠진 숫자가 있는 블록 찾기 */
            int blockIndex = findBlockWithMissing(blocks, rangeSize);
            if (blockIndex < 0)
                return (-1);

            /* 해당 영역에서 사용될 비트 벡터 생성하기 */
            byte[] bitVector = getBitVectorRange(filename, blockIndex, rangeSize);

            /* 비트 벡터에서 0인 위치 찾기 */
            int offset = findZero(bitVector);
            if (offset < 0)
                return (-1);
            /* 빠진 값 계산하기 */
            return (blockIndex * rangeSize + offset);
        }

        /* 바이트에 0이 들어 있는 비트의 인덱스 찾기 */
        int findZero(byte b)
        {
            for (int i = 0; i < Byte.SIZE; i++)
            {
                int mask = 1 << i;
                if ((b & mask) == 0)
                    return (i);
            }
            return (-1);
        }

        /* 비트 벡터에서 0을 찾고 해당 인덱스를 반환 */
        int findZero(byte[] bitVector)
        {
            for (int i = 0; i < bitVector.length; i++)
            {
                if (bitVector[i] != ~0) // If not all 1s
                {
                    int bitIndex = findZero(bitVector[i]);
                    return (i * Byte.SIZE + bitIndex);
                }
            }
            return (-1);
        }


        /* 해당 영역에서 사용될 비트 벡터 생성하기 */
        byte[] getBitVectorRange(String filename, int blockIndex, int rangeSize) throws IOException{
            int startRange = blockIndex * rangeSize;
            int endRange = startRange + rangeSize;
            byte[] bitVector = new byte[rangeSize/Byte.SIZE];
            BufferedReader br = new BufferedReader(new FileReader(filename));
            while (true)
            {
                String s = br.readLine();
                if (s == null)
                    break;
                int value = Integer.parseInt(s);
                /* 숫자가 블록 안에 존재한다면 그 숫자를 기록한다. */
                if (startRange <= value && value < endRange)
                {
                    int offset = value - startRange;
                    int mask = (1 << (offset % Byte.SIZE));
                    bitVector[offset / Byte.SIZE] |= mask;
                }
            }
            br.close();
            return (bitVector);
        }

        /* 숫자를 센 다음, 값이 작은 블록을 찾기 */
        int findBlockWithMissing(int[] blocks, int rangeSize) {
            for (int i = 0; i < blocks.length; i++)
                if (blocks[i] < rangeSize)
                    return (i);
            return (-1);
        }

        /* 해당 영역에 들어 있는 원소의 개수 세기 */
        int[] getCountPerBlock(String filename, int rangeSize) throws IOException
        {
            int arraySize = Integer.MAX_VALUE / rangeSize + 1;
            int[] blocks = new int[arraySize];
            BufferedReader br = new BufferedReader(new FileReader(filename));
            while (true)
            {
                String s = br.readLine();
                if (s == null)
                    break;
                int value = Integer.parseInt(s);
                ++blocks[value / rangeSize];
            }
            br.close();
            return (blocks);
        }
    }
}
```