<properties
   pageTitle="Poslušajte aplikacije za korištenje s sigurnost granicu okruženjima | Microsoft Azure"
   description="Implementacija ovu jednostavne web-aplikaciju nakon stvaranja u DMZ da biste testirali scenarija tijeka promet"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/01/2016"
   ms.author="jonor"/>

# <a name="sample-application-for-use-with-security-boundary-environments"></a>Primjer aplikacije za sigurnost granicu okruženja

[Vratite se na stranicu najbolje prakse sigurnost granice][HOME]

Sljedeće skripte komponente PowerShell lokalno mogu se izvoditi na poslužiteljima IIS01 i AppVM01 za instaliranje i postavljanje vrlo jednostavne web-aplikacije koja prikazuje html stranicu s poslužiteljem IIS01 sučelje sa sadržajem pozadinskog AppVM01 poslužitelja.

Aplikacija će omogućuje jednostavno testiranja okruženje za mnogim se primjerima DMZ i na koji način promjene na krajnje točke, NSGs, UDR i vatrozid pravila možete efekta promet tokova.

## <a name="firewall-rule-to-allow-icmp"></a>Vatrozid pravilo kojim se omogućuje ICMP
Jednostavan PowerShell izjavu o zaštiti mogu se izvoditi na bilo kojem Windows VM da dopušta promet ICMP (Ping). To će omogućiti jednostavnije testiranje i otklanjanje poteškoća dopuštanjem ping protokol za proći kroz vatrozid za windows (za većinu Linux distros ICMP po zadanom je uključeno).

    # Turn On ICMPv4
    New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" `
        -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow

**Bilješke:** Ako koristite u skripti, ova zbrajanja pravila vatrozida slijedi prvu naredbu.

## <a name="iis01---web-application-installation-script"></a>IIS01 – web-aplikacije instalacije skripte
Ova skripta će:

1.  Otvorite IMCPv4 (pomoću naredbe Ping) na lokalni poslužitelj Vatrozid za windows za jednostavnije testiranje
2.  Instalirajte IIS i .net Framework v4.5
3.  Stvaranje web-stranice u ASP.NET i Web.config datoteke
4.  Promjena zadanog aplikacija da biste pojednostavnili pristup datotekama
5.  Postavljanje anonimni korisnik administratorskog računa i lozinke

U ovom skriptu PowerShell se trebale bi funkcionirati lokalno dok RDP imali u IIS01.

    # IIS Server Post Build Config Script
    # Get Admin Account and Password
        Write-Host "Please enter the admin account information used to create this VM:" -ForegroundColor Cyan
        $theAdmin = Read-Host -Prompt "The Admin Account Name (no domain or machine name)"
        $thePassword = Read-Host -Prompt "The Admin Password"
        
    # Turn On ICMPv4
        Write-Host "Creating ICMP Rule in Windows Firewall" -ForegroundColor Cyan
        New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
        
    # Install IIS
        Write-Host "Installing IIS and .Net 4.5, this can take some time, like 15+ minutes..." -ForegroundColor Cyan
        add-windowsfeature Web-Server, Web-WebServer, Web-Common-Http, Web-Default-Doc, Web-Dir-Browsing, Web-Http-Errors, Web-Static-Content, Web-Health, Web-Http-Logging, Web-Performance, Web-Stat-Compression, Web-Security, Web-Filtering, Web-App-Dev, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Net-Ext, Web-Net-Ext45, Web-Asp-Net45, Web-Mgmt-Tools, Web-Mgmt-Console
        
    # Create Web App Pages
        Write-Host "Creating Web page and Web.Config file" -ForegroundColor Cyan
        $MainPage = '<%@ Page Language="vb" AutoEventWireup="false" %>
        <%@ Import Namespace="System.IO" %>
        <script language="vb" runat="server">
            Protected Sub Page_Load(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Load
                Dim FILENAME As String = "\\10.0.2.5\WebShare\Rand.txt"
                Dim objStreamReader As StreamReader
                objStreamReader = File.OpenText(FILENAME)
                Dim contents As String = objStreamReader.ReadToEnd()
                lblOutput.Text = contents
                objStreamReader.Close()
                lblTime.Text = Now()
            End Sub
        </script>
            
        <!DOCTYPE html>
        <html xmlns="http://www.w3.org/1999/xhtml">
        <head runat="server">
            <title>DMZ Example App</title>
        </head>
        <body style="font-family: Optima,Segoe,Segoe UI,Candara,Calibri,Arial,sans-serif;">
          <form id="frmMain" runat="server">
            <div>
              <h1>Looks like you made it!</h1>
              This is a page from the inside (a web server on a private network),<br />
              and it is making its way to the outside! (If you are viewing this from the internet)<br />
              <br />
              The following sections show:
              <ul style="margin-top: 0px;">
                <li> Local Server Time - Shows if this page is or isnt cached anywhere</li>
                <li> File Output - Shows that the web server is reaching AppVM01 on the backend subnet and successfully returning content</li>
                <li> Image from the Internet - Doesnt really show anything, but it made me happy to see this when the app worked</li>
              </ul>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>Local Web Server Time</b>: <asp:Label runat="server" ID="lblTime" /></div>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>File Output from AppVM01</b>: <asp:Label runat="server" ID="lblOutput" /></div>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>Image File Linked from the Internet</b>:<br />
                <br />
                <img src="http://sd.keepcalm-o-matic.co.uk/i/keep-calm-you-made-it-7.png" alt="You made it!" width="150" length="175"/></div>
            </div>
          </form>
        </body>
        </html>'
        
        $WebConfig ='<?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <system.web>
            <compilation debug="true" strict="false" explicit="true" targetFramework="4.5" />
            <httpRuntime targetFramework="4.5" />
            <identity impersonate="true" />
            <customErrors mode="Off"/>
          </system.web>
          <system.webServer>
            <defaultDocument>
              <files>
                <add value="Home.aspx" />
              </files>
            </defaultDocument>
          </system.webServer>
        </configuration>'
            
        $MainPage | Out-File -FilePath "C:\inetpub\wwwroot\Home.aspx" -Encoding ascii
        $WebConfig | Out-File -FilePath "C:\inetpub\wwwroot\Web.config" -Encoding ascii
    
    # Set App Pool to Clasic Pipeline to remote file access will work easier
        Write-Host "Updaing IIS Settings" -ForegroundColor Cyan
        c:\windows\system32\inetsrv\appcmd.exe set app "Default Web Site/" /applicationPool:".NET v4.5 Classic"
        c:\windows\system32\inetsrv\appcmd.exe set config "Default Web Site/" /section:system.webServer/security/authentication/anonymousAuthentication /userName:$theAdmin /password:$thePassword /commit:apphost
        
    # Make sure the IIS settings take
        Write-Host "Restarting the W3SVC" -ForegroundColor Cyan
        Restart-Service -Name W3SVC
        
        Write-Host
        Write-Host "Web App Creation Successfull!" -ForegroundColor Green
        Write-Host


## <a name="appvm01---file-server-installation-script"></a>AppVM01 - datoteke poslužitelja instalacijskog skripta
Ova skripta postavlja pozadinska za ovu aplikaciju jednostavni. Ova skripta će:

1.  Otvorite IMCPv4 (pomoću naredbe Ping) u vatrozidu za jednostavnije testiranje
2.  Stvaranje novog direktorija
3.  Stvaranje tekstne datoteke može daljinski pristup gornju web-stranicu
4.  Postavljanje dozvola za direktorija i datoteke za anonimno dopustili pristup
5.  Isključivanje preglednika Internet Explorer Poboljšana sigurnost da biste omogućili pregledavanje jednostavnije ovaj poslužitelj 

>[AZURE.IMPORTANT] **Najbolja praksa**: Nikad ne isključite Poboljšana zaštita preglednika Internet Explorer na poslužitelju radnog plus je obično dobro pregledavati s poslužitelja radnog. Osim toga, otvara se zajedničke datoteke za anonimni pristup je dobro, ali gotovo ovdje jednostavnosti.

U ovom skriptu PowerShell se trebale bi funkcionirati lokalno dok RDP imali u AppVM01. Da biste se Pokreni kao Administrator da biste bili sigurni uspješno izvršavanje je PowerShell.
    
    # AppVM01 Server Post Build Config Script
    # PowerShell must be run as Administrator for Net Share commands to work
    
    # Turn On ICMPv4
        New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
    
    # Create Directory
        New-Item "C:\WebShare" -ItemType Directory
    
    # Write out Rand.txt
        $FileContent = "Hello, I'm the contents of a remote file on AppVM01."
        $FileContent | Out-File -FilePath "C:\WebShare\Rand.txt" -Encoding ascii
    
    # Set Permissions on share
        $Acl = Get-Acl "C:\WebShare"
        $AccessRule = New-Object system.security.accesscontrol.filesystemaccessrule("Everyone","ReadAndExecute, Synchronize","ContainerInherit, ObjectInherit","InheritOnly","Allow")
        $Acl.SetAccessRule($AccessRule)
        Set-Acl "C:\WebShare" $Acl
    
    # Create network share
        Net Share WebShare=C:\WebShare "/grant:Everyone,READ"
    
    # Turn Off IE Enhanced Security Configuration for Admins
        Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0
    
        Write-Host
        Write-Host "File Server Setup Successfull!" -ForegroundColor Green
        Write-Host
    

## <a name="dns01---dns-server-installation-script"></a>DNS01 - DNS poslužitelja instalacijskog skripta
Postoji bez skripte uključeni u ovu uzorka aplikaciju za postavljanje DNS poslužitelj. Ako testiranje pravila vatrozida, NSG ili UDR morate uključiti promet DNS-a, DNS01 server bit će potrebno postavljanje ručno. Xml datoteke mrežnoj konfiguraciji za oba Primjeri obuhvaćaju DNS01 kao primarni DNS poslužitelj i javne DNS poslužitelj hostira tvrtka razinu 3 kao sigurnosnu kopiju DNS poslužitelj. Razinu 3 DNS poslužitelj će biti stvarni DNS poslužitelj za promet-local, a s DNS01 nije postavljen, bez lokalni DNS bi se pojaviti.

<!--Link References-->
[HOME]: ../best-practices-network-security.md
