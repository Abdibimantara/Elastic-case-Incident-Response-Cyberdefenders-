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


