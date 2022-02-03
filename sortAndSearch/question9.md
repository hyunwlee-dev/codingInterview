```
package codingInterview.sortAndSearch;

/**
 *  정렬된 행렬 탐색: 각 행과 열이 오름차순으로 정렬된 (N x M) 행렬이 주어졌을 때, 특정한 원소를 찾는 메서드를 구현하라.
 *
 *       0   1   2   3
 *----------------------
 *  0   15  20  40   85
 *----------------------
 *  1   20  35  80   95
 *----------------------
 *  2   30  55  95   105
 *----------------------
 *  3   40  80  100  200
 *----------------------
 *
 *  solution1)
 *  이진탐색 시간복잡도: O(M log N), (이걸 이진탐색이라 하는게 웃기네)
 *  총 M개의 행이 있고, 각 행을 탐색하는 데 O(log N)이 걸린다.
 *
 *  [if] find X
 *  1) 어떤 열의 시작점 원소의 값이 원소의 값이 X보다 크다면, x는 해당 열 왼쪽에 있다.
 *  2) 어떤 열의 마지막 원소의 값이 원소의 값이 X보다 작으면, x는 해당 열 오른쪽에 있다.
 *  3) 어떤 행의 시작점 원소의 값이 원소의 값이 X보다 크다면, x는 해당 행 위쪽에 있다.
 *  4) 어떤 행의 마지막 원소의 값이 원소의 값이 X보다 작으면, x는 해당 행 아래쪽에 있다.
 */
public class Question9 {
    public static void main(String[] args) {
        int[][] arr = {{15, 20, 40, 85}, {20, 35, 80, 95}, {30, 55, 95, 105}, {40, 80, 100, 200}};
        Solution solution = new Solution();
        Solution.Coordinate coordinate = solution.findElement(arr, 70);
        if (coordinate == null)
            System.out.println("Not Found Exception");
        else
            System.out.println(coordinate.row + ":" + coordinate.column);
    }

    static class Solution {
        /* 2.solution */
        public class Coordinate implements Cloneable {
            int row;
            int column;
            public Coordinate(int row, int column)
            {
                this.row = row;
                this.column = column;
            }

            public boolean inbounds(int[][] matrix)
            {
                return ((0 <= row && row < matrix.length) && (0 <= column && column < matrix[0].length));
            }

            public boolean isBefore(Coordinate p)
            {
                return (row <= p.row && column <= p.column);
            }

            public Object clone() {
                return (new Coordinate(row, column));
            }

            public void setToAverage(Coordinate min, Coordinate max)
            {
                row = (min.row + max.row) / 2;
                column = (min.column + max.column) / 2;
            }
        }

        Coordinate findElement(int[][] matrix, Coordinate origin, Coordinate dest, int value) {
            // origin == (0, 0)
            // dest == (matrix.length - 1, matrix[0].length - 1)
            /* inbound(matrix): matrix 범위안에는 있는거지? */
            if (!origin.inbounds(matrix) || !dest.inbounds(matrix))
                return (null);
            /* 정답을 찾음 */
            if (matrix[origin.row][origin.column] == value)
                return (origin);
            /* dest가 coordinate상 더 크거나 같아야 한다. */
            if (!origin.isBefore(dest))
                return (null);
            if (origin.row == dest.row && origin.column == dest.column)
                return (null);
            /* start를 대각선 시작 지점으로 설정하고 end를 대각선의 마지막 지점으로 설정한다.
             * 주어진 행렬이 정사각형이 아닐 수도 있기 때문에, 마지막 지점은 dest와 다를 수 있다. */
            int diagDist = Math.min(dest.row - origin.row, dest.column - origin.column);
            Coordinate start = (Coordinate) origin.clone();
            Coordinate end = new Coordinate(start.row + diagDist, start.column + diagDist);
            Coordinate mid = new Coordinate(0, 0);

            while (start.isBefore(end))
            {
                mid.setToAverage(start, end);
                if (value > matrix[mid.row][mid.column])
                {
                    start.row = mid.row + 1;
                    start.column = mid.column + 1;
                }
                else
                {
                    end.row = mid.row - 1;
                    end.column = mid.column - 1;
                }
            }

            Coordinate lowerLeftOrigin = new Coordinate(start.row, origin.column);
            Coordinate lowerLeftDest = new Coordinate(dest.row, start.column - 1);
            Coordinate upperRightOrigin = new Coordinate(origin.row, start.column);
            Coordinate upperRightDest = new Coordinate(start.row - 1, dest.column);

            /* 행렬을 사분면으로 분할한다. 왼쪽 하단과 오른쪽 상단을 탐색한다. */
            Coordinate c;
            c = findElement(matrix, lowerLeftOrigin, lowerLeftDest, value);
            if (c == null)
                return (findElement(matrix, upperRightOrigin, upperRightDest, value));
            else
                return (c);
        }
        /**
         *       0           1   2   3
         *--------------------------------------
         *  0   15(origin)  20  40   85
         *--------------------------------------
         *  1   20          35  80   95
         *--------------------------------------
         *  2   30          55  95   105
         *--------------------------------------
         *  3   40          80  100  200(dest)
         *--------------------------------------
         */
        Coordinate findElement(int[][] matrix, int value)
        {
            Coordinate origin = new Coordinate(0, 0);
            Coordinate dest = new Coordinate(matrix.length - 1, matrix[0].length - 1);
            return (findElement(matrix, origin, dest, value));
        }


        /* 1.solution */
//        boolean findElement(int[][] matrix, int elem)
//        {
//            int row = 0;
//            int col = matrix[0].length - 1;
//            while (row < matrix.length && col >= 0)
//            {
//                if (matrix[row][col] == elem)
//                    return (true);
//                else if (matrix[row][col] > elem)
//                    --col;
//                else
//                    ++row;
//            }
//            return (false);
//        }
    }
}
```