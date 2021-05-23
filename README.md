### Kelompok IT-04

* Maulida Meirisdiana 05311940000005
* Dava Aditya Jauhar 05311940000030
* Gerry Putra Fresnando 05311940000031

### Pembahasan Soal

## Soal 2

#### a. Membuat program perkalian matrix (4x3 dengan 3x6) dan menampilkan hasilnya. Matriks nantinya akan berisi angka 1-20 (tidak perlu dibuat filter angka).

    #include <stdio.h>
    #include <sys/ipc.h>
    #include <sys/shm.h>
    #include <unistd.h>
    #define col1 4
    #define col2 3
    #define col3 6
    #define zero 0

    int arr1[col1][col2];
    int arr2[col2][col3];
    int temp[col1][col3];

* Pertama kami menginputkan library yang akan digunakan, setelah itu men-define beberapa variabel konstan untuk membuat ukuran matriks sesuai dengan yang dibutuhkan.
* Selanjutnya kami membuat array dua dimensi untuk membuat matriks1, matriks2, dan hasil perkalian dari matriks1 dan matriks2.

      void matrixprep(){

      int a = 0, rem;
      if(a == 0){
        a++;
        rem = col1*col2;
        for(int i = 0; i < col1; i++){
          for(int j = 0; j < col2; j++){
            printf("Number to input to arr1 remaining: %d\n", rem-(i*col2+j));
            scanf("%d", &arr1[i][j]);
            printf("\n");
          }
        } 
      }
      if (a != 0){
        rem = col2*col3;
        for(int i = 0; i < col2; i++){
          for(int j = 0; j < col3; j++){
            printf("Number to input to arr2 remaining: %d\n", rem-(i*col3+j));
            scanf("%d", &arr2[i][j]);
            printf("\n");
          }
        }
      }
      printf("\n");

      }
      
* Fungsi `matrixprep()` digunakan untuk menginputkan angka yang akan dimasukkan ke dalam matriks. 
* Pertama dilakukan inisiasi variabel `a = 0` sebagai parameter untuk menjalankan iterasi pemasukan nilai pada matriks 1. Setelah itu dideklarasikan ukuran dari matriks 1 di dalam variabel `rem` dan selanjutnya adalah menjalankan perulangan for untuk memasukkan nilai pada baris dan kolom matriks.
* Pemasukan nilai untuk matriks oleh user dilakukan dengan cara mengeluarkan output berisi perintah untuk memasukkan nilai pada matriks dengan posisi `rem-(i*col2+j)` dan setelah itu inputan nilai akan disimpan dengan fungsi `scanf`.
* Setelah iterasi pemasukan nilai pada matriks 1 terselesaikan, akan dilanjutkan dengan matriks 2 karena setelah menjalankan proses pertama, nilai dari variabel a akan bertambah.

		void matrixprint(int param){

			if (param == 0){

				for(int i = 0; i < col1; i++){
					for(int j = 0; j < col3; j++){
						printf("%d ", temp[i][j]);
					}
					printf("\n");
				}

				printf("\n");

			}

			else if (param == 1){

				for(int i = 0; i < col1; i++){
					for(int j = 0; j < col2; j++){
						// printf("Halo ");
						printf("%d ", arr1[i][j]);
					}
					printf("\n");
				}

				printf("\n");
			}

			else if (param == 2){

				for(int i = 0; i < col2; i++){
					for(int j = 0; j < col3; j++){
						printf("%d ", arr2[i][j]);
					}

					printf("\n");
				}

				printf("\n");
			}

			else {
				matrixprint(1); matrixprint(2); matrixprint(0);
			}

		}

* Fungsi `matrixprint` digunakan untuk mengeluarkan hasil perkalian antar matriks.
* Fungsi ini memerlukan sebuah parameter yang digunakan sebagai percabangan kondisi untuk nilai matriks apa yang akan diprint.
* Kondisi pertama dimana parameter bernilai 0 akan mengeluarkan output dari matriks `temp` yang menyimpan nilai matriks berukuran 4x6.
* Kondisi kedua dimana parameter bernilai 1 akan mengeluarkan output dari matriks `arr1` yang menyimpan nilai matriks berukuran 4x3.
* Kondisi ketiga dimana parameter bernilai 2 akan mengeluarkan output dari matriks `arr2` yang menyimpan nilai matriks berukuran 3x6.
* Kondisi lain dimana parameter bernilai selain 0,1,2 akan mengeluarkan output dari ketiga parameter di atasnya.

		void multiply(int mat1[col1][col2], int mat2[col2][col3], int res[col1][col3]){

		    int i, j, k;
		    for (i = 0; i < col1; i++) {
			for (j = 0; j < col3; j++) {
			    temp[i][j] = 0;
						// printf("%d ", arr1);
			    for (k = 0; k < col2; k++){
				printf("%d += %d * %d", temp[i][j], mat1[i][k], mat2[k][j]);
						if (k < col2) printf(" = ");
				temp[i][j] += mat1[i][k] * mat2[k][j];
						printf("%d", temp[i][j]);
						if (k < col2-1) printf("\n");
						}
					printf("\n");
			}
				printf("\n");
		    }
			printf("\n");
		}

* Fungsi `multiply` ini digunakan untuk melakukan perkalian dari matriks 1 dan matriks 2.
* Perkalian dilakukan dengan iterasi baris dari matriks 1 dan kolom dari matriks 2 lalu dikalikan. Hasil perkalian akan dimasukkan ke variabel `temp[i][j]` dan selanjutnya ditambahkan dengan perkalian dari baris dan kolom yang sama. Setelah perkalian dari baris dan kolom yang sama selesai, program akan melanjutkan iterasi ke baris dan kolom selanjutnya.

		void main()
		{
		    key_t key = 1234;
		    int	*value, *andri;
			*andri = 0;

		    int shmid = shmget(key, sizeof(int), IPC_CREAT | 0666);
		    value = shmat(shmid, NULL, 0);
			andri = shmat(shmid, NULL, 0);

		    matrixprep();
			multiply(arr1, arr2, temp);
			matrixprint(3);

		    for (int i = 0; i < col1; i++){
			for (int j = 0; j < col3; j++){
			    value[i*col3+j] = temp[i][j];
			}
		    }

			printf("Value: \n");
			for (int i = 0; i < col1; i++){
				for (int j = 0; j < col3; j++){
			    		printf("%d ", value[i*col3+j]);
			}
				printf("\n");
		    }
			printf("\n");

			printf("\nhampir jalan\n");

			while(1){
				if (*andri == 5) {printf("andri received = %d\n", *andri); *andri = 0; break;}
				printf("waiting for andri...\n");
				sleep(1);
			}


		    shmdt(value);
			shmdt(andri);
		    shmctl(shmid, IPC_RMID, NULL);
		}
		
* Pada fungsi main yang pertama kami lakukan adalah mendeklarasikan sebuah key yang dapat digunakan untuk meminta resource shared memory. Kami juga membuat pointer `andri` dan `value` untuk shared memory nantinya.
* `int shmid = shmget(key, sizeof(int), IPC_CREAT | 0666);` digunakan untuk shared memory identifier yang memuat beberapa parameter untuk membuat shared memory. 
*  Berikutnya baik pada pointer `value` dan `andri` dideklarasikan dengan perintah `shmat(shmid, NULL, 0)` untuk menuju ke alamat shared memory yang sudah diindentifikasi oleh `shmid`.
*  Dilanjutkan dengan memanggil fungsi `matrixprep`, `multiply`, dan `matrixprint` dengan parameter 3.
*  Selanjutnya hasil perkalian kedua matriks yang disimpan pada variabel `temp` akan dimasukkan ke pointer `value`.
*  Langkah selanjutnya adalah melakukan perulangan `while` untuk menunggu terkoneksi dengan shared memory pada soal 2b. Jika masih menunggu untuk terkoneksi maka program akan mengeluarkan output `printf("waiting for andri...\n");` yang akan berulang setiap 1 detik.
*  `shmdt(value)` dan  `shmdt(andri)` digunakan untuk melepaskan segmen shared memory yang digunakan.

#### b. Membuat program perhitungan matriks baru dengan inputan matriks dari user dan inputan matris dari soal 2.a. Perhitungannya adalah setiap cel yang berasal dari matriks A menjadi angka untuk faktorial, lalu cel dari matriks B menjadi batas maksimal faktorialnya (dari paling besar ke paling kecil).

	#include <stdio.h>
	#include <sys/ipc.h>
	#include <sys/shm.h>
	#include <unistd.h>
	#include <pthread.h>
	#define col1 4
	#define col2 3
	#define col3 6
	#define zero 0
	
	pthread_mutex_t softex;
	int *value, *andri, goal1, goal3, muteks;
	int hasil[col1*col3];
	int baru[col1*col3];
	int final[col1*col3];
	int argt[col1*col3];
	pthread_t thread_id[col1*col3];
	
* Pertama kami menginputkan library yang akan digunakan, setelah itu men-define beberapa variabel konstan untuk membuat ukuran matriks sesuai dengan yang dibutuhkan.
* Selanjutnya kami membuat sebuah muteks `pthread_mutex_t softex` dikarenakan program ini akan mengakses shared memory. Lalu variabel-variabel pointer dan array lain yang sekiranya dibutuhkan untuk menjalankan program. `pthread_t thread_id[col1*col3]` digunakan untuk membuat program matriks 4x6.

		int fact(int num){
			int i = 1;
			while (num > 1){
				i *= num;
				num--;
			}
			return i;
		}

* Fungsi `fact` dibuat untuk menampung sebuah parameter bertipe integer yang akan digunakan untuk melakukan faktorial dari perhitungan matriks.

		void matrixprep(){
			int rem = col1*col3;;
			for(int i = 0; i < col1; i++){
				for(int j = 0; j < col3; j++){
					printf("Number to input remaining: %d\n", rem-(i*col3+j));
					scanf("%d", &baru[i*col3+j]);
					printf("\n");
				}
			}
		}
		
* Fungsi `matrixprep` digunakan untuk menyimpan nilai matriks yang telah diinputkan oleh user. Selanjutnya nilai-nilai inputan tadi akan dimasukkan ke dalam variabel array `baru`.

		void* factpthread3(void *z){

			muteks = 0;

			int *goal2 = (int*) z;
			printf("goal2: %d\n", *goal2);
			printf("%d vs %d\n", hasil[*goal2], baru[*goal2]);

			if (hasil[*goal2] >= baru[*goal2]){

				final[*goal2] += fact(hasil[*goal2])/fact(hasil[*goal2]-baru[*goal2]);
				printf("final %d: %d\n", *goal2, final[*goal2]);

			}

			else if (hasil[*goal2] < baru[*goal2]){

				final[*goal2] += fact(hasil[*goal2]);
				printf("final %d: %d\n", *goal2, final[*goal2]);

			}
			else final[*goal2] += 0;
			muteks = 1;
			printf("/n");

		}

* Selanjutnya adalah membuat fungsi thread `factpthread3` untuk perhitungan setiap cell dalam matriks. Selanjutnya kami menginisiasi variabel muteks yang digunakan untuk mencegah threadnya saling berebut memori.
* Dilakukan sebuah pendeklarasian variabel pointer bernama `goal2` yang digunakan untuk mengakses nilai integer per tiap cell dari matriks.
* Setelah itu dikeluarkan output untuk mengeluarkan nilai tiap cell dari matriks pertama dan matriks kedua.
* Selanjutnya diberikan sebuah kondisi dimana jika nilai cell dari matriks pertama lebih besar sama dengan matriks kedua, maka nilai variabel array `final` akan menyimpan nilai perhitungan sesuai yang diberikan di soal yaitu memfaktorialkan nilai cell dari matriks 1 dibagi dengan pengurangan nilai cell matriks 1 dan 2 di dalam fungsi `fact` kemudian hasil dari perhitungan akan dikeluarkan.
* Kondisi kedua dimana nilai cell dari matriks 1 kurang dari nilai cell matriks 2, maka perhitungan hanya akan melakukan faktorial pada nilai dari cell matriks 1.
* Kondisi terkahir adalah jika nilai cell yang diakses oleh pointer `goal2` bernilai 0, maka hasilnya juga akan bernilai 0 atau tidak terjadi perhitungan.

		void matrixprint(int param){

			if (param == 0){
				printf("Value:\n");
				for(int i = 0; i < col1; i++){
					for(int j = 0; j < col3; j++){
						printf("%d ", value[i*col3+j]);
					}
					printf("\n");
				}

				printf("\n");

			}

			else if (param == 1){
				printf("Baru:\n");
				for(int i = 0; i < col1; i++){
					for(int j = 0; j < col3; j++){
						printf("%d ", baru[i*col3+j]);
					}
					printf("\n");
				}

				printf("\n");
			}

			else if (param == 2){
				printf("Final:\n");
				for(int i = 0; i < col1; i++){
					for(int j = 0; j < col3; j++){
						// printf("Halo ");
						printf("%d ", final[i*col3+j]);
					}
					printf("\n");
				}

				printf("\n");
			}

			else {
				matrixprint(0); matrixprint(1); matrixprint(2);
			}

		}

* Fungsi `matrixprint` digunakan untuk mengeluarkan output matriks yang sudah diperhitungkan. Fungsi ini akan mengambil nilai parameter dari variabel integer bernama `param`.
* Kondisi pertama jika parameter bernilai 0, maka fungsi akan mengeluarkan nilai matriks dari variabel array `value` dimana variabel ini diambil dari soal no 2.a
* Kondisi kedua jika parameter bernilai 1, maka fungsi akan mengeluarkan nilai matriks dari variabel array `baru`.
* Kondisi ketiga jika parameter bernilai 2, maka fungsi akan mengeluarkan nilai matriks dari variabel array `final`.
* Ketika tidak mendapat input selain ketiga parameter di atas, maka fungsi akan mengeluarkan ketiga kondisi secara urut.


			void main()
		{
		    key_t key = 1234;

		    int shmid = shmget(key, sizeof(int), IPC_CREAT | 0666);
		    value = shmat(shmid, NULL, 0);
		    andri = shmat(shmid, NULL, 0);

			matrixprep();

		    for (int i = 0; i < col1; i++){
			for (int j = 0; j < col3; j++){
			    hasil[i*col3+j] = value[i*col3+j];
			}
		    }

			printf("\n");

			for (int i = 0; i < col1; i++){
			for (int j = 0; j < col3; j++){

					goal1 = i*col3+j;
					printf("goal1: %d\n", goal1);

					if (!(goal3 = pthread_create(&(thread_id[goal1]), NULL, &factpthread3, (void *) &goal1)))
				pthread_join(thread_id[i*col3+j], NULL);

			}
		    }

			matrixprint(3);

			*andri = 5;

		    shmdt(value);
			shmdt(andri);
		    shmctl(shmid, IPC_RMID, NULL);
		}
		
* `int shmid = shmget(key, sizeof(int), IPC_CREAT | 0666);` digunakan untuk shared memory identifier yang memuat beberapa parameter untuk membuat shared memory. 
* Berikutnya baik pada variabel `value` dan `andri` dideklarasikan dengan perintah `shmat(shmid, NULL, 0)` untuk mengakses alamat shared memory yang sudah diindentifikasi oleh `shmid`.
* Dilanjutkan dengan memanggil fungsi `matrixprep` untuk mendapatkan inputan nilai per cell matriks dari user. 
* Selanjutnya adalah melakukan perbandingan nilai cell pada matriks lama dan matriks baru dan dijalankan perhitungannya menggunakan thread.
* Jika semua proses sudah selesai maka pointer `andri` akan bernilai lima sehingga program pada soal 1.a bisa selesai.
* `shmdt(value)` dan  `shmdt(andri)` digunakan untuk melepaskan segmen shared memory yang digunakan.
		
#### c. Membuat program untuk mengecek 5 proses teratas menggunakan pipe dengan command `ps aux | sort -nrk 3,3 | head -5`	
		
## Soal 3 : Membuat opsi untuk menjalankan program

	#include <stdlib.h>
	#include <stdio.h>
	#include <sys/types.h>
	#include <sys/wait.h>
	#include <unistd.h>

	int pid, status;
	int pipe1[2];
	int pipe2[2];

* Pertama kami menginputkan beberapa library yang akan digunakan serta deklarasi `pipe1` dan `pipe2` yang akan digunakan sebagai pipes.

		int main() {
			printf("Halo lur!\n");
		if (pipe(pipe1) == -1){printf("Andri"); exit(1);}
		if (pipe(pipe2) == -1){printf("Andri lagi"); exit(1);}

* Di fungsi `main`, yang pertama kali kami lakukan adalah melakukan pengecekan apakah pipe berhasil dijalankan atau tidak. Jika `pipe1` gagal, maka program akan mengeluarkan output "Andri" dan jika `pipe2` gagal, maka program akan mengeluarkan output "Andri lagi".

			if ((fork()) == 0) {


				close(pipe1[0]);
				dup2(pipe1[1], STDOUT_FILENO);

				char *argv1[] = {"ps", "-aux", NULL};
				execv("/bin/ps", argv1);
			}
			
* Di child process  pertama ini dibuat file STDOUT dan eksekusi `ps`.

			else {

				while(wait(&status) > 0);

				if ((fork()) == 0) {
				close(pipe1[1]);
				close(pipe2[0]);
				// printf("---masuk child 2\n");
				dup2(pipe1[0], STDIN_FILENO);
				dup2(pipe2[1], STDOUT_FILENO);

				char *argv1[] = {"sort", "-nrk", "3,3", NULL};
				execv("/usr/bin/sort", argv1);

				} 
				
* Di child process ini dibuat file STDIN dan STDOUT dan eksekusi `sort`.

				else {

					// // input from pipe1
					close(pipe1[0]);
					close(pipe1[1]);
					close(pipe2[1]);
					while(wait(&status) > 0);
					// printf("---masuk parent");
					dup2(pipe2[0], STDIN_FILENO);
					close(pipe2[0]);

					char *argv1[] = {"head", "-5", NULL};
					execv("/usr/bin/head", argv1);

				}
			}
		}

* Masuk ke parent proses dibuat file STDIN dan eksekusi `head`.

#### a. opsi -f: digunakan untuk mengkategorikan file untuk file-file tertentu sebagai argumen.
#### b. opsi -d: digunakan untuk mengkategorikan file dalam directory tertentu sebagai argumen.
#### c. opsi * : digunakan untuk mengkategorikan file untuk seluruh file dalam current working directory saat program dijalankan.
  
		#include<sys/stat.h>
		#include<stdio.h>
		#include<unistd.h>
		#include<ctype.h>
		#include<string.h>
		#include<pthread.h>
		#include<dirent.h>

		pthread_t tid[10000];
		char cwd[1000];
		char dirpath[1000];

		void *pindah(void *arg);

* Pertama kami menginputkan beberapa library yang akan digunakan. Setelah itu kami membuat thread dan beberapa variabel array, juga sebuah pointer bernama `pindah`.

		int main(int argc, char *argv[]) {
			getcwd(cwd, sizeof(cwd));
			int a;

			if(argc < 2) {
				printf("argumen invalid");
			}

			if(strcmp(argv[1], "-f") == 0) {
				for(int i=2; i<argc; i++) {
					pthread_create(&tid[i], NULL, pindah, (void *)argv[i]);
				}
				for(int j=2; j<argc; j++) {
					a = pthread_join(tid[j], NULL);
					if(a == 0) {
						printf("File %d: Berhasil Dikategorikan\n", j-1);
					}
					else {
						printf("File %d: Sad, gagal :(\n", j-1);
					}
				}
			}
			
* Di fungsi `main` diperlukan dua parameter yaitu sebuah variabel dan sebuah pointer.
* Pertama dilakukan pengambilan path dari direktori saat ini menggunakan fungsi `getcwd` yang menyimpan path direktori saat ini di dalam sebuah buffer yaitu `cwd`
* Langkah kedua adalah mengecek apakah argumen yang diberikan sudah benar atau masih salah menggunakan `if(argc < 2)` karena jumlah argumen yang diberikan seharusnya lebih dari 2.
* Selanjutnya adalah membuat sebuah perulangan `for(int i=2; i<argc; i++)` dengan opsi `-f` yang akan membuat thread untuk mengategorikan sejumlah file. Selanjutnya thread akan ditunggu hingga selesai menggunakan `a = pthread_join(tid[j], NULL);` dan jika thread pertama berhasil dijalankan maka akan mengeluarkan output "Berhasil Dikategorikan" dan melanjutkan ke perulangan thread berikutnya. Jika thread pertama tidak berhasil dijalankan, maka akan mengeluarkan output "Sad, gagal :("


			else {
				DIR *dir;
				struct dirent *tmp;
				int i=0;
				if(strcmp(argv[1], "-d") == 0) {
					dir = opendir(argv[2]);
					strcpy(dirpath, argv[2]);
				}

* Selanjutnya adalah membuat direktori baru sebagai direktori pengkategorian untuk masing-masing ekstensi file.
* Kondisi kedua adalah jika opsi `-d` diinputkan, maka program akan membuka direktori yang dimaksud pada variabel `dir` dengan fungsi `opendir(argv[2])` dan directory path yang diinputkan pada parameter ke-2 akan disimpan pada variabel array `dirpath` dengan fungsi `strcpy(dirpath, argv[2])`

		else if((argv[1][0]=='*') && (strlen(argv[1])==1)) {
							dir = opendir(cwd);
						}
						else {
							printf("argumen invalid");
						}

						while((dir!=NULL) && (tmp=readdir(dir))) {
					    if(strcmp(tmp->d_name, ".")==0 || strcmp(tmp->d_name, "..")==0 || strcmp(tmp->d_name, "soal3.c")==0 || strcmp(tmp->d_name, "soal3")==0 || tmp->d_type==DT_DIR) 
							continue;

					    pthread_create(&tid[i], NULL, pindah, tmp->d_name);
					    i++;
					}
					for(int j=0; j<i; j++)
						a = pthread_join(tid[j], NULL);
						if(a == 0) {
							printf("Direktori sukses disimpan!\n");
						}
						else {
							printf("Yah, gagal disimpan :(\n");
						}
					closedir(dir);
					}
					return 0;
				}

* Selanjutnya jika opsi yang diinputkan adalah `*` maka variabel `dir` akan membuka working directory yang telah disimpan pada variabel buffer `cwd`. Jika gagal dijalankan/ argumen yang diinputkan tidak memenuhi, maka akan mengeluarkan output "argumen invalid".
* Berikutnya dilakukan pengeliminasian direktori dan file `soal3.c` serta file `soal3` dengan menggunakan perintah berikut `if(strcmp(tmp->d_name, ".")==0 || strcmp(tmp->d_name, "..")==0 || strcmp(tmp->d_name, "soal3.c")==0 || strcmp(tmp->d_name, "soal3")==0 || tmp->d_type==DT_DIR) `. Jika pengeliminasian ini berhasil, maka dilanjutkan dengan pembuatan thread untuk mengkategorikan seluruh file yang ada di working directory.
* Selanjutnya thread akan ditunggu hingga selesai. Jika berhasil dijalankan maka akan keluar output "Direktori sukses disimpan!" dan jika gagal maka akan mengeluarkan output "Yah, gagal disimpan"


		void *pindah(void *arg) {
			char *fpath = (char *)arg;
			char *hid = NULL;
			hid = strchr(fpath, '.');
			char *ext = NULL;
			ext = strrchr(fpath, '.');

			char temp[1000];
			if(ext) {
				ext++;
				for(int i=0;i<strlen(ext);i++) {
					temp[i] = tolower(ext[i]);
				}
			}
			else if(hid) {
				strcpy(temp, "Hidden");
			}
			else strcpy(temp, "Unknown");

			char *fname = NULL;
			fname = strrchr(fpath, '/');
			if(fname) {
				fname++;
			}
			else fname = fpath;

			char dirname[1000];
			strcpy(dirname, cwd);
			strcat(dirname, "/");
			strcat(dirname, temp);

			mkdir(dirname, S_IRWXU);

			if(strlen(dirpath) > 1) {
				char fullname[1000];
				strcpy(fullname, dirpath);
				strcat(fullname, "/");
				strcat(fullname, fname);

				strcat(dirname, "/");
				strcat(dirname, fname);

				rename(fullname, dirname);
			}
			else {					
				strcat(dirname, "/");
				strcat(dirname, fname);

				rename(fpath, dirname);
			}
		}

* Fungsi `pindah` digunakan untuk memindahkan file ke direktori ekstensi masing-masing. Pertama kami melakukan pendeklarasian beberapa variabel pointer yang akan digunakan untuk menjadi parameter jalannya suatu kondisi. Pointer `hid` dan `ext` menyimpan delimiter `.` untuk memastikan bahwa argumen pada `fpath` menyimpan sebuah file. Perbedaannya hanya cara pengecekan saja, jika `ext` mengecek dari kemunculan terakhir, maka `hid` mengecek dari kemunculan terdepan.
* `ext` akan menjadi sebuah kondisi untuk mengubah huruf pada path direktori menjadi lower case menggunakan fungsi `tolower`. Kondisi lain akan memindahkan file `hidden` dan `unknown` ke variabel `temp`.
* Selanjutnya adalah membuat path dari direktori extension yang akan dibuat menggunakan fungsi `strcpy` dan `strcat`
* Lalu ada fungsi `mkdir(dirname, S_IRWXU);` untuk membuat direktori baru.
* Berikutnya ada kondisi untuk memenuhi opsi `-d`  yaitu pertama membuat array `fullname` untuk menyimpan path yang akan dibuat dan selanjutnya memindahkan file yang ada pada `fullname` ke `dirname`.
* Kondisi lain adalah untuk memnuhi opsi `*`
