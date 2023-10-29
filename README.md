# Exemple d’un DNS - mp07-uf01-04-dns-win2020


En aquesta ocasió se seguiran els passos de la web [jmsolanes.net/dns/](https://www.jmsolanes.net/dns/)


## Posant-nos en situació

Imaginem l'**Escola Ginebró SCCL**, composta per:

|Càrrec|Nom persona|
|---|---|
|Directora pedagògica|Maria Exposito|
|Cap d'estudis|Marc Lurbe|
|Responsable de cicles d'informàtica|Iván Nieto|
|Tutor SMX1|Salvador Quadrades|
|Tutor SMX2|Vladi Bellavista|
|Professor SXA|Iván Nieto|
|Professor SXA|Joan Pardo|


L'**Escola Ginebró** té les següents dades:

|Dada|Valor|
|---|---|
|Domicili social|C/ Joaquim Costa, s/n|
|CP|08450|
|Població|Llinars del Vallès|
|Província|Barcelona|
|CIF|F58241191|
|Correu electrònic|escola@ginebro.cat|

L'**Escola Ginebró** té dos edificis, un edifici per Primaria i Secundària i un segon edifici EBC (Eso, Batxillerat i Cicles Formatius).

I també disposa d’una bústia per les cartes.

![mapa-escola](./images/mapa-escola.png)

## Com es resoldria amb un servei de DNS?

Posats en situació, **com es resoldria amb un servei de DNS**?

**1.** **Dominis**

Primer cal identificar el **domini principal** i el **domini de la família**, el seu símil **```FQDN```** podria ser:

|Domini|Valor|Comentari|
|---|---|---|
|Principal|.cat|En aquest cas és el territorial, però també hi ha el .com, .es, .net, .org, etc.|
|De l'escola|ginebro|On s’identifica el grup, l'escola|

**2.** **Registre DNS**

Després, s’han de donar d’alta tots els noms que intervenen (persones, portes, bústies, etc..) formant el **```FQDN```** (nom, domini de l'escola i domini principal separats per un punt); relacionant-los amb l’adreça IP (C/ Joaquim Costa, s/n de Benante - Llinars del Vallès; o bé, Edifici primaria i secundària - Llinars del Vallès - Barcelona).


Els **```HOSTS```** (**registres de tipus** **```A```**) que relacionen un nom amb una **adreça IP**, podrien ser:

* Les **persones**:

|Nom|Adreça IP|
|---|---|
|mariaexposito.ginebro.cat|80.80.80.81|
|marclurbe.ginebro.cat|80.80.80.81|
|ivannieto.ginebro.cat|80.80.80.81|
|salvadorquadrades.ginebro.cat|80.80.80.81|
|vladibellavista.ginebro.cat|80.80.80.81|
|joanpardo.ginebro.cat|80.80.80.81|


* Els **espais**:
|Nom|Adreça IP|
|---|---|
|correu.ginebro.cat|80.80.80.81|
|ed-primaria.ginebro.cat|80.80.80.81|
|ed-ebc.ginebro.cat|45.45.45.46|
 

* Els **SOBRENOMS**

Serien els **registres** **```CNAME```** que relacionen un **sobrenom** a un **nom**, podrien ser:

|Sobrenom|Nom|
|---|---|
|dire-peda|mariaexposito.ginebro.cat|
|cap-estudis|marclurbe.ginebro.cat|
|resp-cicles-info|ivannieto.ginebro.cat|
|tutor-sxm1|salvadorquadrades.ginebro.cat|
|tutor-sxm2|vladibellavista.ginebro.cat|
|profe-sxa|joanpardo.ginebro.cat|
|profe-sxa|ivannieto.ginebro.cat|


* Els **serveis especials**

Alguns serveis especials, com el cas del servei de correu electrònic (**```registre MX```**) que relaciona un domini amb el **```HOST```** on s’ha d’**entregar el correu**.

Seguint l’exemple, es pot informar al carter que quan porti una carta la deixi a la bústia, no fa falta que busqui a la persona en concret, ells ja s’ocuparan de distribuir-la:

|Domini|Nom|
|---|---|
|ginebro.cat|correu.ginebro.cat|

L’encarregat de mantenir tot aquest llistat per qui el vulgui consultar és el **servei de DNS** i és útil per resoldre les **adreces públiques** (**Internet**) com les **adreces internes** (**xarxa privada**).

## Fer consultes a un DNS

El propi sistema ja s’encarrega de fer les consultes automàticament al DNS quan ho necessita, però també és una eina que es pot utilitzar manualment, per extreure informació molt valuosa d’un domini. Motiu pel que hem d’assegurar una bona higiene del mateix.

L’eina, d’ús més habitual, per fer les consultes a un **servei de DNS** és el **```nslookup```**. És una eina de la consola de sistema, per executar-la, cal anar a la consola i escriure:

```nslookup```

![executar-cmd.png](./images/executar-cmd.png)

I des de la línia de comandes executem la comanda **```nslookup```**, i ens obrirà l'entorn d'**```nslookup```** per poder realitzar les consultes que vulguem.

![inici-nslookup.png](./images/inici-nslookup.png)

Com veiem en el nostre cas, la primera informació que ens apareix és:


