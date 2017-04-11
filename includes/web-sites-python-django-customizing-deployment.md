Azure će odrediti da se aplikacija koristi Python **Ako su zadovoljena oba sljedeća uvjeta**:

- Requirements.txt datoteke u korijenskoj mapi
- bilo koju datoteku .py u korijenskoj mapi ili runtime.txt koji određuje python

Kada je to slučaj, će koristiti određene implementacijsku skriptu Python, koji se izvodi standardne sinkronizacija datoteke, kao i dodatne operacije Python kao što su:

- Automatsko upravljanje virtualne okruženja
- Instalacija paketa naveden u requirements.txt pomoću točaka
- Stvaranje odgovarajuće web.config ovisno o odabranom Python verzijom.
- Prikupljanje statičke datoteke za Django aplikacije

Možete kontrolirati određene aspekte zadani koraci za implementaciju bez potrebe da biste prilagodili skriptu.

Ako želite preskočiti svih koraka Python određene implementaciju, možete stvoriti praznu datoteku:

    \.skipPythonDeployment

Ako želite preskočiti skup statičke datoteke Django aplikacije:

    \.skipDjango 

Za veću kontrolu nad implementacije možete nadjačati implementacijsku skriptu zadani stvaranjem sljedeće datoteke:

    \.deployment
    \deploy.cmd

[Azure sučelje naredbenog retka][] možete koristiti za stvaranje datoteka.  Iz mape programa project, koristite sljedeću naredbu:

    azure site deploymentscript --python

Kada se datoteke ne postoji, Azure stvara privremene implementacijsku skriptu i pokreće je.  Jednak onome koji ste stvorili pomoću naredbe iznad.

[Azure sučelje naredbenog retka]: http://azure.microsoft.com/downloads/
