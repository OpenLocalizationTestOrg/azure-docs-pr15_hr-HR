
Da biste konfigurirali uređaju VPN-a, morat ćete javnu IP adresu pristupnika virtualne mreže za konfiguriranje uređaju lokalnog VPN-a. Rad s proizvođača uređaja za određenu konfiguraciju informacije i konfiguriranje uređaja. Pogledajte na [Uređajima VPN-a](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md) dodatne informacije o VPN uređaje koji rade s Azure.

Da biste pronašli javnu IP adresu pomoću komponente PowerShell pristupnika za virtualne mreže, koristite sljedeći primjer:

    Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG

Možete pogledati i na javnu IP adresu za pristupnik za virtualne mreže pomoću portala za Azure. Dođite do **virtualne mreže pristupnika**, a zatim kliknite naziv svoje pristupnika.