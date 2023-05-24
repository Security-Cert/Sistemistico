Oggi voglio iniziare una serie di articoli riguardanti alcuni **"Real Case"** affrontati a lavoro , partirò dalle cose piu' facili ad arrivare a casi piu' complessi, l'intento è quello di fornire un valido ausilio a chi inizia in ambito sistemistico.

PS non faro un analisi della vulnerabilità che trovate in fondo all' articolo spiegata nei vari link

# Ogetto:
_BUONGIORNO,
A SEGUITO DELLA RILEVAZIONE DELLE VULNERABILITÀ DI TIPO WEAK SSL/TLS KEY EXCHANGE EMERSE SUI SERVER `***.***.***.***` (ALLEGHIAMO ULTIMO SCAN REPORT RICEVUTO) E A SEGUITO DI CALL AVVENTUTE TRA NOI E , `********* ` VI CHIEDIAMO DI PIANIFICARE LA MODIFICA DELLE CONFIGURAZIONI DEL SERVER SSL/TLS PER CONSENTIRE SOLO SCAMBI DI CHIAVI EFFICACI (CON UNA DIMENSIONE DI 2048 BIT)
POICHÉ LE APPLICAZIONI IN ESECUZIONE SULLE PORTE SU CUI È STATA RILEVATA QUESTA VULNERABILITÀ POTREBBERO NON SUPPORTARE LA MODIFICA DELLA DIMENSIONE DELLE CHIAVI E DI CONSEGUENZA NON ACCETTARE LA CONTRATTAZIONE E QUINDI COMPORTARE DEI MALFUNZIONAMENTI SU ALCUNE APPLICAZIONI, TALI MODIFICHE ANDREBBERO DAPPRIMA APPORTATE SULL’AMBIENTE DI SVILUPPO, OVVERO SUL SERVER `***.***.***.***`  , RIMANIAMO  IN ATTESA DI ESSERE CONTATTATI PER CONCORDARE LE TEMPISTICHE PER L’APPLICAZIONE DELLE MODIFICHE ATTE A SANARE TALE VULNERABILITÀ_



Quello che ci viene chiesto in questo task è di andare a rimuovere la **key exchange ssl/tls vulnerabile** andando a hardenizzare cosi il server in ogetto 



# Anamnesi di un intervento....

appena ricevuto il ticket ci mettiamo all'opera e ci organizziamo con gli applicativi per comprendere meglio l'impatto , infatti sarebbe quantomeno avventato da parte nostra pensare di andare a disabilitare qualcosa senza conoscerne gli impatti , vero è che il server in ogetto è una macchina evolutiva ma è anche vero che alcune evolutive sono critiche come o forse piu di una macchina di produzione , quindi prima di tutto si fa un passaggio dove si analizza il possibile impatto , essendo noi un reparto sistemistico potremmo dover chiedere a sicurezza la remediation per la cve o un workaround .....
una volta stimati gli impatti e analizzato con i vari reparti coinvolti la tipologia di intervento andiamo a eseguire il vero e proprio interneto....

# Analisi del server....

La nostra analisi preliminare del server ci ha dato diverse informazioni.....
abbiamo reperito .....

>**IP :** ********
>**Host:** *******
>**Cliente:** *******
>**OS :** Win2012R2
>**Ambiente:** Evolutivo



# Intervento...

Dopo le varie analisi con gli eventuali reparti coinvolti , call con gli applicativi del cliente è arrivato il momento di andare ad eseguire l'intervento vero e proprio .....

quindi ci colleghiamo alla macchina a secondo di quelle che sono le specifiche date dal cliente , VPN, accesso diretto  etc etc etc.

una volta entrati nel server andiamo a settare una chiave di registo nel registro di win , abbiamo 2 modi per farlo ....

1. Aprire **regedit.exe** e andare a creare la chiave come segue:

> *`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\Diffie-Hellman \ServerMinKeyBitLength=dword:00000800`*

2. creare un file ***TXT*** e inserire il seguente testo all'interno

> `Windows Registry Editor Version 5.00`
>
> `[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\Diffie-Hellman]
> "ServerMinKeyBitLength"=dword:00000800`

una volta inserita la chiave salvare il file con l estensione ***.reg***

tasto destro sul file appena generato e premi ***unisci***  

***il metodo 2 è particolarmente utile per automatizzare il processo o replicarlo su piu server senza dover di volta in volta andare a ricreare la key a mano.***



Ecco fatto , il vostro intervento è termitato , tuttavia nel mio caso altre analisi hanno seguito l'intervento per verificare che la modifica alla key a 2048 bit non creasse disservizi.

alla prossima ;)



**Riferimenti:**

[[SSL/TLS Weak Key Exchange Supported | Tenable®](https://www.tenable.com/plugins/was/113316)]

[[Transport Layer Security (TLS) Parameters (iana.org)](https://www.iana.org/assignments/tls-parameters/tls-parameters.xhtml)]



---

**By Ddos**

