<properties
   pageTitle="Generički SQL Connector korak po korak | Microsoft Azure"
   description="U ovom se članku koje je prolaska kroz jednostavne HR sustavom detaljne generički SQL poveznika protokola."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="generic-sql-connector-step-by-step"></a>Generički SQL poveznik korak po korak
U ovoj se temi je Postupni vodič. Stvara jednostavno uzorka HR bazu podataka i koristiti za uvoz nekim korisnicima i njihovim članstvo u grupi.

## <a name="prepare-the-sample-database"></a>Priprema oglednu bazu podataka
Na poslužitelju koji se izvodi SQL Server, pokrenuti skriptu SQL koji se nalaze u [dodatak A](#appendix-a). Ova skripta stvara ogledne baze pod nazivom GSQLDEMO. Model objekta baze podataka stvorene izgleda ovoj slici:  
![Objektni Model](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\objectmodel.png)

Stvoriti korisnika koji želite koristiti za povezivanje s bazom podataka. U ovom vodiču korisnik pod nazivom FABRIKAM\SQLUser i koja se nalazi u domeni.

## <a name="create-the-odbc-connection-file"></a>Stvaranje datoteke za ODBC veze
Generički poveznik za SQL ODBC koristi za povezivanje s udaljeni poslužitelj. Najprije ćemo potrebnih za stvaranje datoteke s ODBC podatke za povezivanje.

1. Da biste pokrenuli uslužni upravljanje ODBC na poslužitelju:  
![ODBC](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc.png)
2. Odaberite karticu **DSN datoteke**. Kliknite **Dodaj..**.
![ODBC1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc1.png)
3. – Okvir upravljačkog programa works precizno pa odaberite ga i kliknite **Dalje >**.  
![ODBC2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc2.png)
4. Imenujte datoteku, kao što su **GenericSQL**.  
![ODBC3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc3.png)
5. Kliknite **Završi**.  
![ODBC4](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc4.png)
6. Vrijeme će konfigurirati vezu. Dodijelite izvor podataka dobro opis, a zatim navedite naziv poslužitelja na kojem je SQL Server.  
![ODBC5](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc5.png)
7. Odaberite način provjere autentičnosti sa SQL. U ovom slučaju koristimo provjeru autentičnosti sustava Windows.  
![ODBC6](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc6.png)
8. Navedite naziv oglednu bazu podataka, **GSQLDEMO**.  
![ODBC7](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc7.png)
9. Zadrži sve zadano na zaslonu. Kliknite **Završi**.  
![ODBC8](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc8.png)
10. Da biste provjerili funkcionira sve prema očekivanjima, kliknite **Izvor podataka**.  
![ODBC9](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc9.png)
11. Provjerite je li test je uspješan.  
![ODBC10](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc10.png)
12. Konfiguracijska datoteka ODBC sada bi trebala biti vidljiva DSN datoteke.  
![ODBC11](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc11.png)

Imamo sada datoteka ćemo potrebna i mogu početi stvarati poveznik.

## <a name="create-the-generic-sql-connector"></a>Stvaranje poveznika generički SQL

1. U korisničkom Sučelju Upravitelj sinkronizacije servisa odaberite **poveznika** i **Stvaranje**. Odaberite **Generički SQL (Microsoft)** , a joj opisni naziv.  
![Connector1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector1.png)
2. Pronađite datoteku DSN koji ste stvorili u prethodnom odjeljku i prenesite ga na poslužitelj. Unesite vjerodajnice za povezivanje s bazom podataka.  
![Connector2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector2.png)
3. U ovom vodiču ćemo se jednostavno je za nam i recite da postoje dvije vrste objekata, **korisnika** i **grupa**.
![Connector3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector3.png)
4. Da biste pronašli atribute, želimo poveznik za otkrivanje te atribute tako da pogledate samu tablicu. Budući da **korisnici** je rezervirana riječ u SQL, moramo unesite ih u uglate zagrade [].  
![Connector4](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector4.png)
5. Vrijeme da biste definirali atribut sidra i atribut DN. Kombinacija dva atributa korisničko ime i IDZaposlenika koristimo za **korisnike**. **Grupa**za koristimo naziv grupe (ne realističan stvarnih, ali ovaj vodič radi).
![Connector5](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector5.png)
6. Sve vrste atributa moguće je otkriti u SQL baze podataka. Atribut vrstu reference posebice ne. Za vrstu objekta grupe moramo promjena ID vlasnika i MemberID reference.  
![Connector6](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector6.png)
7. Ne možemo odabran kao referencu atributa u prethodnom koraku zahtijevaju vrsta objekta te vrijednosti su atributi referenca. U našem slova vrsta objekta korisnika.  
![Connector7](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector7.png)
8. Na stranici globalni parametara odaberite **vodeni žig** kao strategije delta. Upisati u oblik datuma/vremena **gggg-MM-dd hh**.
![Connector8](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector8.png)
9. Na stranici **Konfiguracija particije i hijerarhije** odaberite obje vrste objekata.
![Connector9](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector9.png)
10. Na stranici **Odaberite vrste objekata** i **Odaberite atribute**odaberite vrste objekata i sve atribute. Na stranici **Konfiguracija sidra** kliknite **Završi**.

## <a name="create-run-profiles"></a>Stvaranje profila za pokretanje

1. U korisničkom Sučelju Upravitelj sinkronizacije servisa odaberite **poveznika**i **Konfigurirati pokretanje profila**. Kliknite **novi profil**. Ne možemo započnite **Potpuni uvoz**.  
![Runprofile1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile1.png)
2. Odaberite vrstu **Potpuni uvoz (faza samo)**.  
![Runprofile2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile2.png)
3. Odaberite particija **OBJEKTA = korisnika**.  
![Runprofile3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile3.png)
4. Odaberite **tablicu** , a zatim upišite **[KORISNICI]**. Pomaknite se prema dolje do odjeljka vrsti objekta s više vrijednosti, a zatim unesite podatke kao na sljedećoj slici. Odaberite **Završi** da biste spremili u koraku.
![Runprofile4a](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile4a.png)  
![Runprofile4b](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile4b.png)  
5. Odaberite **Novi korak**. Ovaj put, odaberite **OBJEKTA = grupe**. Na zadnjoj stranici pomoću konfiguracije kao na sljedećoj slici. Kliknite **Završi**.  
![Runprofile5a](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile5a.png)  
![Runprofile5b](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile5b.png)  
6. Neobavezno: Ako želite, možete konfigurirati dodatne izvođenja profile. Za ovaj vodič koristi se samo potpuni uvoz.
7. Kliknite **u redu** da biste dovršili promjenu izvođenja profila.

## <a name="add-some-test-data-and-test-the-import"></a>Dodavanje neke testiranje podataka i testiranje uvoza
Popunite neki podaci test u oglednu bazu podataka. Kada ste spremni, odaberite **Pokreni** , a **potpuni uvoz**.

Evo korisnik s dva telefonske brojeve i grupu s neke članove.  
![cs1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\cs1.png)  
![CS2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\cs2.png)  

## <a name="appendix-a"></a>Dodatak A
**SQL skriptu da biste stvorili oglednu bazu podataka**

```SQL
---Creating the Database---------
Create Database GSQLDEMO
Go
-------Using the Database-----------
Use [GSQLDEMO]
Go
-------------------------------------
USE [GSQLDEMO]
GO
/****** Object:  Table [dbo].[GroupMembers]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GroupMembers](
    [MemberID] [int] NOT NULL,
    [Group_ID] [int] NOT NULL
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[GROUPS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GROUPS](
    [GroupID] [int] NOT NULL,
    [GROUPNAME] [nvarchar](200) NOT NULL,
    [DESCRIPTION] [nvarchar](200) NULL,
    [WATERMARK] [datetime] NULL,
    [OwnerID] [int] NULL,
PRIMARY KEY CLUSTERED
(
    [GroupID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[USERPHONE]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[USERPHONE](
    [USER_ID] [int] NULL,
    [Phone] [varchar](20) NULL
) ON [PRIMARY]

GO
SET ANSI_PADDING OFF
GO
/****** Object:  Table [dbo].[USERS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[USERS](
    [USERID] [int] NOT NULL,
    [USERNAME] [nvarchar](200) NOT NULL,
    [FirstName] [nvarchar](100) NULL,
    [LastName] [nvarchar](100) NULL,
    [DisplayName] [nvarchar](100) NULL,
    [ACCOUNTDISABLED] [bit] NULL,
    [EMPLOYEEID] [int] NOT NULL,
    [WATERMARK] [datetime] NULL,
PRIMARY KEY CLUSTERED
(
    [USERID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_GROUPS] FOREIGN KEY([Group_ID])
REFERENCES [dbo].[GROUPS] ([GroupID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_GROUPS]
GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_USERS] FOREIGN KEY([MemberID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_USERS]
GO
ALTER TABLE [dbo].[GROUPS]  WITH CHECK ADD  CONSTRAINT [FK_GROUPS_USERS] FOREIGN KEY([OwnerID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GROUPS] CHECK CONSTRAINT [FK_GROUPS_USERS]
GO
ALTER TABLE [dbo].[USERPHONE]  WITH CHECK ADD  CONSTRAINT [FK_USERPHONE_USER] FOREIGN KEY([USER_ID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[USERPHONE] CHECK CONSTRAINT [FK_USERPHONE_USER]
GO
```
