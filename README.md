# Useful Linux Commands

### Table of Contents

---

| No. | Topic                                                                   |
| --- | ----------------------------------------------------------------------- |
| 1   | [**General**](#General)                               |
| 2   | [**Vi**](#Vi)               |
| 3   | [**Startup**](#Startup)                           |
| 4   | [**Permission**](#Permission)                  |
| 5   | [**Network**](#Network)                         |
| 6   | [**Package Management**](#Package-management)                 |
| 7   | [**Filesystem**](#Filesystem)                              |
| 8   | [**Logs**](#Logs)                              |
| 9   | [**Process**](#Process) |
|10   | [**System**](#System)                                                   |
|11   | [**Shortcuts**](#Shortcuts)
|12   | [**Others**](#Others)

### General

1. **ls** 

   ```bash
    ls -alR
	ls -alh
	ls -ld
	ls -i1
	ls -lSrh
   ```

2. **grep** 

   ```bash
    egrep -v "^#|^$"
    egrep -vh "^#|^$" /etc/apt/sources.list.d/*
	fgrep -B5 -A5 '/xsql//query.xsql?sql=select%20username%20from ALL_USERS' access.log
	egrep -in "select|union|insert|update|delete|replace|truncate" access.log
	egrep -iHno '\ssmiles' all_dbs.sql
	egrep -iHno '\ssmiles' all_dbs.sql
	egrep -f dangerousPHPfunctions.txt -R devOps/zsi/store/zsi --exclude-dir=.svn 
	egrep -l "/var/tmp/hiddenfile|/var/tmp/hiddenfile2" pliki_*.txt
	egrep -Ha 'test' /dev/*
	grep "15/Jan/2023:07:3[0-9]" access.log
   ```
   3. **sort**
   ``` bash
   sort -u
   sort -dc(useragent)
   cat /etc/passwd | sort -t: -n -k3
   ls -l /home | sort -Mr
   ```
   4. **sed**
   ``` bash
    sed -i 's/asdf/qwerty/g' plik.txt
	sed ':a;N;$!ba;s/\n/ /g'
	sed -n '/13\/Sep\/2016:17:20/,/13\/Sep\/2016:19:46/p' access.log
	sudo sed -i '0,/#log-format/s//log-format/' /etc/goaccess/goaccess.conf
	sed -n 5,8p file
	sed -e 's/10\.0\.0\.1/xxx.xxx.xxx.xxx/' access.log
	sed -n '/15\/Jan\/2023:07/,/15\/Jan\/2023:08/p' access.log
   ```
   
   5. **awk**
   ``` bash
   awk '{print $1}'
   awk -F':' '{print $NF}' access.log
   awk -F\" '($4!="-"&&$4!~"http://localost")' access.log
   awk '/15\/Jan\/2023:07/,/15\/Jan\/2023:08/' access.log
   awk -F: '$3 == "0" {print $1}' /etc/passwd
   awk '$1==1 {print $0}'
   awk -F: '{if($3==0||$4==0){print $0}}' /etc/passwd
   ```
   
   6. **cut**
   ``` bash
   cut -d: -f1 < /etc/passwd
   ```
   tee - stanadardowe wejscie na wyjscie i jeszcze d o pliku, rozdzila strumien na dwa
wc - zlicza -l linie, -w slowa, -c znaki
head - wypisz -c znakow, -n linii
tail - wypisz ostatnie -z znakow, -n linii
tr lancuch lancuch - zmian kazdy znaak z lewego na prawy
expand - zmaien tab na spacje
unexpand - spacje na tabulacje
nl - numeruj linie
od - kody bajtow otrzymanych danych, domyslnie osemkowe
fmt - formatowanie wierszy, zawijanie
pr - dzieli dane na strony, 
split - dzili plik na rowne czesci po 1000 linii i zapisuje z plikach xaa, xab,xac ...
join laczy dwa koniecznie posortowane piki majace wspolna kolumne
watch -n 1 date - urucmianie polecenia co 1 sekunde

diff test test2 - porownuje 2 pliki jesli takie same to nic nie wyswietla, jesli rozne wyswietla roznice
sdiff test test2 - porownuje i wyswieta 2 pliki
   
**[⬆ Back to Top](#table-of-contents)**

### Vi
i - tryb wprowadzania
R - tryb zastępowania
v - wizualny

:q - wyjdź
:q! wyjdź bez zapisu
:w zapisz
:w nazwa - zapisz kopie
:wq - zapisz i wyjdź
:r wstaw zawartość pliku
h j k l - lewo dol góra prawo
^ $ - początek koniec
w e - nastepne słowo , koniec następnego słowa
gg G - początek pliku, koniec pliku
numer linii gg - przedz do linii
Ctrl-B, Ctr-F - page up, page down

2w - 2 słowa dalej
u - cofnij zmiany
d - usuń
dd - usuń linie
d$ - usuń do końca
d^ - usuń do początku
21dgg - usuń 21 linie

y - kopiuj do bufora
y3w - skopiuj do bufora 3 słowa
3dw - wytnij znak, zaznaczenie

/test Enter- wyszukaj test
n - kolejne wystąpienie w dół
N - kolejne wystąpienie w góre

zastępowanie
zakres - jaki zakres przeszukania :%s/ - cały plik, 2,15s/ (linie od 2 do 15), .,.+6s/(od bieżącej do 6 poniżej), .,$s/(od bieżącego do końca pliku)
wyrażenie - \1 ciąg dopasowany w pierwszym wystąpieniu, \0 - dopasowany ciąg znaków
znaczniki
g - podmiana każdego dopasowanego ciagu a nie tylko pierwszego w linii
i - ignoruj wielkość znaków
c - pytanie do potwierdzenia każdej zmiany
:/s/Windows/Linux/g - zamien wszystkie wystąpienia Windows na Linux
:1,30s/Jan.*Kowalski/dr. \0/g - dodaj dr. przed każdym wystąpieniem Jana Kowalskiego

/etc/vimrc - opcje globalne edytora
~/.vimrc - opcje lokalne

### Startup

1. System V 
/etc/init.d lub /etc/inittab - skrypty startowe
rcS.d, rc5.d,... - uruchamianie w zależności od poziomu pracy
runlevel - poziom pracy
telinit 2 lub init 2 - zmiana poziomu pracy

Usługi są wyłączane a później uruchamiane, uruchamiane i zmykane  w kolejnosci alfabetycznej
S50usluga - start
K50uslug - kill

chkconfig - RHEL
update-rc.d mysql start 01 2 3 4 5 . stop 99 0 1 6 . - Debian

2. Systemd
Rodzaje jednostek:
service - usluga
socket - gniazdo
device - urzadzenie fizyczne
mount - punkt montowania
target - określa które usługi trzeba uruchomić 

/lib/systemd/system i /etc/systemd/system - pliki konfiguracyjne

Stany jednostek:
 loaded
 not found
 masked
 active
 inactive
 enabled
 disabled
 
 systemctl - zarzdza
 systemctl --all - aktywne i nieaktywne
 systemctl start httpd.service
 systemctl stop htt...
 systemctl reload http...
 systemctl restart http...
 systemctl reload-or-restart htt...
 systemctl kill htt...
 systemctl status httt....
 systemctl get-default - domyslny cel
 systemctl set-default multi-user.target - zmiana na tryb tekstowy multiuser
 systemctl list-units  --type target - aktywne cele
 systemctl isolate emergency.target - jak init 1, inne targety zostana wylaczone, uruchomiony zostanie podany z zaleznosciami
  
 grub, dodanie do parmaetrow jadra system.unit=emergency.target spowoduje uruchomienie tego trybu
 
 systemctl poweroff
 systemctl reboot
 systemctl suspent
 systemctl hibernate
 systemctl enable tralall.service
 systemctl disable tralall.service 
 systemctl is-enabled trall.service

 systemd-journald.service - jednostka rejestrowania zdarzen
 /run/log/journal - logi w pamieci
 /var/log/journal - logi na dysku
 
-unit usera  
~/.config/systemd/user/my-service.service
/usr/lib/systemd/user/my-service.service

3. Upstart
events - poziomy pracy zastąpione zdarzeniami, w rakcji na nie uruchmiane są jobs które uruchamiaja task i services
/etc/init - joby
exec - uruchomienie procesu
respawn - przy niespodziewanym zakonczeniu uruchomic ponownie

-przesylanie zdarzen i uruchamianie / zatrzymanie uslug
initctl stop ssh
initctl list

-wylaczenie automatyczne uruchminia
echo "manual" >> /etc/init/mojserwis.override

.init - w katalogu domowym definiujemy własne joby, np. tunele vpn/ssh które w odróżnieniu od .bashrc będą sie restartowac w razie awarii albo definiować zleżności

-odpytanie wszystkich skryptow używających uruchamiania system V
 sudo service --status-all

4. Środowisko graficzne
gnome-session-properties - dla GNOME 3


### Permission

bit setuid - plik wykonywalny uruchomi się na prawach własciciela dla zwykłych plków nie ma znaczenia 
 ls -l /bin/passwd
 -rwsr-xr-x 1 root root 68208 lis 29 12:53 /bin/passwd
chmod u+s file 

bit setgid - dla katalogu, pliki i katalogi tworzone w tym katalogu beda nalezaly do grupy z tego katalogu, dla pliku zwykłych plików nie ma znaczenia
chmod g+s file

bit sticky - dla katalogu, pliki znajdujace sie w nim ma prawo kasowac tylko root, wlasciciel katalogu i wlasciciel pliku. Bez niego każdy kto może pisać do katalogu może usuwać wszystkie pliki. Nawet jeśli nie ma praw do pliku a prawa zapisu do katalogu do plik mozna usunać chyba ze jest ustawiony sticky bit.

ls -la /tmp

chmod
 u g o a - domyslne a np. +x wyjatek +w
 + - =
 r w x s t X (ustawienie x tylko katalogom)
 +w zapis tylko dla wlasciciela i grupy
 chmod u+w file
 
Liczbowo

 opcje		wlasciciel		grupy		pozostali	
 4			4					4		4			read
 2			2					2		2			zapis
 1			1					1		1			execute
 
4 - setuid
2 - setgid 
1 - sticky
 
chmod 4744 file

Domyślne wyjściowe uprawnienia:
plik - 0666 (rw-rw-rw)
katalog - 0777 (rwx-rwx-rwx)

umask - prawa odejmowane nowo tworzonym plikom od wartości wyjściowych
umask
002

#### Logs   

dpkg -l rsyslog
vim /etc/rsyslog.d/02-apache2.conf
module(load="imfile" PollingInterval="10")                                                                                    
input(type="imfile"                                                                                                           
      File="/var/log/apache2/access.log"                                                              
      Tag="http_access"                                                                                                       
      StateFile="/var/spool/rsyslog/statefile1"                                                                               
      Severity="info"                                                                                                         
      Facility="local1")                                                                                                      
local1.info        @10.0.0.59:514

/etc/init.d/rsyslog restart
systemctl restart rsyslog


### Shortcuts
Alt+. - ostatni parametr polecenia
Ctrl+R nazwa - wyszukaj ostatnie polecenie
Ctrl+u - usun wszystko po lewej
Ctrl+a - na poczatek
Ctrl+e - na koniec
Ctrl+strzalka - przejsdz do slowa
Ctrl+w - usun slowo w lewo
Ctrl+c - zakoncz linie, przejdz do nowej
Shift+PgUp/PgDown - strona gora/dol
Ctrl+d - zamknij/wyloguj
Ctrl+l - wyszsc ekran


### Others

Standardowe wejscie, stumien stdin, deskryptor 0, plik
Standardowe wyjscie, strumien stdout, desktryptor 1, plik
Standardowe wyjscie bledow stderr,deskryptor 2, plik

cut -d: -f1 < /etc/passwd
cat /etc/passwd | cut -d ....

cat /etc/passwd > file

-dowiazanie
cat /etc/passwd > file 2>&1

| - operator potoku
<> - operatory przekierowania

- polecenia w poleceniach
echo `cat /etc/passwd`
echo $(date)
echo $(date +%d.%m.%Y)





-funkcje mieszajace

echo "dasdadas" | md5sum

md5sum test > sumy.txt
md5sum test2 >> sumy.txt
md5sum test3 >> sumy.txt

md5sum -c sumy.txt



sha256sum -c sha256sum.txt.asc   


wget https://www.centos.org/keys/RPM-GPG-KEY-CentOS-7
gpg --show-keys RPM-GPG-KEY-CentOS-7
pub   rsa4096 2014-06-23 [SC]
      6341AB2753D78A78A7C27BB124C6A8A7F4A80EB5
uid                      CentOS-7 Key (CentOS 7 Official Signing Key) <security@centos.org>


https://www.centos.org/keys/
Key fingerprint = 6341 AB27 53D7 8A78 A7C2  7BB1 24C6 A8A7 F4A8 0EB5

gpg --import RPM-GPG-KEY-CentOS-7

gpg --list-keys
/home/wkuchta/.gnupg/pubring.kbx
--------------------------------
pub   rsa4096 2014-06-23 [SC]
      6341AB2753D78A78A7C27BB124C6A8A7F4A80EB5
uid      

get http://ftp.osuosl.org/pub/centos/7/isos/x86_64/sha256sum.txt.asc
gpg --verify sha256sum.txt.asc
gpg: Signature made czw, 4 sie 2022, 19:58:57 CEST
gpg:                using RSA key 24C6A8A7F4A80EB5
gpg: Good signature from "CentOS-7 Key (CentOS 7 Official Signing Key) <security@centos.org>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 6341 AB27 53D7 8A78 A7C2  7BB1 24C6 A8A7 F4A8 0EB5


gpg --delete-key 6341AB2753D78A78A7C27BB124C6A8A7F4A80EB5






PROCESY

EUID - w czyim kontekscie i uprawnieniami dziala program
RUID - kto uruchomil program
EGID - to samo tylko grupy 
RGID - to samo tylko grupy

