# SCT-DEV
Documentació d'integració del projecte de direcció electrònica vial del servei català de trànsit 
### 1. Introducció
Aquest document detalla la missatgeria associada al servei de consulta de credencials per DNI  del Servei de Credencials de Servei Català de Trànsit (en endavant SCT).
Per poder realitzar la integració cal conèixer prèviament la següent documentació:
* Document d’Especificació de missatgeria pel consum de productes de la plataforma PCI del Consorci AOC.

### 2. Transmissions de dades disponibles
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

![Esquema de comunicació](https://github.com/ConsorciAOC/SCT-DEV/blob/main/esquema.png)
