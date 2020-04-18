# Kecerdasan Buatan
Tugas KB(Kecerdasan Buatan)
Ryan Danu Saputra 
05111840000144

LIST TUGAS
  - [1. 8 Puzzle (BFS)](#BFS)
  - [2. 8 Puzzle (DFS)](#DFS)
  - [3. Minimax TicTacToe](#Minimax)
 
  
  
## (BFS) 8 Puzzle 

  BFS merupakan singkatan dari `(Breadth First Search)` adalah suatu algoritma untuk yang melakukan pencarian berdasarkan kelebaran. strategi pencarian sederhana ini dimulai melalui simpul akar yang diperluas terlebih dahulu, kemudian di eksplorasi ke semua penerus simpul akar diperluas selanjutnya, kemudian penerusnya dan seterusnya sampai jalan terbaik yang mungkin telah ditemukan. 

Breadth First Search Menggunakan struktur data `queue` 

  BFS menggunakan struktur data antrian yang merupakan struktur data `First In, First Out` atau FIFO. 
Antrian ini menyimpan semua node yang harus kita jelajahi dan setiap kali sebuah node dieksplorasi ditambahkan ke set node yang dikunjungi.

note : 
  Breadth First Search digolongkan sebagai pencarian yang kurang informasi atau blind.
dikarenakan adanya kenyataan bahwa strategi untuk grafik traversal ini tidak memiliki informasi tambahan tentang status di luar yang disediakan dalam definisi masalah,

Berikut merupakan alur pencarian BFS

  Bfs akan secara rekursif mencari setiap tingkat pohon biner menjadi lebih dalam dan lebih dalam sampai menemukan jalan terpendek yang mungkin.

![BFS](https://user-images.githubusercontent.com/59832754/79303582-714cf400-7f19-11ea-9625-c38803e3f668.gif)

- Node 1 merupakan root 
- Node ini akan dimasukkan ke queue
- Tambahkan Node ini ke set yang dikunjungi
- Jika node ini adalah node tujuan kami, maka kembalikan benar,
  jika tidak, tambahkan Node 2,3 dan 4 ke queue
- Cek Node 2 dan jika itu bukan pencarian, Tambahkan Node 5 ke queue
- Ambil node selanjutnya dari queue yang seharusnya Node 3 dan cek lagi
- Jika Node 3 bukan node tujuan kami , tambahkan Node 6 dan Node 7 ke queue
- Ulangi sampai Node sasaran ditemukan.

Source Code : [8-puzzle_bfs.cpp](https://github.com/ryandanus/KB/blob/master/8puzzle%20(BFS)/8-puzzle_bfs.cpp)

  Penyelesaian 8puzzle dalam bahasa c++ ini , melakukan pencarian secara `BFS`
Pencarian dilakukan dengan memperluas jangkauannya , dimulai dari root kemudian ke tingkat selanjutnya namun ketika di tingkatan selanjutnya tidak ditemukan nilai yang cari maka program akan meneruskan ke sebelahnya bukan ke tingkatan selanjutnya lagi.

```c
int bfs(){
		deque<Node> toexpand;
		deque<State> expanded;
		
		toexpand.push_back(*this);
		while ( !toexpand.empty() ){
			if ( toexpand.front().s.goal()==1 ){ 
				cout << "------|BFS|------" << endl;
				cout << "Solution found!" << endl;
				toexpand.front().print();
				cost = toexpand.front().cost;
				toexpand.clear();
				return cost;
			}
			else{
				if ( !(toexpand.front().expanded(&expanded)) ){
					toexpand.front().expand(&toexpand);
					expanded.push_front( toexpand.front().s );
					toexpand[1].cost=toexpand[0].cost+1;
				}
				toexpand.pop_front();
			}
		}
		if ( toexpand.empty() ) cout << endl << "Solution NOT found!" << endl;
		return 0;
	}
```

## (DFS) 8 Puzzle 

  DFS merupakan singkatan dari `Depth First Search` adalah suatu algoritma untuk yang melakukan pencarian berdasarkan kedalaman. strategi pencarian sederhana ini dimulai melalui simpul akar yang diperdalam terhadap suatu Node, kemudian di eksplorasi ke semua penerus simpul akar diperdalam hingga ujung Node, kemudian mengunjungi cabang node yang lain dan seterusnya sampai menemukan suatu nilai yang diinginkan.

Jika pada BFS Menggunakan struktur data 'queue',DFS berlawanan dan menggunakan struktur data `stack` 
  
Berikut merupakan alur pencarian BFS

  DFS `Depth First Search` akan berjalan secara rekursif dan menggunakan gagasan backtracking karena semua node yang kita jelajahi  yang dengan melanjutkan, jika mungkin, dengan mundur melalui Node yang sudah dikunjungi.

![DFS](https://user-images.githubusercontent.com/59832754/79416990-22679300-7fdb-11ea-8cd2-7c3a64e0fa98.gif)

- Node 1 merupakan root 
- Lalu akan mengunjungi Node paling kiri yaitu Node 2
- Jika node ini adalah node tujuan kami, maka kembalikan benar,
  jika tidak, lanjutkan pencarian ke anak Node 2 yaitu  
- Cek Node 3 dan jika itu bukan pencarian, lanjutkan  pencarian ke anak dari Node 3
- Karena Node 3 merupakan bagian dalam ia akan `kembali` dan mengecek anak dari Node 2 yang lain
- Karena Node 2 tidak memiliki anak, maka ia akan `kembali` ke atasnya lagi yaitu root Node 1 
- Pencarian akan diulangi dengan melakukan langkah yang sama melalui anak lain dari Node 1 
- Ulangi sampai Node sasaran ditemukan.

Source Code : [8-puzzle_dfs.cpp](https://github.com/ryandanus/KB/blob/master/8puzzle%20(DFS)/8-puzzle_dfs.cpp)

  Penyelesaian 8puzzle dalam bahasa c++ ini , melakukan pencarian secara `DFS`
Pencarian dilakukan dengan memperdalam jangkauannya , dimulai dari root kemudian ke tingkat selanjutnya hingga sampai dalam.


```c
int dfs(int idsdepth){
		deque<Node> toexpand;
		
		if (idsdepth==-1) idsdepth = sizeof(int);
		
		toexpand.push_back(*this);
		while ( !toexpand.empty() ){
				if (toexpand.back().depth < idsdepth){
					if ( toexpand.back().s.goal()==1 ){ 
						if (idsdepth == sizeof(int)) 
							cout << "------|DFS|------" << endl;
						cout << "Solution found!" << endl;
						toexpand.back().print();
						toexpand.clear();
						return cost;
					}
					else{
						Node t;
						t= toexpand.back().copy();
						toexpand.pop_back();
						t.expand(&toexpand);
					}
				}
				else return 0;
		}
		if ( toexpand.empty() ) cout << endl << "Solution NOT found!" << endl;
		return 0;
	}
```
## Minimax TicTacToe 

TicTacToe merupakan game yang sangat populer, permainan ini dimainkan oleh dua pemain ,`X` dan `O` yang saling mengisi kotak 3x3
secara bergantian. Untuk memenangkan permainan ini salah satu pemain harus bisa mengisi kotak tersebut dengan tiga tanda mereka secara horizontal,vertikal atau diagonal. Dalam permainan sangat memungkinkan terjadinya seri / draw karena pada dasarnya kedua pemain harus saling memaksimalkan kemungkinan untuk meraih kemenangan dan meminimalkan terjadinya kekalahan persis dengan jalan dari algoritma minimax

Minimax adalah semacam algoritma backtracking yang digunakan dalam pengambilan keputusan dan teori permainan untuk menemukan langkah optimal bagi pemain, dengan asumsi bahwa lawan Anda juga bermain secara optimal. Ini banyak digunakan dalam dua permainan berbasis giliran pemain seperti Tic-Tac-Toe, Backgammon, Mancala, Catur, dll.
(https://www.geeksforgeeks.org/minimax-algorithm-in-game-theory-set-1-introduction/)

Di Minimax kedua pemain disebut maximizer dan minimizer. The maximizer mencoba untuk mendapatkan skor tertinggi mungkin sementara minimizer mencoba untuk melakukan yang sebaliknya dan mendapatkan skor terendah mungkin. Kejadian tersebut akan seperti ini jika digambarkan

![Annotation 2020-04-17 093335](https://user-images.githubusercontent.com/59832754/79526274-1937fe00-808f-11ea-83fb-110c856adc4d.png)


Karena ini adalah algoritma berbasis backtracking, ia mencoba semua gerakan yang mungkin, kemudian melakukan backtracks dan membuat keputusan.

Source Code : [bot tic tac toe minimax](https://github.com/ryandanus/KB/blob/master/Tictactoe/minimax.cpp)

terdapat beberapa fungsi pada kodingan tersebut yaitu `gridChar();` ,`draw();` ,`win();` ,`minimax();` ,`computerMove();` ,`playerMove();` ,`main();` 

`gridChar();` berfungsi untuk mengecek apakah di dalam kolom tersebut sudah terisi karakter X maupun O atau masih kosong

`draw();`draw untuk mencetak board papan tictactoe yang berbentuk array dan sebelumnya telah di cek di fungsi grid

`win();` fungsi win untuk mengecek kondisi capaian kemenangan dari pemain, apakah ada pemain yang telah mengisi board secara horizontal,vertikal ataupun diagonal

```c
int win(const int board[9]) {
    //determines if a player has won, returns 0 otherwise.
    unsigned wins[8][3] = {{0,1,2},{3,4,5},{6,7,8},{0,3,6},{1,4,7},{2,5,8},{0,4,8},{2,4,6}};
    int i;
    for(i = 0; i < 8; ++i) {
        if(board[wins[i][0]] != 0 &&
           board[wins[i][0]] == board[wins[i][1]] &&
           board[wins[i][0]] == board[wins[i][2]])
            return board[wins[i][2]];
    }
    return 0;
}
```
`minimax();` fungsi inilah yang mengimplementasikan metode minimax pada permainan tictactoe ini dengan mengecek perpindahan pemain dan ia akan berjalan secara rekursif , pertama kali dipanggil di `computerMove();`

```c
int minimax(int board[9], int player) {
    //How is the position like for player (their turn) on board?
    int winner = win(board);
    if(winner != 0) return winner*player;

    int move = -1;
    int score = -2;//Losing moves are preferred to no move
    int i;
    for(i = 0; i < 9; ++i) {//For all moves,
        if(board[i] == 0) {//If legal,
            board[i] = player;//Try the move
            int thisScore = -minimax(board, player*-1);
            if(thisScore > score) {
                score = thisScore;
                move = i;
            }//Pick the one that's worst for the opponent
            board[i] = 0;//Reset board after try
        }
    }
    if(move == -1) return 0;
    return score;
}
```






