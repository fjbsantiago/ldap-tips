# ldap-tips
Cheat cheat for LDAP tasks

#### Listing DN in cn=config
```
sudo ldapsearch  -Y EXTERNAL -H ldapi:/// -b cn=config dn
...
# {1}hdb, config
dn: olcDatabase={1}hdb,cn=config
...

sudo ldapsearch  -Y EXTERNAL -H ldapi:/// -b cn=config 'olcDatabase={1}hdb'

# {1}hdb, config
dn: olcDatabase={1}hdb,cn=config
objectClass: olcDatabaseConfig
objectClass: olcHdbConfig
olcDatabase: {1}hdb
olcDbDirectory: /var/lib/ldap
olcSuffix: dc=example,dc=com
olcAccess: {0}to attrs=userPassword,shadowLastChange by self write by anonymou
 s auth by dn="cn=admin,dc=example,dc=com" write by * none
olcAccess: {1}to dn.base="" by * read
olcAccess: {2}to * by self write by dn="cn=admin,dc=example,dc=com" write by * read
olcLastMod: TRUE
olcRootDN: cn=admin,dc=example,dc=com
olcRootPW: {SSHA}_skip_
olcDbCheckpoint: 512 30
olcDbConfig: {0}set_cachesize 0 2097152 0
olcDbConfig: {1}set_lk_max_objects 1500
olcDbConfig: {2}set_lk_max_locks 1500
olcDbConfig: {3}set_lk_max_lockers 1500
olcDbIndex: objectClass eq
```

#### Example ACL for group with olcAccess
```
dn: olcDatabase={1}hdb,cn=config
changetype: modify
delete: olcAccess
-
add: olcAccess
olcAccess: to attrs=userPassword,shadowLastChange by self write by dn="cn=admin,dc=example,dc=com" write by anonymous auth by * read
olcAccess: to * by self write by dn="cn=admin,dc=example,dc=com" write by * read
olcAccess: to dn.base="cn=user,ou=groups,dc=example,dc=com" by * read
```

#### Loading ACL from .ldif file and applying it
```
ldapmodify  -Y EXTERNAL -H ldapi:/// -f /tmp/group_acl.ldif 
```
