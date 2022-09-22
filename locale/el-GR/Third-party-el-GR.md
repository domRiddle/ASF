# Εφαρμογές τρίτων

Η ενότητα αυτή περιλαμβάνει διάφορες προσθήκες τρίτων που έχουν γραφτεί αποκλειστικά (ή κυρίως) για χρήση μαζί με το ASF. Κυμαίνονται από ASF plugins, μέχρι απλών διαδικτυακών εφαρμογών και εσωτερικών βιβλιοθηκών για ενσωμάτωση, που τελειώνουν με πλήρη χαρακτηριστικά bots για άλλες πλατφόρμες. Αν θέλετε να προσθέσετε κάτι στη λίστα, ενημερώστε μας στο Discord ή στην ομάδα στο Steam.

Λάβετε υπόψη ότι τα ακόλουθα προγράμματα **δεν** συντηρούνται από προγραμματιστές ASF και ως εκ τούτου **δεν δίνουμε καμία εγγύηση για κανένα**, ειδικά όσον αφορά τους όρους ασφάλειας, την ασφάλεια ή τη συμμόρφωση με το Steam ToS. Ο κατάλογος αυτός διατηρείται μόνο για αναφορά. Θα πρέπει πάντα να βεβαιωθείτε ότι το βοηθητικό πρόγραμμα τρίτων που είστε έτοιμος να χρησιμοποιήσετε είναι αρκετά νόμιμο για εσάς, όπως χρησιμοποιείτε όλα αυτά με δική σας ευθύνη.

---

## ASF plugins

### **[Ryzhehvost](https://github.com/Ryzhehvost)**

- **[ASF-Achievement-Manager](https://github.com/Ryzhehvost/ASF-Achievement-Manager)**, plugin για το ASF που σας επιτρέπει να διαχειριστείτε τα Steam achievements.
- **[BoosterCreator](https://github.com/Ryzhehvost/BoosterCreator)**, πρόσθετο για την προσθήκη ASF λειτουργικότητας της δημιουργίας ενισχυτικών πακέτων.
- **[Case-Insensitive-ASF](https://github.com/Ryzhehvost/Case-Insensitive-ASF)**, πρόσθετο για το ASF για να μην ελέγχει κεφαλαία στα ονόματα IoT.
- **[Commandless-Redeem](https://github.com/Ryzhehvost/Commandless-Redeem)**, πρόσθετο για το ASF για επαναφορά κλειδιού εξαργύρωσης χωρίς εντολή.
- **[ItemDispenser](https://github.com/Ryzhehvost/ItemDispenser)**, πρόσθετο για ASF να δέχεται αυτόματα το αίτημα συναλλαγής για ορισμένο(ους) τύπο(ους) αντικειμένου(ων).
- **[Selective-Loot-and-Transfer-Plugin](https://github.com/Ryzhehvost/Selective-Loot-and-Transfer-Plugin)**, πρόσθετο για ASF, παρέχει προχωρημένες `transfer` εντολές για μεταφορά αντικειμένων Steam.

### **[Vital7](https://github.com/Vital7)**

- **[FriendAccepter](https://github.com/Vital7/FriendAccepter)**, πρόσθετο για ASF να αποδέχεται αυτόματα όλες τις προσκλήσεις φίλου.
- **[GameRemover](https://github.com/Vital7/GameRemover)**, πρόσθετο για ASF εφαρμογή μιας εντολής για να αφαιρέσετε άδειες Steam για επιλεγμένες περιπτώσεις bot.
- **[GetEmail](https://github.com/Vital7/GetEmail)**, πρόσθετο για ASF εφαρμογή μιας εντολής για τη λήψη της διεύθυνσης ηλεκτρονικού ταχυδρομείου των δοσμένων παρουσιών bot απευθείας από το Steam.
- **[ResetAPIKey](https://github.com/Vital7/ResetAPIKey)**, πρόσθετο για ASF, εντολή για να επαναφέρετε το κλειδί API για επιλεγμένες παρουσίες bot.
- **[SteamKitProxyInjection](https://github.com/Vital7/SteamKitProxyInjection)**, πρόσθετο για ASF που επιτρέπει proxifing συνδέσεις WebSocket.

### Άλλο

- **[ASFEnhance](https://github.com/chr233/ASFEnhance)**, πρόσθετο για ASF, για ενίσχυση με διάφορα νέα χαρακτηριστικά, ειδικά εντολές.

---

## Ενσωματώσεις

- **[ASFBot](https://github.com/dmcallejo/ASFBot)**, telegram bot γραμμένο σε python με ενσωμάτωση ASF.
- **[ASF STM userscript](https://greasyfork.org/en/scripts/404754-asf-stm)**, για όσους θέλουν να στείλουν προσφορές αυτοματοποιημένων συναλλαγών σε bots στη δικιά μας **[ASF STM λίστα](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication#public-asf-stm-listing)** μέσω web browser, χωρίς να χρησιμοποιήσετε τη λειτουργία `MatchActively` που παρέχεται από ASF.
- **[telegram-asf](https://github.com/deluxghost/telegram-asf)**, ακόμα ένα (βασικό) bot telegram γραμμένο σε python για την ενσωμάτωση του ASF.

---

## Βιβλιοθήκες

- **[ASF-IPC](https://github.com/deluxghost/ASF_IPC)**, βιβλιοθήκη python για περαιτέρω ενσωμάτωση στη διεπαφή IPC του ASF.

---

## Συσκευασία

- **[AUR repo #1](https://aur.archlinux.org/packages/asf)**, επιτρέποντάς σας να εγκαταστήσετε εύκολα το ASF σε arch linux.
- **[AUR repo #2](https://aur.archlinux.org/packages/archisteamfarm-bin)**, επιτρέποντάς σας να εγκαταστήσετε εύκολα το ASF σε arch linux.
- **[Homebrew](https://formulae.brew.sh/formula/archi-steam-farm)**, επιτρέποντάς σας να εγκαταστήσετε εύκολα το ASF στο macOS.
- **[Nix](https://search.nixos.org/packages?channel=unstable&show=ArchiSteamFarm&from=0&size=50&sort=relevance&type=packages&query=ArchiSteamFarm)**, επιτρέποντάς σας να εγκαταστήσετε εύκολα το ASF σε distros με Nix.
- **[NixOS](https://search.nixos.org/options?channel=unstable&from=0&size=50&sort=relevance&type=packages&query=ArchiSteamFarm)**, επιτρέποντάς σας να ρυθμίσετε και να εγκαταστήσετε το ASF με NixOS.

---

## Εργαλεία

- **[Keys extractor](https://ske.xpixv.com)**, σας επιτρέπει να αντιγράψετε τα κλειδιά σε διάφορες μορφές και να δημιουργήσετε `redeem` εντολή για το ASF. Ελέγξτε το **[GitHub repo](https://github.com/PixvIO/SKE)** για περισσότερες λεπτομέρειες.
- **[ASF Mass Config Editor](https://github.com/genesix-eu/ASF_MCE)**, το οποίο επιτρέπει την εύκολη διαχείριση πολλαπλών ASF ρυθμίσεων.

---

## Θέλετε να βρείτε περισσότερα;

Συνιστούμε τη συζήτηση **[ArchiSteamFarm](https://github.com/topics/archisteamfarm)** στο GitHub για όλα τα έργα που ενσωματώνονται με ASF.