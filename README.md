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
## Create Conversation service

```
cf cs conversation free cb0
```
