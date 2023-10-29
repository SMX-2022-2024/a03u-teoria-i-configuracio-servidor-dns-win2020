# Exemple d’un DNS - mp07-uf01-04-dns-win2020


<details><summary>Índex</summary>

### [***Pas 1***: Situació del **servidor** abans de començar](#pas-1-situació-del-servidor-abans-de-començar)

### [***Pas 2***: Fer consultes a un DNS](#pas-2-fer-consultes-a-un-dns-1)

### [***Pas 3***: Posant-nos en situació](#pas-3-posant-nos-en-situacic3b3-1)

### [***Pas 4***: Configurar el nostre propi **servidor de ```DNS```**](#pas-4-configurar-el-nostre-propi-servidor-de-dns-1)

</details>

<hr>

## ***Pas 1***: Situació del **servidor** abans de començar

### **1.** Dades del **Servidor** 

|Component|Estat|
|---|---|
|Nom del servidor|**```srv-pardo```**|
|Servidor DHCP|**instal·lat**|

### **2.** Configuració de les tres **interfícies de xarxa**:

#### **2.a** -> **1a  interfície de xarxa** la **```NAT```**:

![estat-1a-int-xarxa.png](./images/estat-1a-int-xarxa.png)

|Interfície de xarxa|Component|Estat|
|---|---|---|
|**NAT**|**```DHCP enabled```**|**```Yes```**|
||**```IPv4 Address```**|**```10.0.2.15```**|
||**```DNS Servers```**|**```10.0.2.3```**|

#### **2.b** -> 2a **interfície de xarxa** la **```Xarxa interna```**:

![estat-2a-i-3a-int-xarxa.png](./images/estat-2a-i-3a-int-xarxa.png)

|Interfície de xarxa|Component|Estat|
|---|---|---|
|**XarxaInterna**|**```DHCP enabled```**|**```No```**|
||**```IPv4 Address```**|**```172.128.8.1```**|
||**```DNS Servers```**|**```8.8.8.8```**<br>**```8.8.4.4```**|

#### **2.c** -> **3a interfície de xarxa** la **```HostOnly```**:

|Interfície de xarxa|Component|Estat|
|---|---|---|
|**HostOnly**|**```DHCP enabled```**|**```Si```**|
||**```IPv4 Address```**|**```192.168.56.1```**|
||**```DNS Servers```**|**```8.8.8.8```**<br>**```8.8.4.4```**|

### **3** Configuració del **Servei de ```DHCP```** 

![estat-server-abans.png](./images/estat-server-abans.png)

|Component|Estat|
|---|---|
|**nom del servidor**|**```srv-pardo```**|
|**adreça IP inicial**|**```172.128.8.100```**|
|**adreça IP final**|**```172.128.8.200```**|

#### **3.a** -> Reserves:

|Adreça MAC|Adreça ip reservada|nom reservat|
|---|---|---|
|**```0800270e32e4```**|**```172.128.8.105```**|**```localhost```**|

************************************

## ***Pas 2***: Fer consultes a un DNS

Abans de seguir amb l'activitat cal estudiar una eina per poder conèixer com obtenir la resposta a **consultes DNS** que volem fer.

El propi sistema ja s’encarrega de fer les consultes automàticament al DNS quan ho necessita, però també és una eina que es pot utilitzar manualment, per extreure informació molt valuosa d’un domini. Motiu pel que hem d’assegurar una bona higiene del mateix.

L’eina, d’ús més habitual, per fer les consultes a un **servei de DNS** és el **```nslookup```**. És una eina de la consola de sistema, per executar-la, cal anar a la consola i escriure:

```nslookup```

![executar-cmd.png](./images/executar-cmd.png)

I des de la línia de comandes executem la comanda **```nslookup```**, i ens obrirà l'entorn d'**```nslookup```** per poder realitzar les consultes que vulguem.

![inici-nslookup.png](./images/inici-nslookup.png)

Com veiem en el nostre cas, la primera informació que ens apareix és:

```
Default server: UnKnown
Address: 10.0.2.3
```

Això ens indica que en el moment d'iniciar l'aplicació **```nslookup```**, la nostra màquina no té definit cap **servidor de DNS**.

Per indicar a quin **servidor de DNS** volem fer les consultes, li indiquem de la següent manera:

```
> server 8.8.8.8
Default server: dns.google
Address: 8.8.8.8
```

Ara si, l'aplicació **```nslookup```** ens indica que ha trobar el **servidor de DNS** amb **adreça ip** **```8.8.8.8```**.
I també ens informa de que ha detectat que es tracta del **servidor de DNS** que respon al nom **```dns.google```**.

![nslookup-canvi-server.png](./images/nslookup-canvi-server.png)

Per tant, a partir d'ara les respostes que rebrem a les consultes que fem, seran les respostes que ens retorni  **servidor de DNS** **```dns.google```**.

Comencem per la primera consulta.

**1.** Consulta **```ginebro.cat```**

Volem coneixer quina és la resposta que ens retorna el servidor **```dns.google (8.8.8.8)```** a la pregunta sobre el servidor **```ginebro.cat```**.

```
> ginebro.cat
Server: dns.google
Address: 8.8.8.8

Non-authoritative answer:
Name:    ginebro.cat
Address: 217.149.11.212
```

Veiem que la resposta a la pregunta de quina adreça ip respon al nom **```ginebro.cat```** és **```217.149.11.212```**

No ens ho ha dit, pero aquesta consulta ha estat de **tipus ```A```**. Per defecte, si no es diu el contrari, les ***consultes DNS*** sempre son a registres de **tipus ```A```**


Important el tipus de resposta que retorna el servidor: ***Non-authoritative answer*** (**Resposta no autoritzada**), si fos autoritzada es podria arribar a "***robar***" totes les adreces que conté el **DNS**, per després utilitzar-ho amb finalitats diguem ***no massa clares***.

**2.** Consulta de **tipus ```soa```** a **```ginebro.cat```**

Per consultar al **servidor DNS**, la informació general del domini com pot ser el servidor DNS que el manté, adreça de contacte, etc. Cal canviar el tipus de consulta:

> Per modificar el tipus de registre que volem realitzar cal fer servir a comanda **```type=```** i el tipus de **registre DNS**.

```
> set type=soa
>
```

Recordem que **```soa```** és el **node superior** d’una zona (**```SOA```**, ***```S```***```tart``` ***```O```***```f``` ***```A```***```uthority```).

```
> ginebro.cat
Server: dns.google
Address: 8.8.8.8

Non-authoritative answer:

ginebro.cat
        primary name server: ns1.filnet.es
        responsible mail addr = mariaexposito.ginebor.cat
        serial  = 2023102906
        refresh = 10800 (3 hours)
        retry   = 3600 (1 hour)
        expire  = 1209600 (14 days)
        default TTL = 10800 (3 hours)
>
```

**3.** Consulta de **tipus ```mx```** a **```ginebro.cat```**

Per esbrinar quin és el servidor de correu electrònic d’un domini:

> Per modificar el tipus de registre que volem realitzar cal fer servir a comanda **```type=```** i el tipus de **registre DNS**.

```
> set type=mx
>
```

I sembla que no ha passat res. Però ara si fem la consulta ens tornarà la resposta al registre de **tipus ```mx```** del nom **```ginebro.cat```**


```
> ginebro.cat

Server: dns.google
Address: 8.8.8.8

Non-authoritative answer:
ginebro.cat	    MX preference = 30 aspmx3.googlemail.com
ginebro.cat	    MX preference = 30 aspmx5.googlemail.com
ginebro.cat	    MX preference = 10 aspmx.l.google.com
ginebro.cat	    MX preference = 30 aspmx2.googlemail.com
ginebro.cat	    MX preference = 20 alt2.aspmx.l.google.com
ginebro.cat	    MX preference = 20 alt1.aspmx.l.google.com
ginebro.cat	    MX preference = 30 aspmx4.googlemail.com
```

El resultat que retorna va relacionat per la **preferència ```MX```** i el servidor on enviar el correu.

> La **preferència ```MX```** indica a quin servidor s’ha de preguntar primer, en cas que no respongui, el segon, tercer etc. Com més petit és el número, més preferència té **```10```**, **```20```**, **```30```**, ...

**4.** Consulta de **tipus ```CNAME```** a **```mail.ginebro.cat```**

Per comprovar on apunta un sobrenom (**```CNAME```**):

```
> set type=cname
>
```

```
> mail.ginebro.cat

Server: dns.google
Address: 8.8.8.8

Non-authoritative answer:
mail.ginebro.cat 	    canonical name = ghs.google.com
```

**5.** Consulta de **tipus ```PTR```**

I en el cas que tinguem una **adreça IP** (**```PTR```**) i es vol saber a qui correspon?


```
> set type=ptr
>
```

```
> 217.149.11.212

Server: dns.google
Address: 8.8.8.8

Non-authoritative answer:
212.11.149.217.in-addr.arpa    name = srv11212.servatica.com
```

Per què ens pot ajudar aquesta eina?

* Comprovar que es resolt correctament un nom d’equip quan s’intenta accedir des d’un navegador i indica que no la troba o no existeix.
* Comprovar quins són els servidors de correu electrònic d’un domini, per després fer proves de connectivitat sobre els mateixos.
* Com es resol l’adreça IP i a quin domini pertany, per les comprovacions de reverse lookup de correu electrònic.
* Saber on està allotjat un domini concret.
* etc.

**6.** Consulta de **regitres ```DNS```** a [**who.is**](who.is)

Una altra manera de veure aquesta informació és amb pàgines web com [who.is](https://who.is), que retorna la informació de **TOTS** els **registres DNS** que hi ha a **Internet**.

[https://who.is/dns/ginebro.cat](https://who.is/dns/ginebro.cat)


![who-is-dns-ginebro-cat.png](./images/who-is-dns-ginebro-cat.png)

## ***Pas 3***: Posant-nos en situació

En aquesta ocasió se seguiran els passos de la web [jmsolanes.net/dns/](https://www.jmsolanes.net/dns/)

Imaginem l'**Escola Ginebró SCCL**, composta per:

|Càrrec|Nom persona|
|---|---|
|**Directora pedagògica**|**Maria Exposito**|
|**Cap d'estudis**|**Marc Lurbe**|
|**Responsable de cicles d'informàtica**|**Iván Nieto**|
|**Tutor SMX1**|**Salvador Quadrades**|
|**Tutor SMX2**|**Vladi Bellavista**|
|**Professor SXA**|**Iván Nieto**|
|**Professor SXA**|**Joan Pardo**|

L'**Escola Ginebró** té les següents dades:

|Dada|Valor|
|---|---|
|**Domicili social**|**```C/ Joaquim Costa, s/n```**|
|**CP**|**```08450```**|
|**Població**|**```Llinars del Vallès```**|
|**Província**|**```Barcelona```**|
|**CIF**|**```F58241191```**|
|**Correu electrònic**|**```escola@ginebro.cat```**|

L'**Escola Ginebró** té dos edificis, un edifici per **Primaria i Secundària** i un segon edifici **EBC (Eso, Batxillerat i Cicles Formatius)**.

I també disposa d’una bústia per les cartes.

![mapa-escola.png](./images/mapa-escola.png)

## Com es resoldria amb un servei de DNS?

Posats en situació, anem a analitzar com es podria resoldre amb un **servei de DNS**?

**1.** **Dominis**

Primer cal identificar el **domini principal** i el **domini de la família**, el seu símil **```FQDN```** podria ser:

|Domini|Valor|Comentari|
|---|---|---|
|Domini principal|**```.cat```**|En aquest cas és el **territorial**, però també hi ha<br>el **```.com```**, **```.es```**, **```.net```**, **```.org```**, etc.|
|Domini de l'escola|**```ginebro```**|On s’identifica el grup, l'escola|

**2.** **Registre DNS**

Després, s’han de donar d’alta tots els noms que intervenen (persones, portes, bústies, etc..) formant el **```FQDN```** (nom, domini de l'escola i domini principal separats per un punt); relacionant-los amb l’adreça IP (C/ Joaquim Costa, s/n de Benante - Llinars del Vallès; o bé, Edifici primaria i secundària - Llinars del Vallès - Barcelona).


Els **```HOSTS```** (**registres de tipus** **```A```**) que relacionen un nom amb una **adreça IP**, podrien ser:

#### Les **persones**:

|Nom|Adreça IP|
|---|---|
|**```mariaexposito.ginebro.cat```**|**```80.80.80.81```**|
|**```marclurbe.ginebro.cat```**|**```80.80.80.81```**|
|**```ivannieto.ginebro.cat```**|**```80.80.80.81```**|
|**```salvadorquadrades.ginebro.cat```**|**```80.80.80.81```**|
|**```vladibellavista.ginebro.cat```**|**```80.80.80.81```**|
|**```joanpardo.ginebro.cat```**|**```80.80.80.81```**|

#### Els **espais**:

|Nom|Adreça IP|
|---|---|
|**```correu.ginebro.cat```**|**```80.80.80.81```**|
|**```ed-primaria.ginebro.cat```**|**```80.80.80.81```**|
|**```ed-ebc.ginebro.cat```**|**```45.45.45.46```**|
 

#### Els **```SOBRENOMS```** o **àlies**

Serien els **registres** **```CNAME```** que relacionen un **sobrenom** a un **nom**, podrien ser:

|Sobrenom|Nom|
|---|---|
|**```director-pedagogic```**|**```mariaexposito.ginebro.cat```**|
|**```cap-estudis```**|**```marclurbe.ginebro.cat```**|
|**```resp-cicles-informatica```**|**```ivannieto.ginebro.cat```**|
|**```tutor-sxm1```**|**```salvadorquadrades.ginebro.cat```**|
|**```tutor-sxm2```**|**```vladibellavista.ginebro.cat```**|
|**```profe-sxa```**|**```joanpardo.ginebro.cat```**|
|**```profe-sxa```**|**```ivannieto.ginebro.cat```**|

#### Els **serveis especials**

Alguns **serveis especials**, com el cas del **servei de correu electrònic** (**```registre MX```**) que relaciona un domini amb el **```HOST```** on s’ha d’**entregar el correu**.

Seguint l’exemple, es pot informar al carter que quan porti una carta la deixi **a la bústia**. Que no cal que busqui a la persona en concret, ells ja s’ocuparan de distribuir-la:

|Domini|Nom|
|---|---|
|**```ginebro.cat```**|**```correu.ginebro.cat```**|

L’encarregat de mantenir tot aquest llistat per qui el vulgui consultar és el **servei de DNS** i és útil per resoldre les adreces públiques (**Internet**) com les internes (**xarxa privada**).

## ***Pas 4***: Configurar el nostre propi **servidor de ```DNS```**