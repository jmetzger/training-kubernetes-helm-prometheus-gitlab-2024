# Was sollte man noch setzen / nicht setzen 

## Pods / Deployment 

### enableServiceLinks 

```
Umgebungsvariablen mit den Services setzen.
Frage ist hier: Brauche ich das für meine Applikation
-> meistens nein
```

```
#
pod:
  spec:
     enableServiceLinks: "false"
```
