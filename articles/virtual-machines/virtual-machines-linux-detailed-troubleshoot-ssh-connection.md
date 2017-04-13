<properties
    pageTitle="Detaljne SSH upute za otklanjanje poteškoća za Azure VM | Microsoft Azure"
    description="Detaljnije SSH korake ima li problema povezivanja s Azure virtualnog računala za otklanjanje poteškoća"
    keywords="ssh veze odbio, ssh pogreška, azure ssh, SSH nije uspjelo povezivanje"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="detailed-ssh-troubleshooting-steps"></a>Detaljne upute za otklanjanje poteškoća SSH

Postoji više mogućih razloga koji klijent SSH možda nećete moći dosegne servis SSH na VM. Ako ste pratili kroz dodatne [općenite korake za otklanjanje poteškoća SSH](virtual-machines-linux-troubleshoot-ssh-connection.md), morate Dodatno otklanjanje problema. U ovom se članku će vas voditi kroz detaljne upute za otklanjanje poteškoća da biste odredili gdje u SSH prekida se veza i kako da biste riješili problem.

## <a name="take-preliminary-steps"></a>Preliminarni poduzeti

Sljedeći dijagram prikazuje komponente koje su povezane.

![Dijagram koji prikazuje komponente SSH servisa](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot1.png)

Sljedeći koraci pomoći izdvajanja izvor pogreške i utvrđivanje rješenja ili zaobilazna rješenja.

Najprije provjerite stanje VM na portalu.

[Portal za Azure](https://portal.azure.com):

1. Za VMs stvoren pomoću model klasični implementacije, odaberite **Pregledaj** > **virtualnim strojevima (klasični)** > *VM naziv*.

    – ILI –

    Za VMs stvoren pomoću modela Voditelj resursa, odaberite **Pregledaj** > **virtualnim strojevima** > *VM naziv*.

    Okno stanja za na VM treba prikazati **pokrenut**. Pomaknite se do Prikaži nedavne aktivnosti računalnim, za pohranu i mrežni resursi.

2. Odaberite **Postavke** da biste pregledali krajnje točke, IP adrese i druge postavke.

    Da biste odredili krajnje točke u VMs koje su stvorene pomoću upravitelja resursa, provjerite je li [mrežni sigurnosne grupe](../virtual-network/virtual-networks-nsg.md) je definiran. Provjerite da su primijenjeni pravila za mrežni sigurnosne grupe i da ste navedeni u podmreži.

[Azure klasični portal](https://manage.windowsazure.com)za VMs koje su stvorene pomoću klasične implementacije modela:

1. Odaberite **virtualnim strojevima** > *VM naziv*.
2. Odaberite na VM **nadzorne ploče** da biste provjerili ima li status.
3. Odaberite **Monitor** da biste vidjeli nedavne aktivnosti računalnim, za pohranu i mrežni resursi.
4. Odaberite **krajnje točke** da biste bili sigurni da ne postoji krajnje točke za SSH promet.

Da biste provjerili veza s mrežom, provjerite konfigurirani krajnje točke i potražite u članku ako je možete doći do VM putem drugog protokol, kao što su HTTP ili neki drugi servis.

Nakon tih koraka, pokušajte ponovno povezati SSH.


## <a name="find-the-source-of-the-issue"></a>Pronalaženje izvora problema

SSH klijentskog računala na vašem računalu možda se neće doći do servis SSH VM Azure zbog problema ili pogreške u konfiguraciji na sljedećim mjestima:

- [SSH klijentsko računalo](#source-1-ssh-client-computer)
- [Tvrtka ili ustanova rub uređaja](#source-2-organization-edge-device)
- [Krajnja točka servisa u oblaku i pristup popis kontrole (ACL)](#source-3-cloud-service-endpoint-and-acl)
- [Mrežni sigurnosne grupe](#source-4-network-security-groups)
- [Sustavom Linux Azure VM](#source-5-linux-based-azure-virtual-machine)

## <a name="source-1-ssh-client-computer"></a>Izvor 1: SSH klijentsko računalo

Da biste uklonili vaše računalo kao izvor pogreške, provjerite je li koje možete napraviti SSH veze s drugom na lokalnom računalu sa sustavom Linux.

![Dijagram koji se ističe SSH klijent računalne komponente](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot2.png)

Ako povezivanje ne uspije, provjerite sljedeće na vašem računalu:

- Postavka za lokalni vatrozid koji blokira dolazne i odlazne SSH promet (TCP 22)
- Lokalno instaliran klijentski softver proxy onemogućuje SSH veze
- Lokalno instalirali softver koji sprječava SSH veze za nadzor mreže
- Druge vrste sigurnosni softver koji praćenje promet ili Dopusti/onemogućiti određene vrste promet

Primijenite jedan od ovih uvjeta, privremeno onemogućite softver i pokušajte veze SSH sustava sustava lokalnog računala da biste saznali razloga veza blokiran na vašem računalu. Zatim nastaviti s administrator mreže da biste ispravili softverske postavke da dopušta veze SSH.

Ako koristite potvrda autentičnosti, provjerite imate li tih dozvola u mapu .ssh u imeniku kućni:

- ~/.Ssh Chmod 700
- Chmod 644 ~/.ssh/\*.pub
- ~/.Ssh/id_rsa Chmod 600 (ili druge datoteke koje ste privatni ključevi pohranjene u njima)
- Chmod 644 ~/.ssh/known_hosts (sadrži glavnih računala koje ste povezali s putem SSH)

## <a name="source-2-organization-edge-device"></a>Izvor 2: Tvrtka ili ustanova rub uređaja

Da biste uklonili uređaj rub tvrtke ili ustanove kao izvor pogreške, provjerite je li računalo na kojem je izravno povezano s Internetom možete napraviti SSH veze na svoje VM Azure. Ako na VM pristupate putem web-mjesto VPN-a ili programa Azure ExpressRoute, prijeđite na [izvor 4: mreže sigurnosne grupe](#nsg).

![Dijagram koji se ističe uređaj rub tvrtke ili ustanove](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot3.png)

Ako nemate instaliranu na računalu koje izravno povezani s Internetom, stvorite novi VM Azure u vlastitu grupu resursa ili servis u oblaku i ga koristiti. Dodatne informacije potražite u članku [Stvaranje virtualnog računala radi Linux u Azure](virtual-machines-linux-quick-create-cli.md). Brisanje grupe resursa ili VM i oblaka servisa kada završite s testiranja.

Ako stvorite vezu s SSH s računalom koje je izravno povezano s Internetom, provjerite koji uređaj rub tvrtke ili ustanove za:

- Interna vatrozid koji blokira SSH promet s Internetom
- Proxy poslužitelj koji sprječava SSH veze
- Otkrivanje podataka ili softver koji se izvodi na uređaje u mreži rub onemogućuje SSH veze za nadzor mreže

Rad s administrator mreže da biste ispravili postavke uređaja rub tvrtke ili ustanove da dopušta promet SSH s Internetom.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Izvor 3: Krajnja točka servisa u Oblaku i ACL

> [AZURE.NOTE] Ovaj izvor odnosi samo na VMs koje su stvorene pomoću klasične implementacije modela. VMs koje su stvorene pomoću upravitelja resursa, prijeđite na drugi [izvora 4: mreže sigurnosne grupe](#nsg).

Da biste uklonili krajnju točku usluge oblaka i ACL kao izvor pogreške, provjerite je li drugi VM Azure u istom virtualne mreže možete napraviti SSH veze na svoje VM.

![Dijagram koji se ističe krajnja točka servisa u oblaku i ACL-a](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot4.png)

Ako još nemate drugi VM u istom virtualne mreže lako možete stvoriti novi. Dodatne informacije potražite u članku [Stvaranje Linux VM na Azure pomoću na EŽA](virtual-machines-linux-quick-create-cli.md). Brisanje dodatni VM kada ste gotovi s testiranja.

Ako stvorite vezu s SSH s VM u istom virtualne mreže, provjerite sljedeće:

- **Konfiguraciju krajnju točku za SSH promet na odredištu VM.** Privatni TCP priključak krajnje točke moraju se podudarati TCP priključak na kojoj se servis SSH na VM priključuje. (Zadani je priključak je 22). Za VMs stvoren pomoću model implementacije Voditelj resursa, provjerite je li broj priključka SSH TCP na portalu za Azure tako da odaberete **Pregledaj** > **virtualnim strojevima (v2)** > *VM naziv* > **Postavke** > **krajnje točke**.

- **ACL-a za krajnju točku SSH promet na ciljnom virtualnog računala.** ACL omogućuju vam da odredite dopuštene ili zabranjen promet s Interneta, koji se temelji na izvoru IP adrese. Nisu pravilno konfigurirani ACL-a možete spriječiti SSH promet krajnjoj. Provjerite ACL-a da biste bili sigurni te promet na javnu IP adrese proxy poslužitelja ili je dopušteno druge rubni poslužitelj. Dodatne informacije potražite u članku [o pristup mreži kontrola popisa (ACL)](../virtual-network/virtual-networks-acl.md).

Da biste uklonili krajnju točku kao izvor podataka za taj problem, uklonite trenutnu krajnje točke, stvorili krajnju točku i navedite naziv SSH (TCP priključak 22 za broj priključka javne i privatne). Dodatne informacije potražite u članku [Postavljanje krajnje točke na virtualnog računala u Azure](virtual-machines-windows-classic-setup-endpoints.md).

<a id="nsg"></a>
## <a name="source-4-network-security-groups"></a>Izvor 4: Mreže sigurnosne grupe

Mrežni sigurnosne grupe omogućuju precizniji kontrolirati dopuštene ulazni i izlazni promet. Stvorite pravila koja obuhvaćaju podmreže i usluge u Azure virtualne mreže u oblaku. Provjerite mreže sigurnosne grupe pravila da biste bili sigurni da je dopušteno SSH promet i s Internetom.
Dodatne informacije potražite u članku [o sigurnosnim grupama s mrežom](../virtual-network/virtual-networks-nsg.md).

## <a name="source-5-linux-based-azure-virtual-machine"></a>Izvor 5: Sustavom Linux Azure virtualnog računala

Azure virtualnog računala sam je zadnji izvor moguće probleme.

![Dijagram koji se ističe sustavom Linux Azure virtualnog računala](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot5.png)

Ako to još niste učinili, pratite na upute [za ponovno postavljanje lozinke ili SSH za virtualnim strojevima sustavom Linux](virtual-machines-linux-classic-reset-access.md).

Pokušajte se ponovno povezati s računala. Ako je i dalje ne uspije, Slijede neki od mogućih problema:

- Servis SSH nije pokrenut na ciljnom virtualnog računala.
- Servis SSH nije priključuje na TCP priključak 22. Da biste to provjerili, instalirajte telnet klijent na lokalnom računalu i pokrenite "telnet *cloudServiceName*. cloudapp.net 22". Time se određuje ako virtualnog računala omogućuje ulazni i izlazni komunikaciju krajnjoj SSH.
- Lokalni vatrozid na ciljnom računalu virtualne sadrži pravila koja onemogućuju dolazni ili izlazni promet SSH.
- Otkrivanje podataka ili softver koji se izvodi na Azure virtualnog računala za nadzor mreže sprječava SSH veze.


## <a name="additional-resources"></a>Dodatni resursi
Dodatne informacije o otklanjanju poteškoća s aplikaciju programa access potražite u članku [Otklanjanje poteškoća s pristupom aplikacije koje se izvode na Azure virtualnog računala](virtual-machines-linux-troubleshoot-app-connection.md)