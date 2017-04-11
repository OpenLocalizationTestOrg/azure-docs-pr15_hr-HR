<properties
   pageTitle="Primjena šifriranje u centru za sigurnost Azure | Microsoft Azure"
   description="Ovaj dokument pokazuje kako implementirati preporuke centar za sigurnost Azure **Primijeni šifriranje**."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="apply-disk-encryption-in-azure-security-center"></a>Primjena šifriranje u centru za sigurnost Azure

Centar za sigurnost Azure će preporučuje Primjena šifriranje ako imate Windows ili Linux VM diskova koje su šifrirane pomoću šifriranja Azure Disk. Šifriranje omogućuje šifriranje sustava Windows i Linux IaaS VM diskova.  Šifriranje preporučuje količine OS i podatke na vašem VM.


Šifriranje upravlja industrijskih standardne [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) značajka sustava Windows i značajka [DM Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux omogućuje šifriranje OS i podataka za pomoć pri zaštiti i zaštitili podataka i zadovoljavaju vaše tvrtke ili ustanove sigurnost i usklađenost p. Šifriranje integriran s [Azure ključ sigurnog](https://azure.microsoft.com/documentation/services/key-vault/) će vam pomoći odrediti i upravljanje ključeva za šifriranje diska i tajne u pretplatu ključ sigurnog uz jamstvo da sve podatke u diskova VM šifriraju na ostale u [Azure prostora za pohranu](https://azure.microsoft.com/documentation/services/storage/).

> [AZURE.NOTE] Azure šifriranje podržana za na sljedeći operacijski sustavi Windows server – Windows Server 2008 R2, Windows Server 2012 i Windows Server 2012 R2. Šifriranje podržana za Linux poslužitelja operacijski sustavi u nastavku - Ubuntu, CentOS, SUSE i SUSE Linux Enterprise Server (SLES).

## <a name="implement-the-recommendation"></a>Implementacija u preporuka

1. Odaberite **Primijeni šifriranje**plohu **preporuke** .
2. U plohu **šifriranje Primijeni** vidjet ćete popis VMs za koju se preporučuje šifriranje.
3. Slijedite upute da biste primijenili šifriranje te VMs.

![][1]

Da biste šifrirali Azure virtualnim strojevima uočeni su tako da Centar za sigurnost kao potrebe za šifriranje, preporučujemo sljedeće korake:

- Instaliranje i konfiguriranje Azure PowerShell. To će vam omogućiti da biste pokrenuli naredbe ljuske PowerShell potrebne da biste postavili preduvjeti potrebne za šifriranje virtualnim računalima sustava Azure.
- Nabavite i pokrenuti skriptu Azure Disk šifriranje preduvjeti Azure PowerShell.
- Šifriranje virtualnih računala.

[Šifriranje programa Azure virtualnog računala](security-center-disk-encryption.md) će vas voditi kroz sljedeće korake.  U ovoj se temi pretpostavlja da koristite Windows 10 kao na klijentskom računalu s kojeg će konfigurirati šifriranje.

Postoji mnogo postupke koje je moguće koristiti za postavljanje preduvjete i konfiguriranje šifriranje virtualnim računalima sustava za Azure. Ako ste već dobro versed Azure PowerShell ili EŽA Azure, bilo bi dobro da biste koristili zamjenski pristupa. Dodatne informacije o tim druge postupke potražite u članku [Azure šifriranje](../security/azure-security-disk-encryption.md).



## <a name="see-also"></a>Vidi također

Ovaj dokument prikazivao kako implementirati preporuke centar za sigurnost "Primijeni šifriranje." Da biste saznali više o šifriranje, pogledajte sljedeće:

- [Šifriranje i upravljanja ključem s sigurnog ključ Azure](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (videozapis, 36 min 39 sec) – Saznajte kako koristiti disk šifriranje upravljanja IaaS VMs i sigurnog Azure ključ za pomoć pri zaštiti i zaštitili podataka.
- [Šifriranje Azure virtualnog računala](security-center-disk-encryption.md) (dokument) – Saznajte kako šifrirati virtualnim računalima sustava Azure.
- [Azure šifriranje](../security/azure-security-disk-encryption.md) (dokument) – Saznajte kako omogućiti šifriranje za Windows i Linux VMs.

Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Postavljanje pravila zaštite u centru za sigurnost Azure](security-center-policies.md) – Saznajte kako konfigurirati sigurnosna pravila.
- [Nadzor sigurnost stanja u centru za sigurnost Azure](security-center-monitoring.md) – upute za praćenje stanja Azure resurse.
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja.
- [Upravljanje preporuke o sigurnosti u centru za sigurnost Azure](security-center-recommendations.md) – Saznajte kako preporuke olakšavaju zaštita Azure resurse.
- [Najčešća Pitanja za centar za sigurnost azure](security-center-faq.md) – traženje često postavljana pitanja o korištenju servisa.
- [Sigurnost azure blog](http://blogs.msdn.com/b/azuresecurity/) – blog traženje članaka o Azure sigurnost i usklađenost



<!--Image references-->
[1]: ./media/security-center-apply-disk-encryption/apply-disk-encryption.png
