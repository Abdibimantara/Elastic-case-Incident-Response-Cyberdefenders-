# Elastic-case-Incident-Response-Cyberdefenders
Berikut adalah writeup yang saya tulis mengenai Elastic Case - Incident Response yang berasal dari platform CyberDefender.org.

![image](https://user-images.githubusercontent.com/43168046/182867474-4b34cd4e-e5f4-4874-a0fa-35a384d5d4f1.png)

## Source Link 
https://cyberdefenders.org/blueteam-ctf-challenges/90

## Skenario challenge : 
Seorang penyerang dapat mengelabui seorang karyawan agar mengunduh file yang mencurigakan dan menjalankannya. Penyerang mengkompromikan sistem, bersama dengan itu, Tim Keamanan tidak memperbarui sebagian besar sistem. Penyerang dapat berporos ke sistem lain dan membahayakan perusahaan. Sebagai analis SOC, Anda ditugaskan untuk menyelidiki insiden tersebut menggunakan Elastic sebagai alat SIEM dan membantu tim untuk mengusir penyerang.

## Tools yang digunakan
- Virtual Box
- Elastic

## Challenge
### 1. Who downloads the malicious file which has a double extension ?
Langkah pertama yang harus dilakukan adalah dengan masuk kedalam menu Security dan pilih menu alert.

![image](https://user-images.githubusercontent.com/43168046/182867933-a27eb657-05e6-4ab4-ab8d-5b4d852a3795.png)

Pada menu alert, akan terlihat data pada rentang waktu tertentu. Disii saya menyetel waktu pertanggal 8 januari sampai dengan 12 Februari. Untuk mempermudah menemukan data yang diinginkan, terdapat bebera filer yang dapat digunakan. Dikarenakan kita focus pada malware, maka saya akan menggunakan filter “File Name Type”, dan fokus saya pada Acount_details.pdf.exe.  Filter tersebut juga diikutin dengan filter kedua yaitu “Event.Action” dan fokus saya pada “Creation”.

![image](https://user-images.githubusercontent.com/43168046/182868242-f06cafe8-a366-4959-8aa2-da521942bbae.png)

Berdasarkan kedua filter tersebut, terdapat 5 data berhasil kami temukan. Dengan rincian 2 username yang kami dapatkan yaitu “cybery” dan “ahmed”. Tentu saja kami Kembali menyelidiki data tersebut berdasarkan rentang waktu. Dimana kami melihat username “ahmed” menjadi yang pertama kali. Sehingga kami pun membuka table dari data username "ahmed".

![image](https://user-images.githubusercontent.com/43168046/182868310-7e73b930-940d-43d4-a823-9804c61de199.png)

Melihat data pada table tersebut. Terlihat bahwa user ahmed melakukan proses download file \Acount_details.pdf.exe menggunakan browser microsoft edge. Berdasarkan data tersebut, kami menyimpulkan bahwa username “ahmed” telah melakukan proses downloading pertama dan menyebarkan nya ke yang lain.

### 2. What is the hostname he was using?
Berdasarkan data yang saya temukan sebelumnya, diketahui bahwa username “ahmed” yang telah melakukan proses downloading file terindikasi malware. Melalui table pada elastic, dengan keyword “host.hostname” kami mendapati data hostname yaitu “DESKTOP-Q1SL9P2”.

![image](https://user-images.githubusercontent.com/43168046/182873696-be99d247-0f85-4184-bc1c-a5c4c21ceef5.png)

### 3. What is the name of the malicious file?
Pada data table di elastic alert. Kami juga mendapati data nam filenya dengan menggunakan keyword ”file.name”. Dimana nama file tersebut adalah “Acount_details .pdf.exe”.

![image](https://user-images.githubusercontent.com/43168046/182873922-a4e7f56b-c2ce-47f1-b5db-08f184ca11e9.png)

Terlihat dari data diatas, file yang didownload oleh username “ahmad” yaitu “Acount_details.pdf.exe”. Selain itu file tersebut terbaca oleh elastic memiliki 2 ekstensi file yaitu pdf dan exe. Terlampir juga hash 256 nya yaitu “3e7295a9c21d288772ba3780a648af75 15e6c728e7d4b783deb24e86e9d46 79a”

## 4. What is the attacker's IP address ?
Diketahui bahwa username “ahmed” Memiliki ip address “192.168.10.10”. untuk mengetahui ip address dari attacker, saya menggunakan fitur Analyze yang terlihat pada gambar dibawah. 

![image](https://user-images.githubusercontent.com/43168046/182874121-e6ace6b9-a0f7-449f-a4af-294ea9bb5e40.png)

Terlihat pada data dibawah, terdapat 11 network pada file “Acount_details.pdf.exe”. Setelah kami membukanya, kami mendapati ip destination lainnya, yang kami curigai sebagai ip attacker yaitu “192.168.1.10”. 

![image](https://user-images.githubusercontent.com/43168046/182874207-bd002cb6-4cd4-417a-8417-bac9aed74af9.png)

## 5. Another user with high privilege runs the same malicious file. What is the username?
Berdasarkan pada penggunaan filter “File Name Type”, dan fokus saya pada Acount_details.pdf.exe.  Filter tersebut juga diikutin dengan filter kedua yaitu “Event.Action” dan fokus saya pada “Creation”.  Kami mendapat username lainnya yang terlihat mencoba mengakses file malicious tersebut, yaitu “cybery”.

![image](https://user-images.githubusercontent.com/43168046/182874383-6fce03c0-d3af-4a98-8dfa-a5a8af279ba6.png)




