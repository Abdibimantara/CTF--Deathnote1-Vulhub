# CTF-Deathnote1-Vulhub

![image](https://user-images.githubusercontent.com/43168046/172059966-3b272f71-a12a-4046-9e96-17119f8f872a.png)

Pada tulisan kali ini kita akan membahas mengenai CTF (capture the flag) “Deathnote versi 1” yang berasal dari Vulnhub.  Untuk akses mesinnya dapat diunduh pada website resmi vulhub https://www.vulnhub.com/ .  Mesin deathnote tersebut termasuk dalam kategori mudah, sehingga sangat disarankan untuk pengguna awal yang ingin mencoba duni hacking. Ukuran dari mesin tersebut juga tidaklah besar, cukup 600mb saja. Kami menyarankan untuk menggunakan virtualbox ketimbang vmware untuk menjalankan mesin tersebut.
Untuk memulai cerita dari mesin deathnote1 tersebut kita memiliki beberapa tahapan yaitu :
1.	Information Gaining (Reconnaissance)
2.	Proses eksploitasi 
3.	Akses Root

## 1.	Information Gathering (Reconnaissance)
Proses ini adalah proses dimana kita diharuskan untuk mengumpullan informasi sebanyak mungkin. Dimana informasi tersebut akan membantu kita dalam melanjutkan pada fase selanjutnya. Proses information gathering ini terbagi menjadi :
### Scanning IP target
Sebelumnya mesin deathnote1 ini memiliki konfigurasi ip DHCP, sehingga bisa saja hasil scanning ip kita bakal berbeda. Pada kali ini saya menggunakan tool arp-scan.

![image](https://user-images.githubusercontent.com/43168046/172060098-707565cc-bfbe-4418-8704-f1d51440f573.png)

Berdasarkan gambar diatas, terdapat 3 ip. Dimana ip pertama adalah ip dari gateway kami, ip kedua adalah ip dari computer utama yang menjalankan virtualbox, terakhir adalah ip target (deathnote1).

### Scanning Port target
Langkah selanjutnya adalah melakukan scanning port, dengan tujuan untuk mengetahui port berapa dan layanan apa yang terbuka pada mesin tersebut.  Langkah ini kita menggunakan bantuan dari tools Nmap.

![image](https://user-images.githubusercontent.com/43168046/172060143-ba59e90d-cc1b-43ab-88a4-559f93c09c31.png)

Berdasarkan hasil scanning port yang dilakukan menggunakan tools Nmap, terdapat 2 port yang terbuka. Dimana port 80 sebagai http dan port 22 sebagai ssh. Melalui informasi yang kami dapatkan ini, kami memutukan untuk menlajutkan mencari informasi melalui port 80 yaitu layanan http (website)

### Information Gaining from website
Kami mencoba mencoba mengkases http://192.168.43.229:80. Namun saat kami membuka url tersebut mealui browser, url tersebut tidak dapat diakses. 

![image](https://user-images.githubusercontent.com/43168046/172060206-316f8d4e-6a5e-4b7d-8550-9e27f077fb05.png)

Namun, kami mendapati url tersebut melakukan redirect ke url lainnnya yaitu http://deathnote.vuln/wordpress/  . Sehingga kami pun memahami bahwa link IP tersebut belum terkonfigurasi terhadap domain deathnote.vuln. Sehingga, kami mencoba untuk mengkonfigurasi terhadap /ect/hosts.

![image](https://user-images.githubusercontent.com/43168046/172060215-047c7884-69c9-475e-9580-9616b6a775a9.png)

Setelah kami berhasil mengkonfigurasi ip dan domain tersebut, website yang ingin kami tuju telah berhasil terbuka. 

![image](https://user-images.githubusercontent.com/43168046/172060221-5d448488-c746-4699-8c5f-1d9f1c083bfc.png)

Kami melanjutkan investgasi pada halaman website tersebut. Berikut adalah bebera informasi yang kami rasa berguna untuk fase selanjutnya

![image](https://user-images.githubusercontent.com/43168046/172060234-f46a43b6-edeb-45e6-bd95-90b43e30fc4e.png)

![image](https://user-images.githubusercontent.com/43168046/172060238-9978511a-a308-476e-b569-9fb29609b85a.png)

Informasi yang kami dapatkan berpa beberapa nama yang kami curigai sebagai username yaitu Kira dan L. Kami juga mendapakan kalimat yang kami rasa cukup berbeda dengan yang lain yaitu “iamjustic3”. Lalu kami juga menemukan link redirection saat kami menekan tombol hint yang berisi pesan “find notes.txt”

## Akses web admin
Dikarenakan website tersebut terbuat dari wordpress, sehingga kita dapat dengan mudah mencoba akses login. Disini indikasi kredential login yang kami dapatkan adalah
User : Kira / L
Password : iamjustic3

![image](https://user-images.githubusercontent.com/43168046/172060282-846fcde1-9305-4cc5-bb6b-a0d2a57d6601.png)

Setelah melakukan bebera percobaan, kami mendapati bahwa akses username yang dipakai adalah kira. Selanjutnya kami mecoba mengakses akses panel dashboard wordpress untuk menemukan informasi penting lainnya. 

Kami teringat untuk memeriksa file notes.txt. Kami pun menemukan file tersebut terletak pada submenu media 

![image](https://user-images.githubusercontent.com/43168046/172060290-a7eadeec-54fd-4176-8859-8f3309067a60.png)

Kami tertarik untuk membuka dan melihat isi dari file tersebut, dan setelah melihatnya kami dapat berasumsi bahwa list tersebut dapat digunakan sebagai list dictionary password. 

![image](https://user-images.githubusercontent.com/43168046/172060297-9065438f-504c-458e-afc8-118ea2965dbe.png)

## 2.	Proses Eksploitasi

### Bruteforce Akses login SSH
Seperti yang kami sebeutkan sebelumnya, kami menyakitin file notes.txt sebagai password dan Kira ataupun L sebagai username. Untuk membantu kami dalam proses penebakan ini, kami menggunakan tools hydra.

![image](https://user-images.githubusercontent.com/43168046/172060355-1c9a6567-41d2-426e-97e4-06b7e1390c2f.png)

Kami mendapati kredentials login ssh tersebut adalah
Username : l
Password : death4me

###	Akses SSH
Kami langsung mencoba akses target melalui ssh

![image](https://user-images.githubusercontent.com/43168046/172060372-010b9a35-aef8-48f0-b5f6-567bb806d31a.png)

Akses ssh telah berhasil. Langkah selanjutnya adalah mencoba mencari informasi mengenai informasi target dan akses root pada target

![image](https://user-images.githubusercontent.com/43168046/172060386-ab34486f-aeff-4d94-b44b-9ca64696353c.png)

Setelah mengetahui version os dan jenis os yang digunakan, kami tidak mendapatkan informasi eksploitasi. Dan user l bukanlah user root, sehingga kita mengaharuskan mencari informasi Kembali.  Setelah membongkar beberapa direktori yang ada pada mesin tersbeut kami mendapatkan informasi peting pada direktori /opt.

![image](https://user-images.githubusercontent.com/43168046/172060400-62969085-1dee-4723-8592-b2cf70811f35.png)

-	Kami mencurigai terdapat info penting pada subdirektori fake-notebook-rul
-	Setelah kami membukanya, kami mendapatkan suatu teks yang terindikasi telah di enkripsi
-	Kami juga menemukan pentujuk untuk menggunakan cyberchef

![image](https://user-images.githubusercontent.com/43168046/172060442-0cff6771-ccc4-4577-9eab-5200dca2f82d.png)

Berdasarkan bantuan tools cyberchef, kami mendapati password tersebut adalah kiraisevil

## Akses Root
Kami mencoba akses ssh menggunakan akun kira dan behasil. Kami melanjutkan ke direktori root. Dan menemukan file. Root.txt

 ![image](https://user-images.githubusercontent.com/43168046/172060480-e14bc37d-e358-4a76-a5ee-08d00c552a01.png)



