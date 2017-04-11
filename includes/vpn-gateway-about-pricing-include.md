Platite dvije stvari: u hourly izračunati troškove pristupnika virtualne mreže i prijenos izlazne podatke s pristupnika virtualne mreže. Informacije o cijenama pronaći ćete na stranici [određivanje cijena](https://azure.microsoft.com/pricing/details/vpn-gateway) .

**Virtualne mreže pristupnika računalnim troškovi**<br>Svaki pristupnika virtualne mreže ima programa hourly izračunati trošak. Cijena temelji se na pristupnika SKU koji navedete prilikom stvaranja pristupnik virtualne mreže. Trošak je pristupnika sam i uz prijenos podataka koji se piše putem pristupnika.

**Troškovi prijenosa podataka**<br>Troškovi za prijenos podataka se izračunava na temelju izlazne promet s pristupnika virtualne mreže izvora.

- Ako šaljete promet na lokalni VPN uređaj naplatit će s brzinu prijenosa za internetske izlazne podatke.
- Ako šaljete promet između virtualne mreže u različitim područjima cijene temelji se na regiju.
- Ako šaljete promet samo između virtualne mreže koje se nalaze u istom području, postoje troškove bez podataka. Promet između VNets u istom području je besplatno.