IaaS virtualnim strojevima (VMs) i PaaS uloga instance virtualne mreže automatski dobiti privatne IP adrese iz raspona koji navedete, koji se temelji na podmreži povezani s. Tu adresu čuva VMs i instance uloge, dok se ne mogu se prestanka korištenja računala. Uloga ili VM instancu isključiti tako da zaustavljanje PowerShell, EŽA Azure ili Azure portal. U tim slučajevima kada instancu VM ili uloga pokreće ponovno ga primit će dostupna IP adresa iz Azure infrastrukture koje možda neće biti jednaki prethodno imali. Ako isključite instancu VM ili uloga s operacijskim sustavom goste zadržava IP adresa je imala.  

U određenim slučajevima želite VM ili uloga instancu da bi je statičke IP adrese, na primjer, ako vaš VM će pokrenuti DNS-a ili bit će kontroler domene. To možete učiniti tako da postavite statičke privatne IP adrese.