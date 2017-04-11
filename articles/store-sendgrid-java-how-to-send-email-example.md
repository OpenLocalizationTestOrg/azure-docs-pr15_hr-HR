<properties 
    pageTitle="Store-sendgrid-Java-How-to-Send-email-example" 
    description="Upute za slanje e-pošte SendGrid iz Java u implementacije sustava Azure" 
    services="" 
    documentationCenter="java" 
    authors="thinkingserious" 
    manager="sendgrid" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="vibhork;dominic.may@sendgrid.com;elmer.thomas@sendgrid.com"/>

# <a name="how-to-send-email-using-sendgrid-from-java-in-an-azure-deployment"></a>Upute za slanje e-pošte SendGrid iz Java Azure implementacije

Sljedeći primjer pokazuje kako koristiti SendGrid za slanje poruke e-pošte s web-stranice smješten u Azure. Rezultat aplikacije tražit će od korisnika vrijednosti e-pošte, kao što je prikazano na sljedećem zaslonu snimka.

![Obrazac za e-pošte][emailform]

Rezultat e-pošte izgledat će slično kao na sljedećem zaslonu snimka.

![Poruke e-pošte][emailsent]

Morat ćete biste pomoću koda u ovoj temi, učinite sljedeće:

1. Na primjer nabavite staklenke javax.mail od <http://www.oracle.com/technetwork/java/javamail/index.html>.
2. Dodajte u staklenke vašem putu Java Sastavi.
3. Ako koristite Eclipse da biste stvorili ovu aplikaciju Java SendGrid biblioteke možete uključiti u datoteci aplikacije implementacije (WAR) pomoću značajke za sastavljanje Eclipse na implementaciju. Ako se ne koriste Eclipse da biste stvorili ovu aplikaciju Java, provjerite je li biblioteke su uvrštena isti Azure uloga kao Java aplikacije i dodati put klasa aplikacije.


Morate imati vlastitu SendGrid korisničko ime i lozinku, da biste mogli slati e-pošte. Za početak rada s SendGrid, potražite [u](store-sendgrid-java-how-to-send-email.md)članku slanje e-pošte SendGrid iz Java.

Uz to, poznavanje s informacije na [Stvaranje pozdrav svijeta aplikacije za Azure u Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944)ili drugih tehnika za hostiranje Java aplikacije u Azure ako se ne koriste Eclipse, preporučujemo.

## <a name="create-a-web-form-for-sending-email"></a>Stvaranje web-obrasca za slanje e-pošte

Sljedeći kod prikazuje kako stvoriti web-obrasca za dohvaćanje podataka korisnika za slanje e-pošte. Za potrebe ovog sadržaja, JSP naziva **emailform.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Send this email</b>.</p>
     <br/>
      <form action="sendemail.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="emailTo">
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="emailFrom">
           </td>
         </tr>
         <tr>
           <td>Subject:</td>
           <td><input type="text" size=100 name="emailSubject" value="My email subject">
           </td>
         </tr>
         <tr>
           <td>Text:</td>
           <td><input type="text" size=400 name="emailText" value="Hello,<p>This is my message.</p>Thank you." />
           </td>
         </tr>
         <tr>
           <td>SendGrid user name:</td>
           <td><input type="text" name="sendGridUser">
           </td>
         </tr>
         <tr>
           <td>SendGrid password:</td>
           <td><input type="password" name="sendGridPassword">
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Send this email">
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-send-the-email"></a>Stvaranje koda za slanje e-pošte

Sljedeći kod koji se zove se nakon dovršetka obrasca u emailform.jsp stvara poruku e-pošte i šalje ga. Za potrebe ovog sadržaja, JSP naziva **sendemail.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" import="javax.activation.*, javax.mail.*, javax.mail.internet.*, java.util.Date, java.util.Properties" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email processing happens here</title>
    </head>
    <body>
        <b>This is my send mail page.</b><p/>
     <%
     
     final String sendGridUser = request.getParameter("sendGridUser");
     final String sendGridPassword = request.getParameter("sendGridPassword");
     
     class SMTPAuthenticator extends Authenticator
     {
       public PasswordAuthentication getPasswordAuthentication()
       {
            String username = sendGridUser;
            String password = sendGridPassword;
          
            return new PasswordAuthentication(username, password);   
       }
     }
     try
     {
         
         // The SendGrid SMTP server.
         String SMTP_HOST_NAME = "smtp.sendgrid.net";
    
         Properties properties;
        
         properties = new Properties();
         
         // Specify SMTP values.
         properties.put("mail.transport.protocol", "smtp");
         properties.put("mail.smtp.host", SMTP_HOST_NAME);
         properties.put("mail.smtp.port", 587);
         properties.put("mail.smtp.auth", "true");
         
         // Display the email fields entered by the user. 
         out.println("Value entered for email Subject: " + request.getParameter("emailSubject") + "<br/>");        
         out.println("Value entered for email      To: " + request.getParameter("emailTo") + "<br/>");
         out.println("Value entered for email    From: " + request.getParameter("emailFrom") + "<br/>");
         out.println("Value entered for email    Text: " + "<br/>" + request.getParameter("emailText") + "<br/>");
    
         // Create the authenticator object.
         Authenticator authenticator = new SMTPAuthenticator();
         
         // Create the mail session object.
         Session mailSession;
         mailSession = Session.getDefaultInstance(properties, authenticator);
         
         // Display debug information to stdout, useful when using the
         // compute emulator during development.
         mailSession.setDebug(true);
    
         // Create the message and message part objects.
         MimeMessage message;
         Multipart multipart;
         MimeBodyPart messagePart; 
         
         message = new MimeMessage(mailSession);
         
         multipart = new MimeMultipart("alternative");
         messagePart = new MimeBodyPart();
         messagePart.setContent(request.getParameter("emailText"), "text/html");
         multipart.addBodyPart(messagePart);            
    
         // Specify the email To, From, Subject and Content. 
         message.setFrom(new InternetAddress(request.getParameter("emailFrom")));
         message.addRecipient(Message.RecipientType.TO, new InternetAddress(request.getParameter("emailTo")));
         message.setSubject(request.getParameter("emailSubject")); 
         message.setContent(multipart);
         
         // Uncomment the following if you want to add a footer.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"footer\": {\"settings\": {\"enable\":1,\"text/html\": \"<html>This is my <b>email footer</b>.</html>\"}}}}");
    
         // Uncomment the following if you want to enable click tracking.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"clicktrack\": {\"settings\": {\"enable\":1}}}}");
         
         Transport transport;
         transport = mailSession.getTransport();
         // Connect the transport object.
         transport.connect();
         // Send the message.
         transport.sendMessage(message,  message.getRecipients(Message.RecipientType.TO));
         // Close the connection.
         transport.close();
     
        out.println("<p>Email processing completed.</p>");
         
     }
     catch (Exception e)
     {
         out.println("<p>Exception encountered: " + 
                            e.getMessage()     +
                            "</p>");   
     }
    %>
    
    </body>
    </html>

Osim slanja e-pošte, emailform.jsp daje rezultat za korisnika; primjer je snimka zaslona za sljedeće:

![Slanje pošte rezultata][emailresult]

## <a name="next-steps"></a>Daljnji koraci

Implementacija aplikacije da biste emulator računalnim i preglednika pokrenuti emailform.jsp unesite vrijednosti u obliku, kliknite **Slanje poruke e-**, a zatim pogledajte rezultate u sendemail.jsp.

Da bi se prikazala upute za korištenje SendGrid u Java na Azure nije naveden kod. Prije no što implementirate Azure u proizvodnje, preporučujemo vam da biste dodali više pogreškama ili druge značajke. Ako, na primjer: 

* Možete koristiti Azure prostora za pohranu blob-ova ili SQL baze podataka za pohranu adrese e-pošte i poruke e-pošte, umjesto korištenja web-obrasca. Informacije o korištenju Azure prostora za pohranu blob-ova u Java, potražite u članku [upute za korištenje servisa spremište blobova platforme Java](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/). Informacije o korištenju baze podataka SQL u Java potražite u članku [Korištenje baze podataka SQL u Java](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).
* Možete koristiti `RoleEnvironment.getConfigurationSettings` dohvatiti SendGrid korisničko ime i lozinka iz implementaciju sustava konfiguracijske postavke, umjesto korištenja web-obrasca za dohvaćanje vrijednosti. Informacije o na `RoleEnvironment` klase potražite u dokumentaciji paketa izvođenje servisa Azure na <http://dl.windowsazure.com/javadoc>i [korištenja biblioteke za izvođenje servisa Azure u JSP](http://msdn.microsoft.com/library/windowsazure/hh690948) .
* Dodatne informacije o korištenju SendGrid u Java potražite [u](store-sendgrid-java-how-to-send-email.md)članku slanje e-pošte SendGrid iz Java.

[emailform]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailform.jpg
[emailsent]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailSent.jpg
[emailresult]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaResult.jpg
