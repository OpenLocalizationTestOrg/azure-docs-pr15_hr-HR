Azure će određivanje verzije sustava Python za njegov virtualne okruženje pomoću sljedećih prioriteta:

1. verzija navedena u runtime.txt u korijenskoj mapi
1. verzija navedena postavljanjem Python u web-aplikacije konfiguracija ( **Postavke** > plohu**Postavke za aplikaciju** za web-aplikacije na portalu za Azure)
1. Ako ništa od navedenog određeni su klauzulom Python 2.7 je zadana postavka

Valjane vrijednosti za sadržaj 

    \runtime.txt

su:

- Python 2.7
- Python 3.4

Ako micro verzija (treću znamenku) nije naveden, zanemaruje se.
