# Kecerdasan Buatan
Tugas KB(Kecerdasan Buatan)
Ryan Danu Saputra 
05111840000144

LIST TUGAS
  - [1. 8 Puzzle (BFS)](#BFS)
  - [2. 8 Puzzle (DFS)](#DFS)
  - [3. Minimax TicTacToe](#Minimax)
  - [8queenp puzzle](#8queen)
 
  
  
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

## N - Queen(8queen)
8queen puzzle adalah masalah menempatkan delapan catur ratu pada 8 × 8 papan catur sehingga tidak ada dua Ratu mengancam satu sama lain; dengan demikian, sebuah solusi mensyaratkan bahwa tidak ada dua ratu berbagi baris, kolom, atau diagonal yang sama. Delapan ratu puzzle adalah contoh yang lebih umum n Queens menempatkan n non-menyerang ratu pada n × n papan catur, yang solusi yang ada untuk semua bilangan n dengan pengecualian dari n = 2 dan n = 3. 

8 queen merupakan permainan yang diterbutjan pada tahun 1848 oleh seorang komposer catur Max Bezzel dan ada yang menemukan solusi nya pada tahun 1850. Permainan ini juga diperluas dengan cara memperluas masalah n queen dengan n x n papan caturnya 

permasalahan ini diselesaikan dengan menggunakan rekursi untuk mengecek ratu apakah ratu tersebut dapat terserang oleh ratu lain, 
```c
int is_attack(int i,int j)
{
    int k,l;
    //checking if there is a queen in row or column
    for(k=0;k<N;k++)
    {
        if((board[i][k] == 1) || (board[k][j] == 1))
            return 1;
    }
    //checking for diagonals
    for(k=0;k<N;k++)
    {
        for(l=0;l<N;l++)
        {
            if(((k+l) == (i+j)) || ((k-l) == (i-j)))
            {
                if(board[k][l] == 1)
                    return 1;
            }
        }
    }
    return 0;
}
```
jika terdapat ratu yang terserang ratu lain maka ia akan diposisikan di tempat selanjutnya oleh karena itu ia menggunakan 	`backtracking`	dimana posisi ratu sebelum itu akan diubah
```c

int N_queen(int n)
{
    int i,j;
    //if n is 0, solution found
    if(n==0)
        return 1;
    for(i=0;i<N;i++)
    {
        for(j=0;j<N;j++)
        {
            //checking if we can place a queen here or not
            //queen will not be placed if the place is being attacked
            //or already occupied
            if((!is_attack(i,j)) && (board[i][j]!=1))
            {
                board[i][j] = 1;
                //recursion
                //wether we can put the next queen with this arrangment or not
                if(N_queen(n-1)==1)
                {
                    return 1;
                }
                board[i][j] = 0;
            }
        }
    }
    return 0;
}
```
jika digambarkan maka algoritma itu akan  berjalan seperti yang digambarkan dibawah ini

![Eight-queens-animation](https://user-images.githubusercontent.com/59832754/80300410-ac2b1380-87c6-11ea-88d5-f216a0046bba.gif)

![qureen](https://user-images.githubusercontent.com/59832754/80300845-35901500-87ca-11ea-9a9d-22b486915e1f.png)


## Minimax TicTacToe 

TicTacToe merupakan game yang sangat populer, permainan ini dimainkan oleh dua pemain ,`X` dan `O` yang saling mengisi kotak 3x3
secara bergantian. Untuk memenangkan permainan ini salah satu pemain harus bisa mengisi kotak tersebut dengan tiga tanda mereka secara horizontal,vertikal atau diagonal. Dalam permainan sangat memungkinkan terjadinya seri / draw karena pada dasarnya kedua pemain harus saling memaksimalkan kemungkinan untuk meraih kemenangan dan meminimalkan terjadinya kekalahan persis dengan jalan dari algoritma minimax

Minimax adalah semacam algoritma backtracking yang digunakan dalam pengambilan keputusan dan teori permainan untuk menemukan langkah optimal bagi pemain, dengan asumsi bahwa lawan Anda juga bermain secara optimal. Ini banyak digunakan dalam dua permainan berbasis giliran pemain seperti Tic-Tac-Toe, Backgammon, Mancala, Catur, dll.
(https://www.geeksforgeeks.org/minimax-algorithm-in-game-theory-set-1-introduction/)

Di Minimax kedua pemain disebut maximizer dan minimizer. The maximizer mencoba untuk mendapatkan skor tertinggi mungkin sementara minimizer mencoba untuk melakukan yang sebaliknya dan mendapatkan skor terendah mungkin. Kejadian tersebut akan seperti ini jika digambarkan

![Annotation 2020-04-17 093335](https://user-images.githubusercontent.com/59832754/79526274-1937fe00-808f-11ea-83fb-110c856adc4d.png)


Karena ini adalah algoritma berbasis backtracking, ia mencoba semua gerakan yang mungkin, kemudian melakukan backtracks dan membuat keputusan.

jika dalam permainan ini digambarkan dengan tree maka terdapat banyak kondisi/state dan komponen-komponen pembentuknya.
Kondisi awal dalam permainan digambarkan sebagai`Initial state` dimana keadaan ketika pencarian tersebut akan dilakukan. Jika pemain pertama jalan pertama maka ,initial statenya adalah :

![tictactoe.1](https://user-images.githubusercontent.com/52326074/77034748-4dab9200-69dd-11ea-8215-48c660f28237.JPG)

Initial state ini akan selalu berubah saat giliran jalan pemain pertama.

![basic-minimax-1](https://user-images.githubusercontent.com/59832754/79627740-1576ab00-8165-11ea-8fde-7ee92a124b8a.png)


Lalu saat pemain mengisi kotak permainan secara sembarang di kotak yang masih kosong terdapat`Operator (rule/ilegal moves)`, Operator pada permainan ini adalah pemain 

Ada juga `Terminal Test` yang merupakan keadaan dimana komposisi terbaik yang dilakukan pemain pertama setelah diadakan penelusuran.

Dan perhitungan poin minimax ditentukan melalui `Utility function` yang mencari kemungkinan kemenangan bagi pemain pertama dikurangi dengan kemungkinan kemenangan dari pemain kedua. 

Ketika permainan tictactoe dimulai tiap pemain akan memiliki kemungkinan kemenangan sejumlah 8 yaitu saat salah satu dari pemain mengisi kotak permainan secara horizontal, vertikal, dan diagonal.

![tictactoe.2](https://user-images.githubusercontent.com/52326074/77034890-b561dd00-69dd-11ea-9a93-b8adca75b3e8.JPG)

horizontal 	: 3
vertikal	: 3
diagonal	: 2

`3 + 3 + 2 = 8` kemungkinan menang tiap pemain

Contoh Perhitungan pada komposisi :

![tictactoe.3](https://user-images.githubusercontent.com/52326074/77034911-c4488f80-69dd-11ea-904e-87b6294917db.JPG)

Kemungkinan kemenangan pemain pertama adalah 6, sedangkan kemungkinan kemenangan pemain kedua adalah 5.

![tictactoe.4](https://user-images.githubusercontent.com/52326074/77034938-d88c8c80-69dd-11ea-845c-596e936eaad9.JPG)

Jadi nilai untuk komposisi di atas adalah 6 – 5 = 1.

Dari keadaan permainan dimulai, graph minimax untuk Two-Ply Search dapat digambarkan sebagai berikut 
(X = pemain pertama, O = pemain kedua):

![tictactoe.5](https://user-images.githubusercontent.com/52326074/77034975-efcb7a00-69dd-11ea-97ae-2ff5b0943d04.JPG)

Nilai node E sampai P didapat dari utility function yang telah didefinisikan sebelumnya sehingga didapatkan :
```
E = 6 – 5 = 1
F = 5 – 5 = 0
G = 6 – 5 = 1
H = 5 – 5 = 0
I = 4 – 5 = -1
J = 5 – 4 = 1
K = 6 – 4 = 2
L = 5 – 6 = -1
M = 5 – 5 = 0
N = 5 – 6 = -1
O = 6 – 6 = 0
P = 4 – 6 = -2
```

Ada sembilan langkah yang mungkin dilakukan oleh pemain pertama karena kotak kosongnya berjumlah 9, tapi pada diagram diatas hanya diambil 3 kemungkinan (node B,C dan D), karena 6 kemungkinan lainnya setara dengan ke-3 komposisi di atas, misalnya :

![tictactoe.6](https://user-images.githubusercontent.com/52326074/77035007-083b9480-69de-11ea-9932-0b194195291b.JPG)

Dari node B seharusnya didapatkan node anak sebanyak 8 node, dengan mengabaikan node yang setara node anak dari B menjadi E, F, G, H dan I. Dengan cara yang sama didapatkan node anak dari C yaitu J dan K, sedangkan node anak dari D yaitu L, M, N, O dan P.

Karena hanya menggunakan Two-Ply Search, node-node anak dari B, C dan D dicari nilainya, kemudian dicari yang terkecil (min) masing-masing untuk dijadikan nilai B, C dan D. Selanjutnya dicari nilai terbesar (max) dari ketiga nilai tersebut untuk menentukan langkah pemain pertama.

Pada diagram yang digambarkan diatas, node anak dari B bernilai masing-masing E=1, F=0, G=1, H=0 dan I=-1 jadi nilai B diambil yang terkecil yaitu –1.

Node anak dari C bernilai masing-masing J=1 dan K=2 jadi nilai C diambil yang terkecil yaitu 1. Kemudian node anak dari D bernilai masing-masing L=-1, M=0, N=-1, O=0 dan P=-2 jadi nilai C diambil yang terkecil yaitu –2.

Selanjutnya dari nilai node B=-1, C=1, dan D=-2 diambil nilai terbesar yaitu C=1, yang berarti langkah terbaik yang harus dilakukan oleh pemain pertama adalah node C.

Setelah pemain kedua menempatkan buahnya, keadaan saat itu dijadikan initial state dan dilakukan kembali pelacakan dengan langkah yang telah dijelaskan di atas. Contoh : Misalkan pemain kedua meletakkan buahnya seperti gambar berikut :

![tictactoe.7](https://user-images.githubusercontent.com/52326074/77035046-21444580-69de-11ea-89a1-5eeb2dd345c4.JPG)

Dari keadaan ini, graph minimax untuk Two-Ply Search dapat digambarkan sebagai berikut (X = pemain pertama, O = pemain kedua):

![tictactoe.8](https://user-images.githubusercontent.com/52326074/77035066-2e613480-69de-11ea-816d-f67b069d2523.JPG)

Dari diagram diatas, langkah terbaik yang dapat dilakukan oleh pemain pertama terdapat dua pilihan yaitu B(1) dan D(1).

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


