<properties
    pageTitle="Prikaz SAML vratio servis za kontrolu pristupa (Java)"
    description="Saznajte kako prikazati SAML vratio servis za kontrolu pristupa u aplikacijama Java hostirane na Azure."
    services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm" />

# <a name="how-to-view-saml-returned-by-the-azure-access-control-service"></a>Kako prikazati SAML vratio servis za kontrolu pristupa Azure

Ovaj vodič vidjet ćete kako se prikazuju u podlozi sigurnost pridruživanju oznake jezika (SAML) vraća u aplikaciji tako da na Azure pristup kontrola servisa (ACS). Vodič sastavlja o temi [kako autentičnost korisnicima Web Azure pristup kontrola servisa pomoću Eclipse][] unosom kod koji se prikazuju podaci za SAML. Dovršeni aplikacije izgledat će otprilike ovako.

![Primjer SAML Izlaz][saml_output]

Dodatne informacije o ACS potražite u odjeljku [sljedeće korake](#next_steps) .

> [AZURE.NOTE]
> Filtar za kontrolu servisa Azure pristup je pretpregled tehnologije zajednice. Kao predizdanje softver, on je formally Microsoft ne podržava.

## <a name="prerequisites"></a>Preduvjeti

Za dovršenje zadatka u ovom vodiču za dovršavanje uzorka na [način provjere autentičnosti Web korisnicima s Azure pristup kontrola servisa pomoću Eclipse][] i koristiti kao početnu točku za ovog praktičnog vodiča.

## <a name="add-the-jspwriter-library-to-your-build-path-and-deployment-assembly"></a>Dodavanje biblioteke JspWriter Sastavi put i implementaciju skupa

Dodavanje biblioteke koja sadrži klase **javax.servlet.jsp.JspWriter** za sastavljanje put i implementaciju skupa. Ako koristite Tomcat biblioteka je **jsp api.jar**, koja se nalazi u mapi Apache **Biblioteka** .

1. U Eclipse na Project Explorer desnom tipkom miša kliknite **MyACSHelloWorld**, kliknite **Sastavi put**, kliknite **Konfiguriraj sastavljanje put**, kliknite karticu **Biblioteka** , a zatim **Dodavanje vanjskog JARs**.
2. U dijaloškom okviru **Odabir POSUDU** dođite do potrebne POSUDU, odaberite ga, a zatim **Otvori**.
3. Dijaloški okvir **Svojstva za MyACSHelloWorld** još uvijek otvoren, kliknite **Skup implementacije**.
4. U dijaloškom okviru **Sastavljanje implementaciju Web** , kliknite **Dodaj**.
5. U dijaloškom okviru **Novi skup uputa** kliknite **Java sastavljanje put stavke** , a zatim kliknite **Dalje**.
6. Odaberite odgovarajuću biblioteku, a zatim kliknite **Završi**.
7. Kliknite **u redu** da biste zatvorili dijaloški okvir **Svojstva za MyACSHelloWorld** .

## <a name="modify-the-jsp-file-to-display-saml"></a>Izmjena JSP datoteku da biste prikazali SAML

Izmjena **index.jsp** da biste koristili sljedeći kod.

    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
        <%@ page import="javax.xml.parsers.*"
                 import="javax.xml.transform.*"
                 import="org.w3c.dom.*"
                 import="java.io.*"
                 import="javax.xml.transform.stream.*"
                 import="javax.xml.transform.dom.*"
                 import="javax.xml.xpath.*"
                 import="javax.servlet.jsp.JspWriter" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Sample ACS Filter</title>
    </head>
    <body>
        <h3>SAML information from sample ACS program</h3>
        <%!
        void displaySAMLInfo(Node node, String parent, JspWriter out)
        {
        
            try
            {
                String nodeName;
                int nChild, i;
                
                nodeName = node.getNodeName();
                out.println("<br>");
                out.println("<u>Examining <b>" + parent + nodeName + "</b></u><br>");
                   
                   // Attributes.
                   NamedNodeMap attribsMap = node.getAttributes();
                   if (null != attribsMap)
                   {
                         for (i=0; i < attribsMap.getLength(); i++)
                         {
                                Node attrib = attribsMap.item(i);
                                out.println("Attribute: <b>" + attrib.getNodeName() + "</b>: " + attrib.getNodeValue()  + "<br>");
                         }
                   }
                   
                   // Child nodes.
                   NodeList list = node.getChildNodes();
                   if (null != list)
                   {
                          nChild = list.getLength();
                          if (nChild > 0)
                          {                    
    
                                 // If it is a text node, just print the text.
                                 if (list.item(0).getNodeName() == "#text")
                                 {
                                     out.println("Text value: <b>" + list.item(0).getTextContent() + "</b><br>");
                                 }
                                 else
                                 {
                                     // Print out the child node names.
                                     out.print("Contains " + nChild + " child node(s): ");   
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        
                                        out.print("<b>" + temp.getNodeName() + "</b>");
                                        if (i < nChild - 1)
                                        {
                                            // Separate the names.
                                            out.print(", ");
                                        }
                                        else
                                        {
                                            // Finish the sentence.
                                            out.print(".");
                                        }
                                            
                                     }
                                     out.println("<br>");
                                     
                                     // Process the child nodes.
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        displaySAMLInfo(temp, parent + nodeName + "\\", out);
                                     }
                               }
                          }
                      }
                  }
                catch (Exception e)
                {
                    System.out.println("Exception encountered.");
                    e.printStackTrace();            
                }
            }
        %>
    
        <%
        try 
        {
            String data  = (String) request.getAttribute("ACSSAML");
            
            DocumentBuilder docBuilder;
            Document doc = null;
            DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory.newInstance();
            docBuilderFactory.setIgnoringElementContentWhitespace(true);
            docBuilder = docBuilderFactory.newDocumentBuilder();
            byte[] xmlDATA = data.getBytes();
            
            ByteArrayInputStream in = new ByteArrayInputStream(xmlDATA); 
            doc = docBuilder.parse(in);
            doc.getDocumentElement().normalize();
            
            // Iterate the child nodes of the doc.
            NodeList list = doc.getChildNodes();
    
            for (int i=0; i < list.getLength(); i++)
            {
                displaySAMLInfo(list.item(i), "", out);
            }
        }
        catch (Exception e) 
        {
            out.println("Exception encountered.");
            e.printStackTrace();
        }
        
        %>
    </body>
    </html>

## <a name="run-the-application"></a>Pokrenite aplikaciju

1. Pokretanje aplikacije u emulator računala ili implementacija Azure, pomoću koraka navedenih u [način provjere autentičnosti Web korisnicima s Azure pristup kontrola servisa pomoću Eclipse][].
2. Pokretanje preglednika, a zatim otvorite web-aplikacije. Kada se prijavite u aplikaciji, vidjet ćete SAML podatke, uključujući pridruživanju sigurnosti koje ste dobili od davatelja identiteta.

## <a name="next-steps"></a>Daljnji koraci

Da biste dodatno Istraživanje ACS na funkcionalnost i Eksperimentirajte s više sofisticirane scenariji, potražite u članku [2.0 Service kontrole programa Access][].

[Prerequisites]: #pre
[Modify the JSP file to display SAML]: #modify_jsp
[Add the JspWriter library to your build path and deployment assembly]: #add_library
[Run the application]: #run_application
[Next steps]: #next_steps
[Servis za kontrolu pristupa 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Upute za provjeru autentičnosti korisnika weba uslugom kontrola pristupa Azure pomoću Eclipse]: ../active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
 