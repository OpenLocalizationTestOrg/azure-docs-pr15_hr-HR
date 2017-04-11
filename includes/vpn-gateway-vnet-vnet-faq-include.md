- Virtualne mreže mogu biti u istom ili drugom Azure područja (mjesta).

- Servis u oblaku ili na krajnjoj točki za ujednačavanje opterećenja ne može obuhvaćati putem virtualne mreža, čak i ako su povezani zajedno.

- Povezivanje više mreža Azure virtualne zajedno ne zahtijeva sve lokalne VPN pristupnika osim ako je potrebna je veza na više lokacija.

- VNet VNet podržava spojnih virtualne mreže. Ga ne podržava povezivanje virtualnim strojevima ili cloud usluge koje nisu unutar virtualne mreže.

- VNet VNet zahtijeva Azure VPN pristupnika s RouteBased (prije se zvao dinamički usmjeravanje) vrste VPN-a. 

- Veza s mrežom virtualne može se koristiti istodobno s više web-mjesta VPN-ovi, s najviše 10 (zadani/standardna pristupnika) ili 30 (visoke performanse pristupnika) VPN tunnels za povezivanje sa svakom virtualne mrežama pristupnik VPN virtualne mreže ili na lokalnim poslužiteljima web-mjesta.

- Za to predviđen adresu virtualne mreže i lokalne lokalne mreže web-mjesta morate preklapaju. Razmaci adresa koji se preklapaju može dovesti do stvaranja veze VNet VNet uvoza.

- Suvišne tunnels između par virtualne mreže nisu podržani.

- Sve tunnels VPN virtualne mreže zajedničko korištenje raspoložive propusnosti pristupnika Azure VPN-a i na istom VPN pristupnika vrijeme aktivnosti SLA u Azure.

- Promet VNet VNet prelazi preko Microsoft Network ne s Internetom.

- Promet VNet VNet unutar iste područja je besplatno za oba smjera; Unakrsni regija VNet VNet izlazne promet se naplaćuje s na izlazni pred-VNet brzinu prijenosa temelju područja izvora. Pogledajte [cijene stranica](https://azure.microsoft.com/pricing/details/vpn-gateway/) pojedinosti.