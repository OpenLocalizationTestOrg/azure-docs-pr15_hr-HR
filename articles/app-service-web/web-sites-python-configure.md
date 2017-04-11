<properties 
    pageTitle="Konfiguriranje Python u aplikacije servisa za Azure Web Apps" 
    description="Pomoću ovog praktičnog vodiča opisuju mogućnosti za stvaranje i konfiguriranje osnovni web-poslužitelj sučelja pristupnika (WSGI) protokolom Python Azure servisa Web aplikacija." 
    services="app-service" 
    documentationCenter="python" 
    tags="python"
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="02/26/2016" 
    ms.author="huvalo"/>




# <a name="configuring-python-with-azure-app-service-web-apps"></a>Konfiguriranje Python u aplikacije servisa za Azure Web Apps

Pomoću ovog praktičnog vodiča opisuju mogućnosti za stvaranje i konfiguriranje osnovni aplikaciju za Web poslužitelja pristupnika sučelja (WSGI) koja Python [Azure servisa Web](http://go.microsoft.com/fwlink/?LinkId=529714)aplikacija.

Opisuje dodatne značajke brojka implementacije, kao što su okruženje virtualne i instalaciju paketa pomoću requirements.txt.


## <a name="bottle-django-or-flask"></a>Boca, Django ili Flask?

Trgovina Azure sadrži predloške za boce, Django i Flask okviri. Ako razvijate prvi web app u aplikacije servisa za Azure ili niste upoznati s brojka, preporučujemo da slijedite jedan od ove praktične vodiče, što obuhvaća detaljne upute za stvaranje aplikacije za rad u galeriji pomoću brojka implementaciju sa sustavom Windows ili Mac:

- [Stvaranje web-aplikacije pomoću boce](web-sites-python-create-deploy-bottle-app.md)
- [Stvaranje web-aplikacije pomoću Django](web-sites-python-create-deploy-django-app.md)
- [Stvaranje web-aplikacije pomoću Flask](web-sites-python-create-deploy-flask-app.md)


## <a name="web-app-creation-on-azure-portal"></a>Web app stvaranje portala za Azure

Pomoću ovog praktičnog vodiča pretpostavlja postojeću pretplatu na Azure i pristup portalu Azure.

Ako nemate postojeći web aplikacije, možete ga stvoriti s [Portala za Azure](https://portal.azure.com).  Kliknite gumb NOVO u gornjem lijevom kutu, a zatim kliknite **Web + Mobile** > **Web app**.

## <a name="git-publishing"></a>Objavljivanje brojka

Konfiguriranje brojka objavljivanjem za novostvorenom web-aplikaciju programa slijedeći upute u [Lokalnoj implementaciji brojka za aplikacije servisa za Azure](app-service-deploy-local-git.md). Pomoću ovog praktičnog vodiča koristi brojka za stvaranje, upravljanje i objaviti naša aplikacija za web Python aplikacije servisa za Azure.

Kada postavite brojka objavljivanja, brojka spremište bit će stvoren i povezan s web-aplikaciju programa. URL-a u spremištu prikazat će se i ponovne može se koristiti za slanje podataka iz lokalne platforme u oblak. Da biste objavili aplikacije putem brojka, provjerite je li brojka klijenta mora biti instalirana i koristiti upute da biste web-aplikacije sadržaja na aplikacije servisa za Azure.


## <a name="application-overview"></a>Pregled aplikacija

U sljedećim odjeljcima se stvaraju sljedeće datoteke. Mora biti smješten u korijenskom direktoriju spremište brojka.

    app.py
    requirements.txt
    runtime.txt
    web.config
    ptvs_virtualenv_proxy.py


## <a name="wsgi-handler"></a>Rukovatelj WSGI

WSGI je opisan [PEP 3333](http://www.python.org/dev/peps/pep-3333/) definiranje sučelja između web-poslužitelj i Python standard Python. Nudi standardizirani sučelja za različite web-aplikacije i okviri pomoću Python za pisanje. Popularne okviri web Python danas pomoću WSGI. Azure aplikacije servisa web-aplikacije stvar podrške za sve takve okviri; Osim toga, napredni korisnici možete čak i autor vlastite dok god prilagođeni rukovatelj slijedi smjernice specifikacija WSGI.

Evo primjera programa `app.py` koji definira prilagođeni rukovatelj:

    def wsgi_app(environ, start_response):
        status = '200 OK'
        response_headers = [('Content-type', 'text/plain')]
        start_response(status, response_headers)
        response_body = 'Hello World'
        yield response_body.encode()

    if __name__ == '__main__':
        from wsgiref.simple_server import make_server

        httpd = make_server('localhost', 5555, wsgi_app)
        httpd.serve_forever()

Možete pokrenuti aplikaciju lokalno s `python app.py`, zatim pronađite `http://localhost:5555` u web-pregledniku.


## <a name="virtual-environment"></a>Virtualna okruženje

Iako gornji primjer aplikaciju ne zahtijeva svih vanjskih paketa, vjerojatno je da aplikacija potrebno neke.

Da biste lakše upravljanje vanjskim paketa ovisnosti, brojka Azure implementacije podržava stvaranje virtualne okruženja.

Kada Azure otkrije na requirements.txt u korijenskoj mapi spremište, automatski stvara okruženje virtualne pod nazivom `env`. To pojavljuje samo na prve implementacije ili tijekom sve implementacije nakon odabranog Python runtime promijenio.

Vjerojatno ćete željeti stvaranje okruženje virtualne lokalno za razvoj, ali ne uključiti u vašem brojka spremište.


## <a name="package-management"></a>Upravljanje paketa

Paketi naveden u requirements.txt instalirat će se automatski virtualne okruženja pomoću točaka. To se događa u svakoj implementaciji, ali točaka preskočiti instalacije ako je paket već instaliran.

Primjer `requirements.txt`:

    azure==0.8.4


## <a name="python-version"></a>Python verzija

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

Primjer `runtime.txt`:

    python-2.7


## <a name="webconfig"></a>Web.config

Morat ćete stvoriti datoteku web.config da biste odredili način na koji poslužitelj tretirati zahtjeva.

Imajte na umu da ako web.x.y.config datoteka u spremištu, gdje x.y odgovara odabrane runtime Python, a zatim Azure automatski kopirati odgovarajuće datoteke kao web.config.

Sljedeći primjeri web.config oslanjate skripte proxy okruženje virtualne koji je opisan u sljedećem odjeljku.  Rade s WSGI događajima koji se koristi u ovom primjeru `app.py` iznad.

Primjer `web.config` za Python 2.7:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\activate_this.py" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_virtualenv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python27\python.exe|D:\Python27\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite"
                      url="handler.fcgi/{R:1}"
                      appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Primjer `web.config` za Python 3.4:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\python.exe" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_venv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python34\python.exe|D:\Python34\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite" url="handler.fcgi/{R:1}" appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Statičke datoteke će se rukovati web-poslužitelj izravno bez prolaze kroz Python šifru za bolje performanse.

U gornjem primjerima mjesto statičke datoteke na disku mora podudarati mjesto u URL-u. To znači da zahtjev za `http://pythonapp.azurewebsites.net/static/site.css` će vam poslužiti datoteku na disku na `\static\site.css`.

`WSGI_ALT_VIRTUALENV_HANDLER`je kojem određujete WSGI događajima. U gornjem primjerima ima `app.wsgi_app` jer rukovatelj je funkcija pod nazivom `wsgi_app` u `app.py` u korijenskoj mapi.

`PYTHONPATH`moguće je prilagoditi, ali ako instalirate sve ovisnosti u okruženje virtualne tako da u requirements.txt navedete, ne morate promijeniti ga.


## <a name="virtual-environment-proxy"></a>Okruženje virtualne proxy poslužitelja

Sljedeću skriptu se koristi za dohvaćanje rukovatelj WSGI, aktiviraj virtualne okruženje i zapisnika pogrešaka. Dizajniran je kako bi generički i koristiti bez izmjene.

Sadržaj `ptvs_virtualenv_proxy.py`:

     # ############################################################################
     #
     # Copyright (c) Microsoft Corporation. 
     #
     # This source code is subject to terms and conditions of the Apache License, Version 2.0. A 
     # copy of the license can be found in the License.html file at the root of this distribution. If 
     # you cannot locate the Apache License, Version 2.0, please send an email to 
     # vspython@microsoft.com. By using this source code in any fashion, you are agreeing to be bound 
     # by the terms of the Apache License, Version 2.0.
     #
     # You must not remove this notice, or any other, from this software.
     #
     # ###########################################################################

    import datetime
    import os
    import sys
    import traceback

    if sys.version_info[0] == 3:
        def to_str(value):
            return value.decode(sys.getfilesystemencoding())

        def execfile(path, global_dict):
            """Execute a file"""
            with open(path, 'r') as f:
                code = f.read()
            code = code.replace('\r\n', '\n') + '\n'
            exec(code, global_dict)
    else:
        def to_str(value):
            return value.encode(sys.getfilesystemencoding())

    def log(txt):
        """Logs fatal errors to a log file if WSGI_LOG env var is defined"""
        log_file = os.environ.get('WSGI_LOG')
        if log_file:
            f = open(log_file, 'a+')
            try:
                f.write('%s: %s' % (datetime.datetime.now(), txt))
            finally:
                f.close()

    ptvsd_secret = os.getenv('WSGI_PTVSD_SECRET')
    if ptvsd_secret:
        log('Enabling ptvsd ...\n')
        try:
            import ptvsd
            try:
                ptvsd.enable_attach(ptvsd_secret)
                log('ptvsd enabled.\n')
            except: 
                log('ptvsd.enable_attach failed\n')
        except ImportError:
            log('error importing ptvsd.\n');

    def get_wsgi_handler(handler_name):
        if not handler_name:
            raise Exception('WSGI_ALT_VIRTUALENV_HANDLER env var must be set')
    
        if not isinstance(handler_name, str):
            handler_name = to_str(handler_name)
    
        module_name, _, callable_name = handler_name.rpartition('.')
        should_call = callable_name.endswith('()')
        callable_name = callable_name[:-2] if should_call else callable_name
        name_list = [(callable_name, should_call)]
        handler = None
        last_tb = ''

        while module_name:
            try:
                handler = __import__(module_name, fromlist=[name_list[0][0]])
                last_tb = ''
                for name, should_call in name_list:
                    handler = getattr(handler, name)
                    if should_call:
                        handler = handler()
                break
            except ImportError:
                module_name, _, callable_name = module_name.rpartition('.')
                should_call = callable_name.endswith('()')
                callable_name = callable_name[:-2] if should_call else callable_name
                name_list.insert(0, (callable_name, should_call))
                handler = None
                last_tb = ': ' + traceback.format_exc()
    
        if handler is None:
            raise ValueError('"%s" could not be imported%s' % (handler_name, last_tb))
    
        return handler

    activate_this = os.getenv('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS')
    if not activate_this:
        raise Exception('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS is not set')

    def get_virtualenv_handler():
        log('Activating virtualenv with %s\n' % activate_this)
        execfile(activate_this, dict(__file__=activate_this))

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler

    def get_venv_handler():
        log('Activating venv with executable at %s\n' % activate_this)
        import site
        sys.executable = activate_this
        old_sys_path, sys.path = sys.path, []
    
        site.main()
    
        sys.path.insert(0, '')
        for item in old_sys_path:
            if item not in sys.path:
                sys.path.append(item)

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler


## <a name="customize-git-deployment"></a>Prilagodba brojka implementacije

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-deployment.md)]


## <a name="troubleshooting---package-installation"></a>Otklanjanje poteškoća – instalaciju paketa

[AZURE.INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]


## <a name="troubleshooting---virtual-environment"></a>Otklanjanje poteškoća – virtualne okruženje

[AZURE.INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u [Centru za razvojne inženjere Python](/develop/python/).

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

## <a name="whats-changed"></a>Što se promijenilo
* Vodič za promjenu iz aplikacije servisa za web-mjestima potražite u članku: [aplikacije servisa za Azure i Its utjecaj na postojećim Azure servisima](http://go.microsoft.com/fwlink/?LinkId=529714)





 
