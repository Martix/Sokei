Sokei
=====

An IO aggregation framework.

Sōkei [soh-kei]: Aggregate (japanese)

***CLEANUP NEEDED***

 1. TEPLO V INDUSTŘE
- regulace vytápění: návrh rozmístnění teploměrů uvnitř (půdorys, výškově) a venku
- heatmap: 3D render teploty vzduchu v závislosti na XYZ souřadnicích v objemu Industry
- mapa tepelné pohody: výpočet koeficientu tepelné pohody daný součtem povrchové teploty stěn a teploty vzduchu v zóně 0 .. 200 cm nad zemí (level kde se normálně pohybují lidi)
- časosběrné logování dat s ohledem na to jak jak zapínání jednotlivých generátorů tepla (kamna, teplovzdušné sahary, IR panely) mění topologii teploty / tepelné pohody v Industře
- predikční modelování jak kombinovat výkon a cenu jednotlivých tepelných zdrojů k dosažení optimální tepelné pohody (P,x,y,t) a funkce T(x,y,z,t) s ohledem na minimalizaci nákladů a náběhovou dobu efektu jednotlivých zdrojů tepla (kamna ... cca. 1800m3 / hod, sahary ... ?, IR zářič ... kvůli nízké povrchové teplotě a rozložení výkonu do velké plochy ... akumulace tepla na ozařovaných plochách za 2-3 dny)
- zohlednění předpovědi počasí (teplota vzduchu, srážky, osvit sluncem a ohřev skrze okna), návštěvnosti (frekvence otevírání dveří, integrál úniku tepla s ohledem na celkový čas otevření dveří a rozdíl teplot v interiéru a exteriéru)
- PID kontroler nad (P,x,y,t) a funkce T(x,y,z,t) se zapojemým machine learning (genetické algoritmy?, neurální sítě, support vector machine?)
- komerční aplikace, když už jsem vymyslel takovouhle prasárnu?
- detekování požáru, větrání, atp.
- nutné vybrat vhodné senzory
- nutné navrhnout jejich rozmístnění a propojení (wire, wireless?)

2. IP locks, zabezpečovací systém, kamerový systém
- detekce otevřených dveří, oknen
- counter počtů průchodu lidí (optický senzor?)
- odemykání dveří - nebezpečnostní zámky
- odemykání dveří - bezpečnostní zámky (nutné vybrat vhodný bezp. zámek s prohlášením o shodě a deklarovanou bezpečnostní třídou)
- logování kdo co otevřel
- acl na jednotlivé dveře, acl skupiny
- rest api, aplikace do telefonu
- rfid / nfc napojení -
- biometrický input? - nutné vybrat
- vysoké zabezpečnení komunikace mezi klientem a serverem, nebo navrhnout decentralizované řešení?
- monitoring kolik osob se nachází v industře (veřená anonymizovaná data kvůli požární ochraně)
- optional trackování lidí podle připojení k APčkům, bluetooth, nfc, rfid, atp. (nutné anonymizovat)
- checkin možnost pro lidi s veřejným profilem - chci v industře najít gra

3. Další řízení budovy
- sběr dat z analogových i digi elektroměrů, průtokoměrů a dalších sráčů
- řízení skladových zásob (výdej spotřebního materiálu podle váhy, integrace s kamerovým systémem)
- integrace 1+2
- blabla
 
 nástřel architektury - framework, backend, frontend, jazyk, coding guidelines
kdyby šlo o clean slate, tak řeknu:
framework (můj návrh: modularní, nebo snad ještě líp jako sadu standalone toolů, nonblocking / async, propojené přes mq, end to end encryption (sasl?), verzované rest api, shell + pajpování, autodiscovery, no spof - takže určo by mělo jít udélat nějakej master-master setup)
interchange format (můj návrh: json s metadaty, tj. min. verze dataformatu, hash dat, příp. i signovaná data)
backend (můj návrh: sql db / maria, honzův návrh: mongo)
frontend (consumer využije rest api -> řada možných frontendů)
nicméně nějaký kód už existuje, a někde bychom mohli znovuvynalézt kolo (hlavně co se týká senzorů tak existuje rrdtool, collectd, & friends) a github určo bude plný projektů které bychom mohli okouknout (https://github.com/rwwarren/door-lock)
- aspoň jedno unipi na veřejné ip (domluvit s fasterem?) - ssh / webrozhraní
- návrh androidí apky? nebo to necháme všechno na webovým rozhraní?
 
Automatizace Indy (co mě napadá z hlavy):
- zámky indoors pro lowlevel security (nfc, nebo ip), zámky na skříňky / private storage pro projekty v labu
- min. 2 outdoor zámky s vysokou bezpečností (nutný najít něco co jde spínat /napojit), důležitá jen bezp. třída
- snímání prostředí (teplota, tlak, koncentrace čpavku ;))
- spínání světel, větráků
- remote start/cut na napájení (bude se hodit až nám bude hořet jistič , můžeme tak udělat acl na zapínání strojů (např. nezapneš něco nač musíš být zaškolen, dokud se neautentikuješ jako zaśkolený uživatel)
- sklad (drahá extensivní varianta: přenosné drahé věci za nfc branou, průchod s vybavením signoffne věc ze skladu
- automatický výdej spotřebáku (šroubky na váhu, atp.)
- napojení na ezs (požární a bezpečnostní hlásič), resp. vytvoření vlastního ezs (napojení pohybových čidel a magnetů na okna/dveře)
- monitoring kolik členů se pohybuje v indě (požární ochrana)
- měření spotřeby energie a vody
- měření opotřebení (jak často je jakej velkej spotřebič / stroj) v provozu

Pan Inkognito Za me mam dobrou zkusenost s pythonem u vseho, co se tyce raspberry.
Jako webovej framework se mi dobre pracovalo s Djangem pro vetsi veci. Clovek si tam nadefinuje modely a necha si vygenerovat databazove schema + administraci. Podporuje to ruzne databaze (mne se libi postgresql). Web server se muze pouzit jakykoliv, treba tornado. Tohle pouzivaji i kluci ve Fasteru.
Pokud bychom chteli nejake interaktivni veci na klientovi, mam dobrou zkusenost s Reactem (JS framework od Facebooku).

Vladimír Brůžek Zdravim, ja za sebe bych take pro MongoDB a klidne bych zvolil node.js, jednak ma velice aktivni komunitu a funguje velice pekne, co jsem tak slysel a mam zkusenosti. V pripade, ze bychom chteli v budoucnu tam pridat nejaky vypocetne narocny backend tak existujou moznosti napojeni na C#, Javu atd. Kazdopadne ohledne UI frameworku moc zkusenosti nemam. Rozhodne bych zvazil jestli GitHub nebo BitBucket (ja osobne k nemu mam blizko). Preji prijemny den
edit: Nechci pusobit jako rebel, ale doma pouzivam nginx a muzu jen doporucit 

Pavel Stratil Add databaze: S Mongem jsem nikdy nepracoval, protoze pokazde kdyz jsem chtel tak jsem narazil na problemy co me trochu vzaly vitr z plachet (posledni 3 roky precijen panuje moda hejtovat mongo). Nicmene od 2/2.2 udelalo Mongo velky skok vpred a třeba je v současný verzi fajn. Koukal jsem vzdy po cassandre, ale trochu me zrazuje to jaky mam zkusenosti s cimkoliv psanym v jave jako extremne user unfriendly. Kazdopadne jestli budete moc chtít mongo, mrkněte taky na http://www.tokutek.com/ - (b-tree nahrazuje fractal tree v mongu a ve storage enginu pro mysql/mariadb) hraju si s tim ted se sql variantou a zatim dobry.

Jan Březina Jak to vidím já, zařízení (UniPi, cokoliv jiného) budou komunikovat přes API1 se SERVERem, který bude ukládat data do STORAGE. Zároveň bude SERVER poskytova veřejné API2 pro klienty (WEB BROWSER, MOBILNÍ APLIKACE, cokoliv dalšího). API1 je pro nás aktuálně REST api, které připravil na UniPi Faster, ale neměli bychom spoléhat na to, že budeme používat vždy a jen UniPi, abychom mohli kdyžtak zařízení nahradit/doplnit o něco dalšího. Jestliže bude na serveru databáze schopně abstrahována, není problém si hrát s více variantami. Co je pro nás asi hodně důležité je správně zvolit API2, které by mělo být standard, ale to si nedokážu představit co by to mělo být. Máte někdo nějakou představu/koukali jste po něčem?

https://scontent-a-mxp.xx.fbcdn.net/hphotos-xfp1/v/t1.0-9/1464647_10202696589166804_3436648543133131368_n.jpg?oh=5758e79645d509949b8720e6af756efb&oe=54ECF209

Jan Březina Jak řešit bezpečnost zařízení<->server? Pollovat zařízení ze strany serveru nebo nechat zařízení, aby posílala update stavu na server? 

Pavel Stratil Honzo: myslim ze by vse melo byt cryptovane (sasl?) a asi bychom meli aspon elementarne testovat co jde ze zarizeni (type/range) a logovat nesmyslny hodnoty - hw se muze zblaznit, muze byt po ceste ruseni na lince ... urcite bych byl pro, aby zarizeni posilalo data samo ale aby existovala i moznost neco pollnout. tak uvidime ze nejake zarizeni umre (kdyz nic neposle) a kdyz posle zgarbdata, tak muzeme treba jeste 3x pollnout jestli nahodou nedoslo k nejakemu problemu "po ceste". kumunikaci bych delal co nejvic robustni, to jak tohle bude dobry bude urcovat do jak narocnych / mission critical situaci pujde cely balik pouzit.
