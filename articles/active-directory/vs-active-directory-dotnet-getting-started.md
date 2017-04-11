<properties 
    pageTitle="Prvi koraci s Azure Active Directory i Visual Studio povezani servisi (MVC projekti) | Microsoft Azure" 
    description="Upute za početak rada s Azure Active Directory u MVC projekata nakon povezivanja s ili stvaranje programa Azure AD pomoću Visual Studio povezani servisi" 
    services="active-directory" 
    documentationCenter="" 
    authors="TomArcher" 
    manager="douge" 
    editor=""/>
  
<tags 
    ms.service="active-directory" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tarcher"/>

# <a name="getting-started-with-azure-active-directory-and-visual-studio-connected-services-mvc-projects"></a>Prvi koraci s Azure Active Directory i Visual Studio povezani servisi (MVC projekti)

> [AZURE.SELECTOR]
> - [Početak rada](vs-active-directory-dotnet-getting-started.md)
> - [Šta se dogodilo](vs-active-directory-dotnet-what-happened.md)
 
##<a name="requiring-authentication-to-access-controllers"></a>Koje je obavezna provjera autentičnosti za pristup kontrolera 

Sve kontrolera u projektu su adorned s atribut **ovlasti** . Taj atribut potrebno korisniku moguće provjeriti autentičnost prije pristupanja te kontrolera. Da biste omogućili kontrolerom za anonimno pristupiti, uklonite taj atribut s kontrolerom. Ako želite da biste postavili dozvole na razini precizniji, Primjena atribut svaki način na koji zahtijeva autorizacije umjesto primjene kontroler predmete.
 
##<a name="adding-signin--signout-controls"></a>Dodavanje prijava / SignOut kontrole 

Da biste dodali na kontrole prijava/SignOut prikaz **_LoginPartial.cshtml** dio prikaza možete koristiti za dodavanje funkcionalnosti na neki od svoje prikaze. Evo primjera funkcija dodati prikaz standardne **_Layout.cshtml** . (Imajte na umu posljednji element u div s navigacijska traka sažimanje klase):

<pre>
    &lt;! DOCTYPE html&gt; 
&lt;html&gt; 
&lt;head&gt; 
&lt;meta charset="utf-8" /&gt; 
&lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt; 
&lt;title&gt;@ViewBag.Title - My ASP.NET Application&lt;/title&gt; 
@Styles.Render("~/Content/css") @Scripts.Render("~/bundles/modernizr") &lt;/head&gt; 
&lt;body&gt; 
&lt;div class="navbar navbar-inverse navbar-fixed-top"&gt; 
&lt;div class="container"&gt; 
&lt;div class="navbar-header"&gt; 
&lt;button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse"&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;/button&gt; 
@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" }) &lt;/div&gt; 
&lt;div class="navbar-collapse collapse"&gt; 
&lt;ul class="nav navbar-nav"&gt; 
&lt;li&gt;@Html.ActionLink("Home", "Index", "Home")&lt;/li&gt; 
&lt;li&gt;@Html.ActionLink("About", "About", "Home")&lt;/li&gt; 
&lt;li&gt;@Html.ActionLink("Contact", "Contact", "Home")&lt;/li&gt; 
&lt;/ul&gt; 
                    <span style="background-color:yellow">@Html.Partial("_LoginPartial")</span> 
                &lt;/ div&gt; 
&lt;/div&gt; 
&lt;/div&gt; 
&lt;div class="container body-content"&gt; 
@RenderBody() &lt;hr /&gt; 
&lt;footer&gt; 
&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET Application&lt;/p&gt; 
&lt;/footer&gt; 
&lt;/div&gt; 
@Scripts.Render("~/bundles/jquery") @Scripts.Render("~/bundles/bootstrap") @RenderSection("scripts", required: false) &lt;/body&gt; 
&lt;/html                                                                                                                                                                                                                                                                                                                                                                                                                                                           &gt;
</pre>

[Dodatne informacije o servisu Azure Active Directory](https://azure.microsoft.com/services/active-directory/) 
