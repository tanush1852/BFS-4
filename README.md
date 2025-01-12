# BFS-4

## Problem1: Minesweeper (https://leetcode.com/problems/minesweeper/)
## Time and Space:O(mxn)
class Solution {
    int[][] dirs;
    int m,n;
    public char[][] updateBoard(char[][] board, int[] click) {
       this.dirs=new int[][]{{-1,0},{1,0},{0,1},{0,-1},{-1,-1},{-1,1},{1,1},{1,-1}}; 
       this.m=board.length;
       this.n=board[0].length;


       if(board[click[0]][click[1]]=='M'){
        board[click[0]][click[1]]='X';
        return board;
       }


       Queue<int[]> q=new LinkedList<>();
       q.add(new int[]{click[0],click[1]});
       board[click[0]][click[1]]='B';


       while(!q.isEmpty()){
           int[] curr=q.poll();
           int count=countMine(board,curr[0],curr[1]);
           if(count!=0){
            board[curr[0]][curr[1]]=(char)(count+'0');

           }
           else{
           for (int[] dir : dirs) {
                    int r = curr[0] + dir[0];
                    int c = curr[1] + dir[1];

                    if (r >= 0 && r < m && c >= 0 && c < n && board[r][c] == 'E') {
                        board[r][c] = 'B'; // Mark as visited
                        q.add(new int[]{r, c});
                    }

           }

       }
       }


       return board;
    
    
    
    
    }


    private int countMine(char[][] board,int i,int j)
    {   int count=0;
        for(int[] dir:dirs)
        {
            int r=i+dir[0];
            int c=j+dir[1];


            if(r<m && c<n && r>=0 && c>=0)
            {
                if(board[r][c]=='M')
                count++;
            }
        }
        return count;
    }


}


*
## Problem 2 Snakes and ladders (https://leetcode.com/problems/snakes-and-ladders/)
## Time and Space:O(N*N)
class Solution {
    int n;
    
    public int snakesAndLadders(int[][] board) {
        this.n = board.length;
        
        
        boolean[] visited = new boolean[n * n + 1];
        Queue<int[]> q = new LinkedList<>();
        
        // Mark start position as visited
        visited[1] = true;
        q.add(new int[]{1, 0}); // Start from cell 1 with 0 moves
        
        while (!q.isEmpty()) {
            int[] curr = q.poll();
            int cell = curr[0], moves = curr[1];
            
            // If reached destination
            if (cell == n * n) {
                return moves;
            }
            
            // Try all possible dice rolls
            for (int i = 1; i <= 6 && cell + i <= n * n; i++) {
                int nextCell = cell + i;
                
                // Convert cell number to board position
                int[] pos = getPosition(nextCell);
                int row = pos[0], col = pos[1];
                
                // Apply snake or ladder if present
                nextCell = board[row][col] == -1 ? nextCell : board[row][col];
                
                // Only process unvisited cells
                if (!visited[nextCell]) {
                    visited[nextCell] = true;
                    q.add(new int[]{nextCell, moves + 1});
                }
            }
        }
        
        return -1; // No path found
    }
    
    private int[] getPosition(int cell) {
        // Convert 1-based cell number to 0-based index
        cell--;
        
        // Calculate row and column
        int row = n - 1 - (cell / n);
        int col = cell % n;
        
        // If row is even (from bottom), reverse column direction
        if ((n - 1 - row) % 2 == 1) {
            col = n - 1 - col;
        }
        
        return new int[]{row, col};
    }
}
