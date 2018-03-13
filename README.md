# demo1

## Log to IBM Cloud in Germany
```
cf l -a https://api.eu-de.bluemix.net -u sebastien.gautier@fr.ibm.com --skip-ssl-validation -s devde -o sebastien.gautier@fr.ibm.com
```

> If **login failed** then get a one time code here:
https://login.eu-de.bluemix.net/UAALoginServerWAR/passcode
and login with **--sso**

```
cf l -a https://api.eu-de.bluemix.net -u sebastien.gautier@fr.ibm.com --sso -s devde -o sebastien.gautier@fr.ibm.com
```
> Paste one time code when prompt
```
One Time Code (Get one at https://login.eu-de.bluemix.net/UAALoginServerWAR/passcode)>
```
> and hit enter.

## Dump marketplace to get service name, plan and description

```
cf marketplace | tee marketplace.de
```

## Get name and plan for Speech to Text service

```
grep -i conversation marketplace.de
```

## Creating Conversation service instance cb0

```
cf cs conversation free cb0
```

## Creating service key user0 for service instance cb0

```
cf csk cb0 user0
```

## Create CreateWorkspace object defining the content of the new workspace

```
cat > wks0.json << EOF
{
  "name": "wks0",
  "intents": [],
  "entities": [],
  "dialog_nodes": [],
  "language": "fr",
  "description": "workspace 0"
}
EOF
```



