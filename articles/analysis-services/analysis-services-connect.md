<properties
   pageTitle="Dohvaćanje podataka iz komponente Analysis Services Azure | Microsoft Azure"
   description="Saznajte kako se povezati i dohvaćanje podataka iz poslužitelju komponente Analysis Services u Azure."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="get-data-from-azure-analysis-services"></a>Dohvaćanje podataka iz komponente Analysis Services Azure
Nakon što ste napravili na poslužitelju u Azure i uvesti tabličnog modela, korisnici u vašoj tvrtki ili ustanovi spremni ste za povezivanje i počeli proučavati podatke.

Azure Analysis Services podržava klijentske veze pomoću [ažurirati modela objekta](#client-libraries); PRILAGOĐENA, AMO, Adomd.Net ili MSOLAP, da biste se povezali putem xmla na poslužitelj. Na primjer, Power BI, Power BI Desktop, Excel ili bilo koju aplikaciju proizvođača klijent koji podržava modela objekta.

## <a name="server-name"></a>Naziv poslužitelja
Kada stvorite poslužitelju komponente Analysis Services u Azure, navedite jedinstveni naziv, a regije u kojima je poslužitelj će biti stvoren. Prilikom određivanja naziva poslužitelja u vezi, sheme imenovanja poslužitelj je:
```
<protocol>://<region>/<servername>
```
 Gdje je protokol niz **asazure**, područje je Uri područja u kojem je stvorena na poslužitelj (Ako, na primjer, za Zapad SAD-a westus.asazure.windows.net) i naziv poslužitelja je naziv poslužitelja jedinstveni unutar područja.

## <a name="get-the-server-name"></a>Dohvaćanje naziva poslužitelja
Prije povezivanja, morate dobiti naziv poslužitelja. **Azure** portalu > poslužitelja > **Pregled** > **naziv poslužitelja**, kopiranje i naziva poslužitelja za cijelu. Ako drugim korisnicima u tvrtki ili ustanovi se povezujete s ovaj poslužitelj previše, ćete htjeti podijeliti ovaj naziv poslužitelja. Prilikom određivanja naziva poslužitelja, mora se koristiti cijeli put.

![Pristupili naziv poslužitelja na servisu Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connect-in-power-bi-desktop"></a>Povezivanje u Power BI Desktop

> [AZURE.NOTE] Ta je značajka pretpregleda.

1. U [Dodatku Power BI Desktop](https://powerbi.microsoft.com/desktop/), kliknite **Dohvati podatke** > **baza podataka** > **Azure Analysis Services**.

2. **Poslužitelj**, zalijepite naziv poslužitelja iz međuspremnika.

3. U **bazu podataka**ako znate naziv baze podataka tabličnog modela ili perspektive koji se želite povezati, zalijepite je u nastavku. U suprotnom, možete to polje ostavite prazno. Možete odabrati baze podataka ili perspektive na sljedećem zaslonu.

4. Ostavite mogućnost za **Povezivanje live** zadani odabrali, a zatim pritisnite **Poveži**. Ako se od vas zatraži da biste unijeli račun, unesite računa tvrtke ili ustanove.

5. **Navigator**za proširite na poslužitelj, a zatim odaberite model ili perspektive koji se želite povezati, a zatim kliknite **Poveži**. Jednim klikom na model ili perspektive prikazuje sve objekte za taj prikaz.


## <a name="connect-in-power-bi"></a>Povezivanje u dodatku Power BI
1. Stvorite Power BI Desktop datoteku koja ima uživo vezu modela na poslužitelju.

2. U [Dodatku Power BI](https://powerbi.microsoft.com)kliknite **Dohvati podatke** > **datoteke**. Pronađite i odaberite datoteku.


## <a name="connect-in-excel"></a>Povezivanje u programu Excel
Povezivanje s poslužiteljem Azure Analysis Services u programu Excel podržava pomoću dohvaćanje podataka u programu Excel 2016 ili Power Query u starijim verzijama. Potreban je [davatelj usluga MSOLAP.7](https://aka.ms/msolap) . Povezivanje pomoću čarobnjaka za uvoz tablica u dodatku Power Pivot nije podržana.

1. U programu Excel 2016, na vrpci **Podaci** kliknite **Dohvaćanje vanjskih podataka** > **Iz drugih izvora** > **Iz komponente Analysis Services**.

2. U čarobnjaku za povezivanje s podacima u **naziv poslužitelja**, zalijepite naziv poslužitelja iz međuspremnika. Zatim **vjerodajnice za prijavu**, odaberite **koristi sljedeće korisničko ime i lozinku**, a zatim upišite tvrtke ili ustanove korisničko ime, primjerice nancy@adventureworks.com, i lozinku.

    ![Povežite se u Excel prijave](./media/analysis-services-connect/aas-connect-excel-logon.png)

4. **Odaberite bazu podataka i tablicu**, odaberite bazu podataka i model ili perspektive, a zatim kliknite **Završi**.

    ![Povezivanje u programu Excel odaberite model](./media/analysis-services-connect/aas-connect-excel-select.png)

## <a name="connection-string"></a>Niz za povezivanje
Prilikom povezivanja s korištenja tabličnog modela objekta servisa Azure Analysis Services, koristite sljedeće oblike niza veze:

###### <a name="integrated-azure-active-directory-authentication"></a>Integriranu provjeru autentičnosti Azure Active Directory
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```
Integriranu provjeru autentičnosti će obraditi vjerodajnica predmemorije Azure Active Directory ako je dostupna. Ako nije, prikazuje se prozor za prijavu na Azure.

###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Provjera autentičnosti Azure Active Directory pomoću korisničkog imena i lozinke
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

## <a name="client-libraries"></a>Biblioteka klijenta
Prilikom povezivanja sa servisa Azure Analysis Services iz programa Excel ili druge sučelja kao što su PRILAGOĐENA, AsCmd ADOMD.NET, morate da biste instalirali najnoviju biblioteka klijentski davatelja usluga. Pojavljuje se najnoviji:  

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>



## <a name="next-steps"></a>Daljnji koraci
[Upravljanje poslužitelja](analysis-services-manage.md)
