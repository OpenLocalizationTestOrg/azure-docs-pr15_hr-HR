Kada stvorite pristupnik virtualne mreže, morate odrediti pristupnika SKU koji želite koristiti. Prilikom odabira više pristupnika SKU dodijeliti više propusnost mreže i procesora pristupnika i zbog toga pristupnika podržava veću propusnost mreže virtualne mrežu.

VPN pristupnika možete koristiti sljedeće SKU-ove:

- Osnovni
- Standardna
- HighPerformance

Kada odaberete SKU, imajte na umu sljedeće:

- Ako želite koristiti vrstu PolicyBased VPN, morate koristiti osnovni SKU-om. PolicyBased VPN-ovima (prije se zvao statične usmjeravanje) nisu podržane na drugim SKU.
- BGP nije podržana na osnovni SKU-om.
- ExpressRoute VPN pristupnika supostojanje konfiguracije nisu podržane na osnovni SKU-om.
- Na SKU HighPerformance na samo moguće je konfigurirati aktivno aktivno pristupnika S2S VPN veza.
