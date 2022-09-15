# SCT-DEV
Documentació d'integració del projecte de direcció electrònica vial del servei català de trànsit    

- [1. Introducció](#1)
- [2. Transmissions de dades disponibles](#2)
- [3. Missatgeria del servei](#3)
   * [3.1. Consulta de dades de credencials per un titular](#3.1)
        * [3.1.1 Petició](#3.1.1)
			* [3.1.1.1 Dades genèriques](#3.1.1.1)
		* [3.1.2 Resposta](#3.1.2)	
			* [3.1.2.1 Resposta - Dades específiques](#3.1.2.1)
   * [3.2. Consulta de dades de credencials per lot](#3.2)
        * [3.2.1 Petició](#3.2.1)
			* [3.2.1.1 Restriccions](#3.2.1.1)
			* [3.2.1.2 Dades específiques](#3.2.1.2)
        * [3.2.2 Resposta – dades específiques](#3.2.2)
- [4. Codis de resposta](#4) 
- [5. Casos de prova](#5) 

### 1. Introducció <a name="1"></a>
Aquest document detalla la missatgeria associada al servei de consulta de credencials per DNI  del Servei de Credencials de Servei Català de Trànsit (en endavant SCT).
Per poder realitzar la integració cal conèixer prèviament la següent documentació:

- [Document de Missatgeria Genèrica de la PCI del Consorci AOC.][PCI]

[PCI]:https://github.com/ConsorciAOC/PCI


### 2. Transmissions de dades disponibles <a name="2"></a>
Les operacions disponibles a través del servei són les que es presenten a continuació:

L'emissor de dades és la DGT (Dirección General de Tráfico).

Producte (CodigoProducto) | Modalitat (CodigoCertificado) | Descripció
------------ | ------------- | -------------
ENOTUM_CREDENCIALS | CONSULTA_CREDENCIALS| Consulta de dades de contacte a partir de  un dentificador personal
ENOTUM_CREDENCIALS | CONSULTA_CREDENCIALS_LOT | Consulta de dades de contacte a partir de  un lot de dentificador s personals

(Correspondencia amb missatgeria PCI)

Modalitats:
Modalitat | Preproducció | Producció
------------ | ------------- | -------------
CONSULTA_CREDENCIALS | PROVES | NOTIFACTADM
CONSULTA_CREDENCIALS_LOT | PROVES | NOTIFACTADM

El servei de lots funciona assíncronament. L’esquema de comunicació és el que segueix:

![Esquema de comunicació](https://github.com/ConsorciAOC/SCT-DEV/blob/main/images/esquema.png)

### 3. Missatgeria del servei <a name="3"></a>
A continuació es detalla la missatgeria de les diferents modalitats de consum del producte.

#### 3.1 Consulta de dades de credencials per un titular <a name="3.1"></a>
Consulta de les credencials DEV (Dirección Electrónica Vial) d’un titular.

##### 3.1.1 Petició <a name="3.1.1"></a>
###### 3.1.1.1 Dades genèriques <a name="3.1.1.1"></a>
Element | Descripció
------------ | -------------
//DatosGenericos/Titular/TipoDocumentacion | Tipus de documentació (DNI / NIF,  NIE o CIF).
//DatosGenericos/Titular/Documentacion | Documentació.

##### 3.1.2 Resposta <a name="3.1.2"></a>
###### 3.1.2.1 Resposta - Dades específiques <a name="3.1.2.1"></a>
![Esquema de la resposta de dades específiques](https://github.com/ConsorciAOC/SCT-DEV/blob/main/images/esquema-resposta-dades-especifiques.png)
Element | Descripció
------------ | -------------
//respostaConsultaCredencials/correus | Bloc de dades amb les dades de correus electrònics facilitades per l’usuari
./correus/correu | Adreça de correu facilitada per l’usuari.
//respostaConsultaCredencials/telefons | Bloc de dades amb les dades de telèfons mòbils facilitades per l’usuari
./telefons/telefon | Telèfon mòbil facilitat per l’usuari.
//respostaConsultaCredencials/resultat | Bloc de dades 
//respostaConsultaCensCredencials/resultat/codiResultat | Codi de resultat de la consulta.
//respostaConsultaCensCredencials/resultat/descripcio | Descripció del resultat. *NOTA : Posar tipus als diferents errors, com adreces incorrectes, o nifs mal passats.*

#### 3.2 Consulta de dades de credencials per lot <a name="3.2"></a>
Consulta de les credencials DEV (Dirección Electrónica Vial) de un lot de identificadors.
##### 3.2.1 Petició <a name="3.2.1"></a>
###### 3.2.1.1 Restriccions <a name="3.2.1.1"></a>
* El màxim d’elements acceptats en un lot és de 1000 sol·licituds de consulta de credencials.
* El màxim de peticions a fer en un dia és de 50 peticions.
##### 3.2.1.2 Dades específiques <a name="3.2.1.2"></a>
![Esquema de dades específiques lot](https://github.com/ConsorciAOC/SCT-DEV/blob/main/images/esquema-peticio-dades-especifiques-lot.png)
Element | Descripció
------------ | -------------
peticioConsultaCredencialsDev/IdPeticio | Identificador de petició informat a la capçalera de la petició genèrica ( identificador de petició PCI ).
peticioConsultaCredencialsDev/ConsultaCredencials | Bloc de sol·licituds de credencials per persones físiques o jurídiques
//ConsultaCredencials/idSolicitud | Identificador incremental, numèric i únic ( per a la petició )
//ConsultaCredencials/idDocument | Identificador de persona ( física o jurídica ) a consultar
#### 3.2.2 Resposta - Dades específiques <a name="3.2.2"></a>
![Esquema de la resposta de dades específiques lot](https://github.com/ConsorciAOC/SCT-DEV/blob/main/images/esquema-resposta-dades-especifiques-lot.png)
Element | Descripció
------------ | -------------
respostaConsultaCredencialsDev/IdPeticio | Identificador de petició
respostaConsultaCredencialsDev/EstatPeticio | Estat de la petició. Els valors s’especifiquen en els codis de resposta
respostaConsultaCredencials/ConsultaCredencials | Bloc de credencials de la persona consultada
//ConsultaCredencials/IdSolicitud/ | Identificador de la solicitud enviada. Els valors s’especifiquen en els codis de resposta
//ConsultaCredencials/Estatsolicitud | Estat de la sol·licitud
//ConsultaCredencials/IdDocument/ | Identificador de document de persona a consultar
// ConsultaCredencials/Credencials/ | Bloc de credencials retornades per DEV
//Credencials/Telefon | Telèfon resgistrat al sistema ( màxim 5 ocurrències )
//Credencials/Correu | Correu electrònic resgistrat al sistema ( màxim 5 ocurrències )

### 4. Codis de resposta <a name="4"></a>
Codi | Descripció | Comentari
------------ | ------------- | -------------
0 | Resposta correcta |
DEV9000 | S’ha produït un error durant la execució del procés | Problema amb la missatgeria, el procés de la resposta o el procés per part de l’emissor, que ha retornat una resposta de error excepcional.
DEV9001 | S’ha produït un problema de comunicació amb l’emissor final de dades | L’emissor de dades no ha pogut processar la petició.
DEV9002 | S'ha excedit el nombre de peticions diàries establert per l'emissor final de dades. | La petició s’haurà de tornar a enviar passades unes hores.
DEV9003 | S'ha excedit el nombre màxim de sol·licituds per petició establert per l'emissor final de dades | La petició s’ha de dividir en peticions més petites
DEV9004 | Ja s'ha processat una petició amb aquest identificador de petició. | El identificador de petició ha de canviar
DEV9005 | Format de la petició incorrecte | La petició té un format diferent al que es presenta en aquest document.
DEV9006 | S'han produit problemes amb el servei de l'emissor final | El emissor ha retornat una resposta d’error de procés
DEV9007 | Temps de servei exhaurit | El emissor ha terminat la petició per excés en el temps establert per resoldre-la
DEV9008 | L'identificador no està donat d'alta al cens | L’identificador facilitat no està al cens de DEV. Aquest error també es produeix si el format del document de l’identificador és incorrecte.
DEV9009 | L'emissor final ha reportat un error relacionat amb la petició | L’emissor ha respost amb un error excepcional no identificat
DEV9010 | El NIF informat per a la recerca és incorrecte. | El format de NIF no és correcte.

*Nota : El servei no valida la correctesa dels documents facilitats. En cas que el document sigui incorrecte, es retornarà el corresponent codi generat pel servei DEV.*

Adjuntem link on trobareu exemples de peticions:
* [Petició i resposta CONSULTA_CREDENCIALS_LOT](https://github.com/ConsorciAOC/SCT-DEV/tree/main/Exemples/Consulta%20de%20credencials%20i%20subscripcions%20DEV%20per%20lots%20(CONSULTA_CREDENCIALS_LOT))
* [Petició i resposta CONSULTA_CREDENCIALS](https://github.com/ConsorciAOC/SCT-DEV/tree/main/Exemples/Consulta%20de%20credencials%20i%20subscripcions%20DEV%20unit%C3%A0ria%20(CONSULTA_CREDENCIALS))


### 5. Casos de prova <a name="5"></a>

Adjuntem els casos que es van predefinir per l'entorn de PRE:

* 12345678Z -> 0003 - Petició processada correctament. (Resposta: prova@dgt.es, 619276412)
* 70134616A -> 9999 - No està donat d'alta en el cens.
* Resta de casos -> 9997 - Petició amb errors.
