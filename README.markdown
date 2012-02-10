#1. Name
Pudding
#2. Author
Roelof Temmingh
#3. License, version & release date
License : GPLv2  
Version : v1.0  
Release Date : 2001/01/11 

#4. Description
1. IDS evasion for web-based exploits via encoding built into a "proxy"
so that any web aware program/exploit/scanner can bypass an IDS without
modification the original code.
2. random UTF-8 encoding

I always wanted a tool that will save me the time to recode a HTTP exploit
or scanner to use a certain IDS evasion method. Its fine to see that
whisker's IDS evasion method 1 (hex encoding) works to bypass a certain
IDS, but now you still have to recode your exploit's HTTP request in hex.

I wanted a type of proxy that will encode the HTTP request for ANY scanner
or exploit. Pudding is such a tool. It supports most of RFP's IDS evasion
encoding methods, and I have added random UTF-8 encoding support.
#5. Usage
##5.1 Example
[exploit]--not encoded-->[(nc)-proxy]--encoded-->IDS -->[target]

Parameters for the proxy is as follows:

> ./pudding listenport:targetIP:targetport:mode

After execution, pudding will use netcat (nc) to listen on port 
< listenport >. When a connection is made it will execute the PERL script
stealth.pl. According to the < mode > parameter, stealth.pl will encode the
request and send it to < targetIP > on < targetport >. The reply will be sent
via the PERL script back to netcat and thus back to the exploit or scanner.

(lets use RFP's popular RDS exploit with random UTF-8 encoding)

> ./pudding 80:160.xxx.xxx.98:80:7 &
> [1] 23689
> ./pudding 80:160.xxx.xxx.98:80:7 & 
> [2] 23697
> ./pudding 80:160.xxx.xxx.98:80:7 & 
> [3] 23705

> perl rfp.orig.pl -h 127.0.0.1

-- RDS smack v2 - rain forest puppy / ADM / wiretrip --
Type the command line you want to run (cmd /c assumed):  
cmd /c echo

Step 1: Trying raw driver to btcustmr.mdb
> winnt -> c: d: e: f: g: h: 
> winnt35 -> c: d: e: f: g: h: 
> winnt351 -> c: d: e: f: g: h: 
> win -> c: d: e: f: g: h: 
> windows -> c: d: e: f: g: h: 

Step 2: Trying to make our own DSN...
> Making DSN: c: <<fail>>

Step 3: Trying known DSNs.....AdvWorks: Success!
> killall -9 perl
> killall -9 nc

##5.2 Encoding methods that pudding do:

      mode 0 clear (no encoding) for testing
      mode 1 all UPPERCASE
      mode 2 hex encoding
      mode 3 /./ directory insertion
      mode 4 fake parameter
      mode 5 premature URL ending
      mode 6 windows delimeter
      mode 7 random UTF8 encoding

(see RFP's documentation on whisker's IDS evasion methods - I butchered 
it from there anyway)

##5.3 Multiple Connections

As browsers and scanners (and some exploits) use multiple connections,
pudding needs to fork for each request. PERL is not as fast as C, and
there for you will need to start a few instances of pudding for programs 
that needs multiple connections (think of Apache and Squid that fire up
a few children to handle the load - same thing here).

##5.4 Note
1. By default debugging is switched ON. Logs of the encoded HTTP requests are
kept in /tmp/debug. Its fun to see the same HTTP request in different forms
when using UTF-8. Good luck to the IDSs out there...to switch debugging off 
uncomment it...duh!

2. UTF8 encoding only works with IIS and enabled version of Apache. Don't
try to use mode 7 on (most) Unix boxes. Oh..and don't try to use the Unicode
exploit with UTF8 encoding..that's just silly (it already encoded, so you will
be re-encoding it).

3. You can use DNS names as < targetIP >. It does not have to be an IP. 

4. Cleanup is a bit messy. You have to kill the processes afterwards manually.
Make sure you do because its not nice having differently configured instances
of pudding (and thus nc) listening on the same port. It will break. Really.

5. The utf8.db file is compiled using only codes for lowercase characters. 
There is no reason not to use the uppercase characters and symbols as well - 
I leave it as an excerise to the reader :)

6. Try this - set pudding up to a website, and hit 127.0.0.1 in your browser.
You will need multiple instance of pudding. Set encoding to UTF8. Note the 
logs on the server. So you don't want them to know where you are surfing? :)

7. The code is MeSsY. Its just concept code - the programming boffs out there
might want to totally redo it. It even uses "goto"...yuk! Anyone is welcome
to change the code - just give me credit somewhere.

8. With VERY little trouble pudding can be changed to function as a proper
proxy that can be used from a browser. See point 6.
#6. Requirements
Perl
#7. Thanks 
Thanks to the people at Sensepost. I promise I won't read your email again.
Thanks to Rain Forest Puppy (RFP). I took a lot of your code.
Thanks to Eric Hacker - your Securityfocus document "IDS Evasion with Unicode"
inspired the Unicode encoding. 
