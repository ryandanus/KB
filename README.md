# Kecerdasan Buatan
Tugas KB(Kecerdasan Buatan)
Ryan Danu Saputra 
05111840000144

LIST TUGAS
  - [1. 8 Puzzle (BFS)](#(BFS))
  - [2. 8 Puzzle (DFS)](#(DFS))
  
  
  
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


