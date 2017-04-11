
1. U programu **Project Explorer** u Studio sa sustavom Android otvorite datoteku ToDoActivity.java i dodajte sljedeće naredbe uvoz.

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;

2. Dodati klase **ToDoActivity** na sljedeći način: 
    
        private void authenticate() {
            // Login using the Google provider.
            
            ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
    
            Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }           
                @Override
                public void onSuccess(MobileServiceUser user) {
                    createAndShowDialog(String.format(
                            "You are now logged in - %1$2s",
                            user.getUserId()), "Success");
                    createTable();  
                }
            });     
        }


    Time ste stvorili nov način rukovanja postupak provjere. Korisnik provjere autentičnosti pomoću Google prijava. Dijaloški okvir prikazat će se koja prikazuje ID korisnika čija je autentičnost provjerena. Ne možete nastaviti bez pozitivan provjere autentičnosti.

    > [AZURE.NOTE] Ako koristite davateljem identiteta osim Google, promijenite vrijednost proslijediti **Prijava** navedene metode nešto od sljedećeg: _MicrosoftAccount_, _Facebook_, _na Twitteru_ili _windowsazureactivedirectory_.

3. U načinu **onCreate** dodajte sljedeći redak koda nakon kod koji instancira na `MobileServiceClient` objekt.

        authenticate();

    U ovom poziv pokreće postupak provjere autentičnosti.

4. Premještanje preostale kod nakon `authenticate();` u metode **onCreate** nov način **createTable** , koji izgleda ovako:

        private void createTable() {
    
            // Get the table instance to use.
            mToDoTable = mClient.getTable(ToDoItem.class);
    
            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);
    
            // Create an adapter to bind the items with the view.
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);
    
            // Load the items from Azure.
            refreshItemsFromTable();
        }

9. Na izborniku za **pokretanje** kliknite **Pokreni aplikaciju** da biste pokrenuli aplikaciju i prijavite se pomoću davatelja identiteta odabranom. 

    Kada uspješno prijaviti, aplikaciju pokretati bez pogrešaka i trebali biste moći upita pozadinskog servisa, a zatim napravite ažuriranja podataka.