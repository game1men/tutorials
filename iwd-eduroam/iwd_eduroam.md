# nastavení eduroam

Prvně je potřeba stáhnout script tady: https://cat.eduroam.org/

Následně na úplnej konec scriptu přidat tyto řádky, které se postarají o vypsání potřebných údajů:

```
print(Config.eap_outer);
print(Config.anonymous_identity);
print(Config.CA);
print(Config.servers);
print(Config.eap_inner);
print(Config.user_realm);
```

Spustit script, a vyplnit údaje. Na konci by to mělo vypsat údaje co jsou potřeba vyplnit v config souboru. (další popis proměnných zde https://wiki.archlinux.org/title/iwd)
Certifikát by to mělo vytvořit ve složce "~/.config/cat_installer/ca.pem" tenhle jsem překopíroval do "/var/lib/iwd/" abych měl jistotu že k tomu má iwd přístup.

Dále je potřeba vytvořit config soubor, který se jmenuje nazevWifi.8021x ve "/var/lib/iwd/" a dát do něj následující obsah s údaji z předchozího scriptu.

```
[Security]
EAP-Method=PEAP
EAP-Identity=<Config.anonymous_identity>
EAP-PEAP-CACert=(cesta k certifikátu)
EAP-PEAP-ServerDomainMask=<Config.servers> (jeden)
EAP-PEAP-Phase2-Method=<Config.eap_inner>
EAP-PEAP-Phase2-Identity=<username@Config.user_realm>
EAP-PEAP-Phase2-Password=<heslo k eduroam>

[Settings]
AutoConnect=true
```

Teď by se mělo jít připojit k wifi po restartování iwd `systemctl restart iwd`.

