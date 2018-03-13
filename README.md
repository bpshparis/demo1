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

## Build url to use to use Language Translator service and store it in URL environment variable

```
export URL=$(cf service-key cb0 user0 | awk 'NR>=2' | jq -r '.url + "/v1/workspaces"')
```

## Build credential string to use Language Translator service and store it in CRED environment variable

```
export CRED=$(cf service-key cb0 user0 | awk 'NR>=2' | jq -r '.username + ":" + .password')
```

## Store Conversation current version in VER environment variable

```
export VER=2018-02-16
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

## Create a workspace based on JSON wks0.json input

```
curl -H "Content-Type: application/json" -X POST -u $CRED -d @wks0.json ${URL}?version=$VER
```

## Store the workspace id in WKS_ID environment variable

```
export WKS_ID=$(curl -u $CRED $URL?version=$VER | jq -r '.workspaces[0].workspace_id')
```

## Create object defining the content of the new intent

```
cat > intent0.json << EOF
{
  "intent": "bonjour",
  "examples": [
    {
      "text": "Bonjour"
    },
    {
      "text": "Salut"
    },
    {
      "text": "Yo"
    },
    {
      "text": "Kenavo"
    }
  ]
}
EOF
```

## Create the intent base on the content of the object created above

```
curl -H "Content-Type: application/json" -X POST -u $CRED -d @intent0.json ${URL}/${WKS_ID}/intents?version=$VER
```

