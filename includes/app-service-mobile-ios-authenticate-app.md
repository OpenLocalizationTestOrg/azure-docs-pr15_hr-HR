**Cilj C**: 

1. Na računalu Mac otvorili _QSTodoListViewController.m_ Xcode i dodali sa sljedećim korakom. Ako ne koristite Google kao davatelja identiteta, promijenite _google_ _microsoftaccount_, _servisa twitter_, _facebook_ili _windowsazureactivedirectory_ . Ako koristite Facebook, [morat ćete whitelist Facebook domena u svojoj aplikaciji](https://developers.facebook.com/docs/ios/ios9#whitelist).

            - (void) loginAndGetData
            {
                MSClient *client = self.todoService.client;
                if (client.currentUser != nil) {
                    return;
                }
            
                [client loginWithProvider:@"google" controller:self animated:YES completion:^(MSUser *user, NSError *error) {
                    [self refresh];
                }];
            }


2. Zamjena `[self refresh]` u `viewDidLoad` u _QSTodoListViewController.m_ sa sljedećim:

            [self loginAndGetData];

3. Pritisnite _Pokreni_ da biste pokrenuli aplikaciju, a zatim se prijavite. Kada ste prijavljeni, moći ćete prikaz popisa obveze i ništa ažurirati.

**Swift**:

1. Na računalu Mac otvorili _ToDoTableViewController.swift_ Xcode i dodali sa sljedećim korakom. Ako ne koristite Google kao davatelja identiteta, promijenite _google_ _microsoftaccount_, _servisa twitter_, _facebook_ili _windowsazureactivedirectory_ . Ako koristite Facebook, [morat ćete whitelist Facebook domena u svojoj aplikaciji](https://developers.facebook.com/docs/ios/ios9#whitelist).
        
            func loginAndGetData() {
                
                guard let client = self.table?.client where client.currentUser == nil else {
                    return
                }
                
                client.loginWithProvider("google", controller: self, animated: true) { (user, error) in
                    self.refreshControl?.beginRefreshing()
                    self.onRefresh(self.refreshControl)
                }
            }


2. Uklonite retke `self.refreshControl?.beginRefreshing()` i `self.onRefresh(self.refreshControl)` na kraju `viewDidLoad()` u _ToDoTableViewController.swift_. Dodavanje poziv `loginAndGetData()` na njihovo mjesto:

            loginAndGetData()

3. Pritisnite _pokrenuti_ da biste pokrenuli aplikaciju, a zatim se prijavite. Kada ste prijavljeni, moći ćete prikaz popisa obveze i ništa ažurirati.
