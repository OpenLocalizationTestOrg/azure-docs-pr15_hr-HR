

Radi povezivanja sa **Sustava SharePoint Online**, morate unijeti identiteta (korisničko ime i lozinku, pametne kartice vjerodajnice, itd.) u sustavu SharePoint Online. Nakon što ste ste provjerena autentičnost, možete nastaviti koristiti poveznik za SharePoint Online u svojoj aplikaciji logike. 

U dizajneru logike aplikacije, slijedite ove korake da biste se prijavili u SharePoint da biste stvorili **vezu** za korisnike u svojoj aplikaciji logika:

1. U okvir za pretraživanje unesite sustava SharePoint i pričekajte za pretraživanje da biste se vratili sve okidača i akcije koje su vezane uz SharePoint Online:   
![Konfiguriranje sustava SharePoint][1]  
2. Odaberite **SharePoint Online – nakon stvaranja datoteke** okidača  
3. Odaberite **prijavite se u sustavu SharePoint Online**:   
![Konfiguriranje sustava SharePoint][2]    
4. Navedite SharePoint vjerodajnice za prijavu za provjeru autentičnosti sa sustavom SharePoint   
![Konfiguriranje sustava SharePoint][3]     
5. Nakon dovršetka provjeru autentičnosti bit ćete preusmjereni na logiku aplikaciju. To je to, stvorio vezu. Obratite pozornost na poruku pri dnu koja pokazuje da ste povezani sa sustavom SharePoint.  
![Konfiguriranje sustava SharePoint][4]  
6. Zatim možete dodati druge okidača i akcije koje su vam potrebne da biste dovršili logike aplikacije.   

[1]: ./media/connectors-create-api-sharepointonline/connectionconfig1.png
[2]: ./media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ./media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ./media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ./media/connectors-create-api-sharepointonline/connectionconfig5.png
