# PwnSSRF

A Python based scanner to find potential SSRF parameters in a web application.

## Motivation
SSRF being one of the critical vulnerability out there in web application, I saw there was no tool which would automate finding potential
vulnerable parameters. PwnSSRF can be added to your arsenal for recon while doing bug hunting/web security testing.

 
## Screenshots 
![Example](example/pwnssrf.png)

## Tech/framework used
<b>Built with</b>
- `Python3`

## Features
1) Takes burp's sitemap as input and parses file with a strong regex matches any GET/POST URL parameters containing potentially vulnerable SSRF keywords like URL/website etc. Also,
Checks the parameter values for any URL or IP address passed.
Examples
GET request -<br/>
google.com/url=https://yahoo.com <br/>
google.com/q=https://yahoo.com <br/>
FORMS -<br/> `<input type="text" name="url" value="https://google.com" placeholder="https://msn.com">`
<br/><br/>
2) Multi-threaded In-built crawler to run and gather as much data as possible to parse and identify potentially vulnerable SSRF parameters.
<br/><br/>
3) Supply cookies for an authenticated scanning.
<br/><br/>
4) By default, normal mode is On, with a verbose switch you would see the same vulnerable param in different endpoints. The same parameter may not be sanitized at all places. But verbose mode generates a lot of noise.
Example:<br/>
https://google.com/path/1/urlToConnect=https://yahoo.com <br/>
https://google.com/differentpath/urlToConnect=https://yahoo.com
<br/><br/>
5) Exploitation - Makes an external request to burp collaborator or any other http server with the vulnerable parameter to confirm the possibility of SSRF. 
<br/><br/>

## How to use?
[-] This would run with default threads=10, no cookies/session and NO verbose mode <br/>
`python3 pwnssrf.py -H https://www.google.com`


[-] Space separate Cookies can be supplied for an authenticated session crawling <br/>
`python3 pwnssrf.py -H https://www.google.com -c cookie_name1=value1 cookie_name2=value2`


[-] Supplying no. of threads and verbose mode (Verbose Mode Is Not Recommended If You Don't Want To Spend Longer Time But The 
Possibility Of Bug Finding Increases)<br/>
`python3 pwnssrf.py -H https://www.google.com -c cookie_name1=value1 cookie_name2=value2 -t 20 -v`

By Default, normal mode is On, with verbose switch you would see the same potential vulnerable param in different endpoints. 
(Same parameter may not be sanitized at all places. But verbose mode generates a lot of noise.)
<br/>Example: <br/>
https://google.com/abc/1/urlToConnect=https://yahoo.com <br/>
https://google.com/123/urlToConnect=https://yahoo.com

## Version-2 (Best Recommended)
 Burp Sitemap (<b>-b switch</b>) & Connect back automation (<b> -p switch </b>) <br/><br/>
 <b>Complete Command would look like this - </b> <br/>
 `python3 pwnssrf.py -H https://www.google.com -c cookie_name1=value1 cookie_name2=value2 -b burp_file.xml -p http://72.72.72.72:8000`

[-] <b>-b switch</b> Provide burp sitemap files for a better discovery of potential SSRF parameters. The script would first parse the burp file and try to identify potential params and then run the built in crawler on it <br/><br/>
Browser the target with your burpsuite running at the background, make some GET/POST requests, the more the better. Then go to target, right click-> "Save selected Items" and save it. Provide to the script as follows. <br/>
`python3 pwnssrf.py -H https://www.google.com -c cookie_name1=value1 cookie_name2=value2 -b burp_file.xml`

</br>![alt text](https://user-images.githubusercontent.com/18059590/61342249-6a644a00-a7fe-11e9-87e8-3b26305cd8b5.png))


[-] <b>-p switch</b> Fire up burpsuite collaborator and pass the host with -p parameter Or start a simple python http server and wait for the 
vulnerable param to execute your request. <b>(Highly Recommended)</b><br/>
(This basically helps in exploiting GET requests, for POST you would need to try to exploit it manually)<br/>
Payload will get executed with the param at the end of the string so its easy to identify which one is vulnerable.
For example: http://72.72.72.72:8000/vulnerableparam <br/>

`python3 pwnssrf.py -H https://www.google.com -c cookie_name1=value1 cookie_name2=value2 -p http://72.72.72.72:8000`

![alt text](https://github.com/pwn0sec/pwnssrf/blob/master/example/Pwnssrf2.png)


## Installation
`git clone https://github.com/pwn0sec/pwnssrf.git`<br/>
`cd PwnSSRF/`<br/>
`pip3 install BeautifulSoup4`<br/>
`pip3 install requests`

## Tests
A basic framework has been created. 
More tested would be added to reduce any false positives.


## Contribute
- Report bugs.
- Suggestions for improvement.
- Suggestions for future extensions.

## Future Extensions
- Include more places to look for potential params like Javascript files
- Finding potential params during redirection.
- More conditions to avoid false positives.
- Exploitation. 


## License
GNUV3 © [Pwn0sec]

Twitter - https://twitter.com/pwn0sec
