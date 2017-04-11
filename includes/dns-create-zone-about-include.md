DNS zone koristi se za hostiranje DNS zapise za određene domene. Da biste započeli hostiranje vaše domene, morate stvoriti DNS zone. Sve DNS zapisa koji su stvorene za određenu domenu će se nalaziti unutar DNS zone za domenu. 

Na primjer, domene "contoso.com" može sadržavati broj DNS zapise, kao što su "mail.contoso.com" (za poslužitelj pošte) i "www.contoso.com" (za web-mjesta). 


## <a name="names"></a>O DNS zone naziva
 
- Naziv zone mora biti jedinstvena u grupi resursa, a u zonu mora već postojati. U suprotnom, postupak neće uspjeti.

- Isti naziv zone može ponovno koristiti u grupu različite resursa ili neku drugu pretplatu na Azure. 

- Gdje je više zona zajednički koristili s istim nazivom, svaku instancu dodjeljuju adrese drugi naziv poslužitelja, a samo jedna instanca se mogu dodijeliti iz nadređenog domene. Dodatne informacije potražite u članku [delegat domene Azure DNS-a](../articles/dns/dns-domain-delegation.md).