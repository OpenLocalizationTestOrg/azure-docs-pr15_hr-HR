Azure Raspoređivač opterećenja je Raspoređivač opterećenja sloj-4 (TCP, UDP). Raspoređivač opterećenja pruža visoke dostupnosti distribuiranjem dolazni promet među instanci pokvarenih servisa u oblak services ili virtualnih računala u skupu raspoređivača opterećenja. Azure Raspoređivač opterećenja možete predstaviti te usluge na više priključaka, više IP adresa ili oboje.

Možete konfigurirati Raspoređivač opterećenja za:

* Učitavanje dolazne internetski promet saldo virtualnih računala (VMs). Pozivamo Raspoređivač opterećenja u ovom scenariju kao [komuniciraju s Internetom Raspoređivač opterećenja](../articles/load-balancer/load-balancer-internet-overview.md).
* Učitavanje saldo promet između VMs virtualne mreže (VNet) između VMs services oblak ili između računala na lokalno i VMs u više lokacija virtualne mreže. Pozivamo Raspoređivač opterećenja u ovom scenariju kao [Interna Raspoređivač opterećenja (ILB)](../articles/load-balancer/load-balancer-internal-overview.md).
* Proslijedite vanjski prometa na određenu instancu VM.
