# KB
tugas KB(Kecerdasan Buatan)
# SoalShiftSISOP20_modul3_C02

### Kelompok 

LIST TUGAS
  - [8 Puzzle (BFS) ](#8 puzzle-BFS)
  

## 8 Puzzle (BFS) 

  BFS merupakan singkatan dari `(Breadth First Search)` adalah suatu algoritma untuk yang melakukan pencarian berdasarkan kelebaran. strategi pencarian sederhana ini dimulai melalui simpul akar yang diperluas terlebih dahulu, kemudian di eksplorasi ke semua penerus simpul akar diperluas selanjutnya, kemudian penerusnya dan seterusnya sampai jalan terbaik yang mungkin telah ditemukan. 

Breadth First Search Menggunakan struktur data `queue` 

BFS menggunakan struktur data antrian yang merupakan struktur data `First In, First Out` atau FIFO. 
Antrian ini menyimpan semua node yang harus kita jelajahi dan setiap kali sebuah node dieksplorasi ditambahkan ke set node yang dikunjungi.

note : 
Breadth First Search digolongkan sebagai pencarian yang kurang informasi atau blind.
dikarenakan adanya kenyataan bahwa strategi untuk grafik traversal ini tidak memiliki informasi tambahan tentang status di luar yang disediakan dalam definisi masalah,

Berikut merupakan alur pencarian BFS

bfs akan secara rekursif mencari setiap tingkat pohon biner menjadi lebih dalam dan lebih dalam sampai menemukan jalan terpendek yang mungkin.

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

penyelesaian 8puzzle dalam bahasa c++ ini , melakukan pencarian secara BFS
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


