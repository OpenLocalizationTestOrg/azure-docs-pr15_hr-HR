Resurs|Zadano ograničenje|Maksimalno ograničenje
---|---|---
Azure Media Services (AMS) računima u jednom pretplate||25
Resursi po AMS računa||1,000,000<sup>1</sup>
Ulančana zadataka po poslu||30
Resursi po svakom zadatku||50
Resursi po poslu||100
Zadatke po AMS računu ||50.000<sup>2</sup>
Jedinstveni locators odjednom pridružene imovine||5<sup>4</sup>
Uživo kanala po AMS računu </p></td>|5</p></td>|N/d<sup>1</sup>
Programi u prestao stanju po kanala </p></td>|50</p></td>|N/d<sup>1</sup>
Programa izvodi stanja po kanala </p></td>|3</p></td>|3
Strujanje krajnje točke u izvodi stanja po AMS računa</p></td>|2</p></td>|N/d<sup>1</sup>
Strujanje jedinica po strujanje krajnje točke </p></td>|10 </p></td>|N/d<sup>1</sup>
Kodiranje jedinica po AMS računa </p></td>|25</p></td>|N/d<sup>1</sup>
Računi za pohranu | |1000<sup>5</sup>
Pravila || 1,000,000<sup>6</sup>

<sup>1</sup> možete zatražiti da biste ažurirali ograničenja za ovaj kvote tako da otvorite zahtjev za podršku možete. Nije moguće stvoriti dodatne račune AMS da biste povećali ograničenja, umjesto toga pošaljite zahtjev za podršku možete.

<sup>2</sup> taj broj sadrži u redu čekanja, dovršeni, aktivna i otkazani. Ne uključuje izbrisane zadatke. Možete izbrisati stari zadatke pomoću **IJob.Delete** ili HTTP zahtjev za **Brisanje** .

<sup>3</sup> kada želite zahtjeva entiteti popis posla, najviše 1000 vratit će se po zahtjev. Ako vam je potrebna da biste pratili sve poslane zadatke, kao što je opisano u odjeljku [Mogućnosti upita za OData sustava](http://msdn.microsoft.com/library/gg309461.aspx)možete koristiti gore/Preskoči.

<sup>4</sup> locators nisu namijenjeni upravljanja kontrolu pristupa po korisniku. Za dodjelu prava pristupa za različite za pojedinačne korisnike, koristite rješenja za upravljanje digitalnim pravima (DRM).

<sup>5</sup> račune za pohranu mora biti iz iste pretplate na Azure.

<sup>6</sup> postoji ograničenje od 1,000,000 pravila za različita pravila AMS (na primjer, za Locator pravila ili ContentKeyAuthorizationPolicy). Trebali biste koristiti isti ID pravila ako uvijek koriste isti dane / dozvole pristupa / itd.
