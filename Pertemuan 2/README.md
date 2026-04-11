# Pertemuan 2

> Pertanyaan

## 2.5.4 Pertanyaan Percobaan 2A: Seven Segment

1. Gambarkan rangkaian schematic yang digunakan pada percobaan!

![Skematik 2.5](skematik-2.5.png)

2. Apa yang terjadi jika nilai num lebih dari 15?

   > Pattren 7 Segment yang di ambil akan error. Errornya LED akan menampilkan angka yang tidak jelas dikarenakan pattern yang didapat berupa undefined karena variabel `digitPattren` hanya memiliki 16 item (index 1-15).

3. Apa program ini menggunakan common cathode atau common anode? Jelaskan alasannya!

   > Pada program ini, menggunakan 7 Segment common anode karena led pada segment baru bisa menyala jika pin common dari segmen dihubungkan ke pin 5V

4. Modifikasi program agar tampilan berjalan dari F ke 0 dan berikan penjelasan di setiap baris kode nya dalam bentuk README.md!

   ```c++
   const int segmentPins[8] = {7, 6, 5, 11, 10, 8, 9, 4}; // Pin 7-Segment

   byte digitPattern[16][8] = { // Pattren
        {1,1,1,1,1,1,0,0}, //0
        {0,1,1,0,0,0,0,0}, //1
        {1,1,0,1,1,0,1,0}, //2
        {1,1,1,1,0,0,1,0}, //3
        {0,1,1,0,0,1,1,0}, //4
        {1,0,1,1,0,1,1,0}, //5
        {1,0,1,1,1,1,1,0}, //6
        {1,1,1,0,0,0,0,0}, //7
        {1,1,1,1,1,1,1,0}, //8
        {1,1,1,1,0,1,1,0}, //9
        {1,1,1,0,1,1,1,0}, //A
        {0,0,1,1,1,1,1,0}, //b
        {1,0,0,1,1,1,0,0}, //C
        {0,1,1,1,1,0,1,0}, //d
        {1,0,0,1,1,1,1,0}, //E
        {1,0,0,0,1,1,1,0}  //F
   };

   // Fungsi tampil digit (dibalik untuk CA)
   void displayDigit(int num)
   {
       for(int i=0; i<8; i++)
           digitalWrite(segmentPins[i], !digitPattern[num][i]); // <-- dibalik, menyalakan pin LED pada 7-segmen
   }

   void setup() // Fungsi yang akan dijalankan sekali pada saat Arduino pertama kali dinyalakan
   {
       for(int i=0; i<8; i++) // Menginisiasi pin yang akan digunakan
           pinMode(segmentPins[i], OUTPUT);
   }

   void loop() // Fungsi yang akan di jalankan secara terus menerus
   {
       for(int i=15; i>=0; i--) // Memulai dari 15 ke 0
       {
           displayDigit(i); // Menampilkan digit
           delay(1000); // Memberi delay 1 detik tiap angka
       }
   }
   ```

## 2.6.4 Pertanyaan Percobaan 2B: Kontrol Counter Dengan Push Button

1. Gambarkan rangkaian schematic yang digunakan pada percobaan!

![Skematik 2.6](skematik-2.6.png)

2. Mengapa pada push button digunakan mode INPUT_PULLUP pada Arduino Uno?

   > Hemat resistor dan mengatasi masalah 'Floating' atau sinyal tidak pasti.

3. Jika salah satu LED segmen tidak menyala, apa saja kemungkinan penyebabnya dari sisi hardware maupun software?

   > Pada sisi hardware kemungkinan penyebabnya bisa berupa pemasangan resistor yang longgar, resistor putus, LED segmen terbakar atau salah pinout. Sedangkan pada sisi software bisa berupa kesalahan logika (HIGH/LOW) saat memberikan sinyal, pin belum diatur atau salah nomor pin.

4. Modifikasi rangkaian dan program dengan dua push button yang berfungsi sebagai penambahan (increment) dan pengurangan (decrement) pada sistem counter dan berikan penjelasan disetiap baris kodenya dalam bentuk README.md!

   ![Rangkaian dengan 2 button](rangkaian-2.6_2btn.png)

   ```c++
   const int segmentPins[8] = {7, 6, 5, 11, 10, 8, 9, 4}; // Pin 7-Segment
   const int buttonPin[2] = {3,2}; // Pin tombol

   int counter = 0; // variabel penampung angka

   bool lastButtonState[2] = {HIGH, HIGH}; // state terakhir dari button

   // Pola digit 0-F
   byte digitPattern[16][8] = {
   {1,1,1,1,1,1,0,0}, //0
   {0,1,1,0,0,0,0,0}, //1
   {1,1,0,1,1,0,1,0}, //2
   {1,1,1,1,0,0,1,0}, //3
   {0,1,1,0,0,1,1,0}, //4
   {1,0,1,1,0,1,1,0}, //5
   {1,0,1,1,1,1,1,0}, //6
   {1,1,1,0,0,0,0,0}, //7
   {1,1,1,1,1,1,1,0}, //8
   {1,1,1,1,0,1,1,0}, //9
   {1,1,1,0,1,1,1,0}, //A
   {0,0,1,1,1,1,1,0}, //b
   {1,0,0,1,1,1,0,0}, //C
   {0,1,1,1,1,0,1,0}, //d
   {1,0,0,1,1,1,1,0}, //E
   {1,0,0,0,1,1,1,0}  //F
   };

   void displayDigit(int num) // Menamplikan digit
   {
       for(int i=0; i<8; i++)
           digitalWrite(segmentPins[i], !digitPattern[num][i]); // Menyalakan pin LED 7-Segment
   }

   void setup()
   {
       for(int i=0; i<8; i++) // Menginisiasi pin 7-Segment
           pinMode(segmentPins[i], OUTPUT);

       for (int i = 2; i < 4; i++) // Menginisiasi pin button
           pinMode(i, INPUT_PULLUP);

       displayDigit(counter); // Menampilkan angka 0 untuk inisisasi
   }

   void loop()
   {
       for (int i = 0; i < 2; i++) { // Loop menyesuaikan total button
           bool currentState = digitalRead(buttonPin[i]); // Membaca state button i

           if (lastButtonState[i] == HIGH && currentState == LOW) { // Membandingkan state lama dengan sekarang
               if (i == 0) counter = (counter + 1) % 16;        // Increment 0-15
               else        counter = (counter - 1 < 0) ? 15 : counter - 1; // Decrement 0-15

               displayDigit(counter);
               delay(200); // Debounce sederhana
               }
           lastButtonState[i] = currentState; // Mengubah state terakhir dari button i dengan state sekarang
       }
   }
   ```
