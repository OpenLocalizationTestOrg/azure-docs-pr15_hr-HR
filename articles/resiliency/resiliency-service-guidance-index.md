<properties
   pageTitle="Smjernice za otpornost za servis | Microsoft Azure"
   description="Veze na oporavak Izrada i određene proaktivne stabilnosti i dostupnost smjernicama servisa Microsoft Azure."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

# <a name="microsoft-azure-service-resiliency-guidance"></a>Upute za otpornost servisa Microsoft Azure
Microsoft Azure osmišljene su da vam pruži resurse koji vam je potrebna, kada su vam potrebne. Dobar dizajn i radu prakse, dio biste trebali znati i kako mijenjanje arhitekture korištenje Azure servisa da biste postigli visoke dostupnosti kao i što učiniti ako vaše aplikacije servisa prekidu utječe. Da biste olakšali ovog postupka, ovaj dokument sadrži veze na upute za oporavak Izrada, kao i dizajn smjernice za različite servise za Azure.

##<a name="disaster-recovery-guidance"></a>Upute za oporavak Izrada
Upute za oporavak Izrada veze u nastavku su može vam pružiti potrebne informacije za pomoć pri aplikacije na mrežni način brzo ako su utječe na prekidu servisa Azure. Ove veze stvorene za odgovor na pitanje, "Li se u tijeku utječe prekidu programa Azure service što učiniti?"

##<a name="design-guidance"></a>Upute za dizajn
Dizajn smjernice veze u nastavku su dizajn i arhitektonski smjernice stvorenog pomoću kojih se objašnjava kako najbolje koristiti svakog servisa Azure na način koji Maksimizira vrijeme aktivnosti vaše aplikacije. Ove veze stvorene za odgovor na pitanje "Kako mogu provjeriti da pogrešku, pogreška hardver, prekidu servisa ili ostale pogreške neće utjecati na cjelokupan dostupnost Moje aplikacije?" Ako nema određene smjernice za servis trenutno tražite, možda je u članku [visoke dostupnosti za aplikacije koje se temelji na Microsoft Azure](./resiliency-high-availability-azure-applications.md) dodatnim informacijama koji vam mogu pomoći u dizajnu. 

##<a name="service-guidance"></a>Upute za servis
| Servis  | Upute za oporavak Izrada | Upute za dizajn |
|:---------|:--------------------------:|:------------------:|
| [Servisi u oblaku] (https://azure.microsoft.com/services/cloud-services/ "Azure servise u Oblaku")       | [veza] (../cloud-services/cloud-services-disaster-recovery-guidance.md "Upute za oporavak Izrada Azure servise u Oblaku")   | Nije dostupno |
| [Ključni zbirke ključeva] (https://azure.microsoft.com/services/key-vault/ "Azure sigurnog ključa")                      | [veza] (../key-vault/key-vault-disaster-recovery-guidance.md "Upute za oporavak Izrada sigurnog ključ Azure")        | Nije dostupno |
| [Prostor za pohranu] (https://azure.microsoft.com/services/storage/ "Azure prostora za pohranu")                            | [veza] (../storage/storage-disaster-recovery-guidance.md "Upute za oporavak Izrada Azure prostora za pohranu")          | Nije dostupno |
| [Baze podataka SQL] (https://azure.microsoft.com/services/sql-database/ "Baze podataka Azure SQL")           | [veza] (../sql-database/sql-database-disaster-recovery.md  "Upute za oporavak Izrada baze podataka SQL Azure")    | [veza] (../sql-database/sql-database-business-continuity.md "Pregled poslovanje s bazom podataka SQL Azure") |
| [Virtualnim strojevima] (https://azure.microsoft.com/services/virtual-machines/ "Azure virtualnim strojevima") | [veza] (../virtual-machines/virtual-machines-disaster-recovery-guidance.md "Upute za oporavak Izrada virtualnim računalima sustava Azure") | Nije dostupno |
| [Virtualne mreže] (https://azure.microsoft.com/services/virtual-network/ "Azure virtualne mreže")    | [veza] (../virtual-network/virtual-network-disaster-recovery-guidance.md "Upute za oporavak Izrada Azure virtualne mreže")  | Nije dostupno |

##<a name="next-steps"></a>Daljnji koraci
Ako tražite upute koje više njih svim korisnicima Dopusti usredotočuje se na sustavi i rješenja, pročitajte [oporavak Izrada i visoke dostupnosti za aplikacije koje se temelji na Microsoft Azure](https://aka.ms/drtechguide).
