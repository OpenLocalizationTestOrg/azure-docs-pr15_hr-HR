### <a name="prerequisites"></a>Preduvjeti
- Račun Azure; možete stvoriti [pomoću računa](https://azure.microsoft.com/free)
- [Spremište blobova platforme Azure račun](../articles/storage/storage-create-storage-account.md) uključujući naziv računa za pohranu i njegov tipkovni prečac. Ove informacije nalazi se u postavkama računa za pohranu na portalu za Azure. Dodatne informacije o [Azure prostora za pohranu](../articles/storage/storage-introduction.md).

Prije korištenja računa za spremište blobova platforme Azure u aplikaciji logike, povezivanju s računom za spremište blobova platforme Azure. To možete jednostavno unutar aplikacije logike portala za Azure.  

Povezivanje s računom spremište blobova platforme Azure na sljedeći način:  

1. Stvaranje logike aplikacije. U dizajneru logike aplikacije dodajte okidač, a zatim dodajte akciju. Odaberite **Prikaži Microsoft upravljani API-ji** padajućem popisu, a zatim u okvir za pretraživanje unesite "blob". Odaberite jednu od akcija:  

    ![Azure blobova veze korak](./media/connectors-create-api-azureblobstorage/azureblobstorage-1.png)  

2. Ako već niste stvorili sve veze za Azure prostora za pohranu, od vas će se traži pojedinosti veze:   

    ![Azure blobova veze korak](./media/connectors-create-api-azureblobstorage/connection-details.png)  

3. Unesite pojedinosti o računu za pohranu. Potrebni su svojstva zvjezdicom.

    | Svojstvo | Pojedinosti |
|---|---|
| Naziv veze * | Unesite naziv neke za vezu. |
| Naziv računa spremišta Azure * | Unesite naziv računa za pohranu. Naziv računa spremišta prikazuje se u svojstvima prostora za pohranu na portalu za Azure. |
| Tipka za pristup računu Azure prostora za pohranu * | Unesite ključ za račun za pohranu. Tipke za pristup prikazuju se u svojstvima prostora za pohranu na portalu za Azure. |

    Da biste autorizirali aplikacijom logike za povezivanje i pristup vašim podacima se koristi te vjerodajnice. 

4. Odaberite **Stvori**.

5. Obratite pozornost na to je stvorena veza. Sada, nastavite s koracima u svojoj aplikaciji logika: 

    ![Azure blobova veze korak](./media/connectors-create-api-azureblobstorage/azureblobstorage-3.png)  
