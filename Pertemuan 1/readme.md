# Pertemuan 1

> Pertanyaan:

## 1.5.4 Percobaan 1A: Percabangan

1. Pada kondisi apa program masuk ke blok if?

   > Program masuk ke blok if pada kondisi `timeDelay <= 100`. Jika nilainya sudah kurang dari 100 maka akan ada delay 3 detik baru mengatur ulang timedelay ke 100 lagi dan seterusnya.

2. Pada kondisi apa program masuk ke blok else?

   > Program masuk ke blok pada kondisi `timeDelay > 100`.

3. Apa fungsi dari perintah delay(timeDelay)?

   > Perintah `delay(timeDelay)` digunakan untuk memberikan jeda antar perintah dalam satuan miliseconds. Pada program tersebut maka itu akan memberikan efek kedip sesuai dengan variabel `timeDelay` saat itu. Kedip akan semakin cepat seiring berkurangnya nilai `timeDelay`.

4. Jika program yang dibuat memiliki alur mati → lambat → cepat → reset (mati), ubah menjadi LED tidak langsung reset → tetapi berubah dari cepat → sedang → mati.

   ```c++
   const int ledPin = 6; // nomor pin LED yang dipakau
   int timeDelay = 1000; // Variabel penampung waktu delay
   unsigned int phase = 0; // variabel penampung fase, (0 = lampat -> cepat, 1 = cepat -> sedang -> lambat), positif only

   void setup() { // fungsi yang akan di jalankan sekali saat arduino menyala
       pinMode(ledPin, OUTPUT); // menginisiasi pin LED sebagai output
   }

   void loop() { // fungsi yang akan dijalankan terus menerus oleh arduino
       digitalWrite(ledPin, HIGH); // mneyalakan LED
       delay(timeDelay); // jeda, berapa lama LED menyala

       digitalWrite(ledPin, LOW); // mematikan LED
       delay(timeDelay); // Jeda antar LED

       if (timeDelay <= 100 && phase == 0) { // mengecek apakah timeDelay kurang dari atau sama dengan 100 dan phase harus 0
           phase = 1; // mengubah ke fase kebalikannya (cepat -> sedang -> lambat)
       } else if(timeDelay > 1000 && phase == 1) { // mengecek apakah timeDelay lebih besar dari 1000 dan phase harus 1
           delay(3000); // memberika jeda setelah full satu loop (lambat -> cepat -> sedang -> lambat -> reset)
           timeDelay = 1000; // mengembalikan nilai timeDelay ke 1000
           phase = 0; // mengembalikan nilai phase ke 0
       } else { // jika kedua kondisi diatas tidak terpenuhi maka blok ini akan di jalankan
           phase == 0 ? timeDelay -= 100 : timeDelay += 200; // ternary, mengecek phase, jika phase == 0 maka timeDelay akan terus berkurang 100, sebaliknya jika phase 1 maka nilai akan semain bertambah
       }
   }
   ```

## 1.6.4 Percobaan 2A: Perulangan

1. Gambarkan rangkaian schematic 5 LED running yang digunakan pada percobaan!

![Skematik](skematik.png)

2. Jelaskan bagaimana program membuat efek LED berjalan dari kiri ke kanan!

   > Program membuat efek LED berjalan dari kiri ke kanan dengan menggunakan pengulangan `for (int ledPin = 2; ledPin < 8; ledPin++)` dalam function loop. Pada baris tersebut telihat melakukan pengulangan for dimulai dari angka 2-8 dengan menggunakan increment, dimana itu merupakan pin yang digunakan sesuai urutan (rendah - tinggi).

3. Jelaskan bagaimana program membuat LED kembali dari kanan ke kiri!

   > Program membuat efek LED kembali dari kanan ke kiri menggunakan cara yang sama dengan no 2 tadi yaitu `for (int ledPin = 7; ledPin >= 2; ledPin--)`, bedanya inisiasi awal pin lED dari 7-2 dengan menggunakan decrement.

4. Buatkan program agar LED menyala tiga LED kanan dan tiga LED kiri secara bergantian

   ```c++
   int timer = 500; // variabel penampung jeda antar lampu
   unsigned int phase = 0; // variabel penampung fase, 0 (2-4, menyala), 1 (5-7 menyala), positif only

   void setup() { // fungsi yang akan di jalankan sekali saat arduino menyala
       for (int ledPin = 2; ledPin < 8; ledPin++) { // looping dari angka 2-7, untuk menginisiasi pin
           pinMode(ledPin, OUTPUT); // menginisiasi pin led sesuai looping menjadi output
       }
   }
   void loop() { // fungsi yang akan dijalankan terus menerus oleh arduino
       phase %= 2; // penahan, agar nilai phase tidak lebih dari 1, hanya 0 & 1
       for (int ledPin = 2; ledPin < 8; ledPin++) {
           if(ledPin <= 4) { // mengecek, apakah pin saat looping kurang dari atau sama dengan 4 (bagian kanan, 2,3,4)
           digitalWrite(ledPin, phase == 0 ? HIGH : LOW); // menyalakan atau mematikan LED sesuai dengan phase
           } else { // berarti pin kiri (5,6,7)
           digitalWrite(ledPin, phase == 0 ? LOW : HIGH); // menyalakan atau mematikan LED sesuai dengan phase, tetapi berkebalikan
           }
       }
       delay(timer); // memberikan jeda antar lampu
       phase++; // melanjutkan fase
   }
   ```
