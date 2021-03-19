# wol-rapsberry-pi-server
Istruzioni per la creazione del server wake on lan per accendere il pc


Operazioni preliminari:

1) Controllare se il tuo pc supporta il wake on lan (successivamente chiamato WOL per abbreviare)
2) Scaricare il seguente eseguibile: http://magicpacket.free.fr/thankyou15.htm (App solo per windows, non ho idea se ne esista una variante per linux)
3) Il tuo smartphone abbia l'nfc oppure scaricare una delle tante app che offrono il servizio di WOL dallo store
4) Avere un calendario a portata di mano
5) Tag NFC 


# INIZIAMO:

Accediamo al nostro server (nel mio caso una Raspberry pi) 
Creiamo una cartella comoda per lavorare, io ho usato   mkdir /home/pi/wol
Andiamo nella cartella appena creata    cd /home/pi/wol
Creiamo il file che conterrà lo script    touch wol.py
wget https://bottlepy.org/bottle.py
pip install wakeonlan
Usiamo un qualsiasi editor di testo per aprirlo (uso mousepad visot che è preinstallato)    mousepad wol.py
Incolliamo il seguente script in python:

# SCRIPT

#Load libraries
from bottle import route, run, template
from wakeonlan import send_magic_packet

#Handle http requests to the root address
@route('/')
def index():
 return 'Allontanati, ti attende solo la rovina.'
 
#Handle http requests to /subdir
@route('/wakeitup/')
def wol():
 #Inserire il proprio mac address nel formato indicato
 send_magic_packet('AA-AA-AA-AA-AA-AA') 
 return 'WOL!'
#Inserire l'ip del server 
#Inserire la porta desiderata, si ricorda che per usare porte sotto il numero 1024 serve il root!
run(host='192.168.x.xxx', port=xxxx) 
#Fine script

# CONTINUA

Una volta salvato diamo i permessi per l'esecuzione   chmod +x wol.py
Eseguiamo il programma e facciamo un test   python wol.py
Visitiamo tramite browser il seguente indirizzo alla seguente porta (es 192.168.1.1:1234), ci restiturà la frase scelta nel primo return
Se visitiamo invece (es 192.168.1.1:1234/wakeitup/) ci restituirà "WOL!" e invierà il pacchetto alla destinazione specificata

Se tutto è andato a buon fine procediamo con il test vero e proprio, sul pc aprimao il programma WOL scaricato in precedenza e apriamo la sezione "receive" lasciando la porta UDP a 9 e cliccando su start, visitiamo di nuovo INDIRIZZO/wakeitup/ e uscirà un popup indicando che il WOL è stato ricevuto

Per fare qualcosa di magico userò il telefono (iphone 11 con NFC)

Sul dispositivo apriamo l'app "COMANDI" e scegliamo un trigger, nel nostro caso NFC e associamo un tag

Selezioniamo la sezione scripting e cerchiamo ssh

Compiliamo i vari campi e nella sezione comandi inseriamo questo:   curl -2 http://192.168.x.xxx:xxxx/wakeitup/

Salviamo

Spegniamo il pc

Avviciniamo il tag al telefono e si compierà la magia del natale

WOL!
