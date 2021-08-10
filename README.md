# JustBreakIn
JustBreakIn, a cheat sheet which will aid you through external network engagements.

## Performing OSINT to break into an external portal such as OWA.

First we perform an OSINT on "TBA" using `theHarvester`. 

(Please note: some results were skewed to allow the guide to flow well).
```
TBA
```

Next we add the user output to a file:
```
TBA
```

We then perform some `cut` to get the names in a clean output:
```
TBA
```

Next we clone namemash and give it execute permissions:

```sudo git clone https://gist.github.com/superkojiman/11076951 /opt/namemash; sudo chmod +x /opt/namemash/namemash.py```

We then execute namemash and save the output to `names.txt`:

```./namemash.py employees.txt > names.txt```

Finally we use namemash and sed to get full email addresses outputted to `possible-emails.txt`:

```./namemash.py names.txt| sed 's/$/@tba.com/g' > possible-emails.txt```



