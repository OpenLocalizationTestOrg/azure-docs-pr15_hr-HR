<properties
   pageTitle="Ključ sef Developer's Guide | Microsoft Azure"
   description="Programeri možete koristiti sef Azure ključ za upravljanje kriptografski ključevi unutar okruženja Microsoft Azure. "
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/03/2016"
   ms.author="bruceper" />

# <a name="azure-key-vault-developers-guide"></a>Azure ključa sef Developer's Guide
Korištenje ključa sef moći sigurno pristup osjetljive informacije iz aplikacije takva da:

- Tipke i tajne bit će zaštićene bez potrebe pisanja šifru i moći jednostavno za korištenje iz aplikacije.
- Bit ćete moći imati vlastitu klijentima i upravljati vlastitim ključeve tako koncentrirajte se na pružanje Jezgra značajke softvera. Na taj način vaše aplikacije će posedujete odgovornosti ili potencijalni odgovornost za tipke klijentske svojim klijentima i tajne.
- Aplikaciju možete koristiti tipke za potpisivanje i šifriranje još zadržava ključa upravljanje vanjskim iz aplikacije takva da je prikladan za aplikaciju koja zemljopisno Distribuirano rješenje.

- S September 2016 izdanje sef ključ aplikacije sada možete napraviti koristiti sef ključ potvrde. Za dodatne informacije, pogledajte **o tipke, tajne, i potvrde** članka u [OSTALE reference](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Za više općenitih informacija na Azure ključ sef vidjeti [što je ključ sef](key-vault-whatis.md).

## <a name="videos"></a>Videozapisi
Ovaj videozapis prikazuje kako stvoriti vlastiti ključ sef i kako koristiti iz aplikacije 'Hello ključ sef' uzorka.

[AZURE.VIDEO azure-key-vault-developer-quick-start]

Veze na resurse spomenute u videozapisa:
- [Azure PowerShell](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)
- [Azure ključa sef uzorak koda](http://go.microsoft.com/fwlink/?LinkId=521527&clcid=0x409)

Da biste saznali više možete slijedite [Ključ sef bloga](http://aka.ms/kvblog) i sudjelovanje u [Ključ Forum sef](http://aka.ms/kvforum).

## <a name="creating-and-managing-key-vaults"></a>Stvaranje i upravljanje sefovi ključa

Prije rad s sef Azure ključ u kodu, možete stvoriti i upravljati sefovi kroz OSTATAK, predlošci upravitelj resursa, PowerShell ili CLI za Defragmentaciju, kao što je opisano u sljedećim člancima:

- [Stvaranje i upravljanje ključa sefovi s OSTATKOM](https://msdn.microsoft.com/library/azure/mt620024.aspx)
- [Stvaranje i upravljanje ključa sefovi s PowerShell](key-vault-get-started.md)
- [Stvaranje i upravljanje ključa sefovi s CLI za Defragmentaciju](key-vault-manage-with-cli.md)
- [Stvaranje ključa sef i dodavanje tajni putem upravitelja resursa Azure predložak](../resource-manager-template-keyvault.md)

>[AZURE.NOTE] Autentičnost kroz AAD i ovlašteni kroz ključ sef 's vlastite Access, definirana pravila po sef operacija protiv sefovi ključa.

## <a name="coding-with-key-vault"></a>Kodiranje sef ključa

Sustav upravljanja sef ključ za programere sastoji se od nekoliko sučelja s OSTATKOM kao foundation, [Ključ sef REST API Reference](https://msdn.microsoft.com/library/azure/dn903609.aspx).

Možete, mogu uspješno autorizacije učinite sljedeće:

- Upravljanje kriptografski ključevi pomoću [Stvaranje](https://msdn.microsoft.com/library/azure/dn903634.aspx), [Uvoz](https://msdn.microsoft.com/library/azure/dn903626.aspx), [Ažuriranje](https://msdn.microsoft.com/library/azure/dn903616.aspx), [Brisanje](https://msdn.microsoft.com/library/azure/dn903611.aspx) i druge operacije

- Upravljanje tajne pomoću [dobiti](https://msdn.microsoft.com/library/azure/dn903633.aspx), [Ažuriranje](https://msdn.microsoft.com/library/azure/dn986818.aspx), [Brisanje](https://msdn.microsoft.com/library/azure/dn903613.aspx) i druge operacije

- Koristite kriptografski ključevi [znakom](https://msdn.microsoft.com/library/azure/dn878096.aspx)/[Provjera](https://msdn.microsoft.com/library/azure/dn878082.aspx), [WrapKey](https://msdn.microsoft.com/library/azure/dn878066.aspx)/[UnwrapKey](https://msdn.microsoft.com/library/azure/dn878079.aspx) i [Šifriraj](https://msdn.microsoft.com/library/azure/dn878060.aspx)/[dešifrirati](https://msdn.microsoft.com/library/azure/dn878097.aspx) operacije

Sljedeće SDK-ovi su dostupni za rad s sef ključ:

|[![.NET](./media/key-vault-developers-guide/msft.netlogo_purple.png)](https://msdn.microsoft.com/library/mt765854.aspx)|[![Node.js](./media/key-vault-developers-guide/nodejs.png)](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)
|:--:|:--:|
|[.NET SDK dokumentaciji](https://msdn.microsoft.com/library/mt765854.aspx)|[Node.js SDK dokumentaciji](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)|
|[.NET SDK paket na Nuget](http://www.nuget.org/packages/Microsoft.Azure.KeyVault)|[Node.js SDK paket](https://www.npmjs.com/package/azure-keyvault)|

Dodatne informacije o verzija 2.x .NET SDK potražite [Napomene](key-vault-dotnet2api-release-notes.md).

## <a name="example-code"></a>Primjer koda
Za potpuni Primjeri sef ključ pomoću aplikacije, pogledajte:

- Aplikacija uzorak .NET *HelloKeyVault* i primjer Azure web usluge. [Uzoraka koda Azure sef ključ](http://www.microsoft.com/download/details.aspx?id=45343)
- Praktični vodič za pomoć Naučite koristiti Azure ključ sef iz web-aplikacije u Azure. [Koristite Azure sef ključa iz web-aplikacije](key-vault-use-from-web-application.md)

## <a name="how-tos"></a>How-tos

Sljedeći članci i scenarije uputiti zadatka specifične za rad s sef Azure ključ:

- [Promjena ključa sef klijentske ID nakon pretplate premještanje](key-vault-subscription-move-fix.md) - kada premjestite Azure pretplate iz klijentske A klijentske B, postojeće ključa sefovi nedostupni su po upravitelje (korisnike i aplikacije) u klijentske B. popravak ovog pomoću ovog vodiča.
- [Pristupanje ključu sef iza vatrozida](key-vault-access-behind-firewall.md) - pristup ključa sef ključa sef klijentska aplikacija mora moći pristupiti više end-points različite radovi.

- [Kako Generiraj i Transfer HSM-Protected tipke za Azure ključ sef](key-vault-hsm-protected-keys.md) - to pomoći će vam planiranje, generirati i prijenos vlastite HSM zaštićene tipke za korištenje s Azure ključ sef.
- [Kako proslijediti sigurne vrijednosti (poput lozinke) tijekom uvođenja](../resource-manager-keyvault-parameter.md) - kada morate proći sigurne vrijednost (poput lozinke) kao parametar tijekom uvođenja, tu vrijednost možete pohraniti kao tajni u sef Azure ključ i referenca vrijednost u drugim predlošcima upravitelja resursa.
- [Kako koristiti ključ sef za extensible upravljanje ključevima SQL Server](https://msdn.microsoft.com/library/dn198405.aspx) - u SQL poslužitelj poveznika za Azure ključ sef omogućuje SQL Server i SQL-u-a-VM uravnotežavanje sef ključ Azure service kao davatelj Extensible Key Management (EKM) zaštititi ključeve za šifriranje za vezu aplikacije; Šifriranje podataka prozirna, sigurnosne kopije šifriranje i stupac razinu šifriranja.
- [Kako implementirati certifikati VMs iz sef ključ](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/) - oblak aplikacija radi u na VM na Azure mora certifikat. Kako prenijeti ovaj certifikat u ovom VM danas?
- [Kako instalacijski ključ sef s kraja završetka ključa zakretanja i nadzor](key-vault-key-rotation-log-monitoring.md) - sferama kroz kako da instalacija ključa zakretanja i nadzor Azure ključ sef.

Više Napuci specifične zadatka na integriranje i ključ sefovi pomoću Azure potražite [Ryan Jones upravitelj resursa Azure predložak primjere za ključ sef](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

## <a name="integrated-with-key-vault"></a>Integrirani sef ključa

Ti članci su o drugim scenarija i usluge koje bi nam od ili integrirati s ključ sef.

- [Šifriranje diska Azure](../security/azure-security-disk-encryption.md) leverages industrijske standardni [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) značajka sustava Windows i značajka [DM Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux pružaju šifriranje jedinice za os-a i diskova podataka. Rješenje integrirani Azure sef ključ da vam kontrolu i upravljanje ključeve za šifriranje diska i tajne pretplatu ključa sef štede sve podatke u virtualni stroj diskova šifriraju na ostatak Azure spremišta.


## <a name="supporting-libraries"></a>Biblioteke podrške

- [Microsoft Azure ključ sef Core biblioteka](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core) pruža `IKey` i `IKeyResolver` sučelja za pronalaženje ključevi iz identifikatora i izvođenje operacije s ključevima.

- [Microsoft Azure ključ sef proširenja](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions) nudi prošireni mogućnosti za Azure ključ sef.

## <a name="other-key-vault-resources"></a>Ostali resursi sef ključ
- [Blog sef ključa](http://aka.ms/kvblog)
- [Forum sef ključa](http://aka.ms/kvforum)
