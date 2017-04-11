<properties
    pageTitle="Kako implementirati web-servisa na više područja | Microsoft Azure"
    description="Koraci za implementaciju regije (kopiranje) web-servisa na drugi."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="v-donglo"/>

# <a name="how-to-deploy-a-web-service-to-multiple-regions"></a>Kako implementirati web-servisa na više područja

Novi Azure web-servisi omogućuju jednostavno implementacija web-servisa na više područja bez potrebe za više pretplata ili radnim prostorima. 

Cijene je područje određene, stoga morate definirati naplate plan za svako područje u kojem će implementacija web-servisa.

## <a name="to-create-a-plan-in-another-region"></a>Da biste stvorili tarifu u drugoj regiji

1. Prijavite se u [Microsoft Azure strojnog učenja Web Services](https://services.azureml.net/).
2. Kliknite izbornik mogućnosti **tarife** .
3. Na tarife putem prikaz stranice, kliknite **Novo**.
4. Na padajućem popisu **Pretplata** odaberite pretplatu u kojem će se nalaziti novoj tarifi.
5. Na padajućem izborniku **područja** , odaberite područje za novu tarifu. Mogućnostima tarife za odabranog područja će se prikazivati u odjeljku **Planiranje mogućnosti** na stranici.
6. Na padajućem izborniku **Grupa resursa** , odaberite grupu resursa za plan. Dodatne informacije o grupama resursa, pročitajte članak [Upravljanje Azure resursi putem portala](../azure-portal/resource-group-portal.md).
7. **Planiranje** naziv upišite naziv plana.
8. U odjeljku **Mogućnosti tarife**kliknite razinu naplate za novu tarifu.
9. Kliknite **Stvori**.


## <a name="deploying-the-web-service-to-another-region"></a>Implementacija web-servisa na drugoj regiji

1. Kliknite izbornik mogućnosti **Web-servisi** .
2. Odaberite web-servisa implementacije novog regiju.
3. Kliknite **Kopiraj**.
4. U **Web-servisa naziv**, upišite novi naziv za web-servisa.
5. U okvir **opis web-servisa**unesite opis web-servisa.
6. Na padajućem popisu **Pretplata** odaberite pretplatu u kojem će se nalaziti nova web-servisa.
7. Na padajućem izborniku **Grupa resursa** , odaberite grupu resursa za web-servisa. Dodatne informacije o grupama resursa, pročitajte članak [Upravljanje Azure resursi putem portala](../azure-portal/resource-group-portal.md).
8. Na padajućem izborniku **područja** , odaberite područje u kojem za implementaciju web-servisa.
9. Na padajućem izborniku **prostora za pohranu računa** odaberite račun za pohranu u koju želite pohraniti web-servisa.
10. Na padajućem izborniku **Plan cijena** , odaberite plan u području koje ste odabrali u koraku 8.
11. Kliknite **Kopiraj**.

