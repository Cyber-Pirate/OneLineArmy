# OneLineArmy
We call it as one line but it has a couple of lines....don't Think me bad ;)

### Before we start !
Tools I've used here :
- Httpx
- Assetfinder
- Subfinder
- Amass
- Gau
- Waybackurls
- Linkfinder
- Secretfinder

## Paste these in your .bashrc file

### Grab all the subdoamins with this

Useage : subsof domain
```
subsof(){
        echo $1 | assetfinder -subs-only | sort -u >> asub.txt;
        echo $1 | subfinder -silent | sort -u >> subf.txt;
        cat asub.txt subf.txt | sort -u >> sub.txt ;
        rm -rf asub.txt subf.txt;
        amass enum -nf sub.txt -d $1 -noalts -passive >> amass.txt;
        cat subs.txt amass.txt | sort -u >> subs.txt;
        rm -rf amass.txt sub.txt;
        cat subs.txt | httpx -silent | sed 's/https\?:\/\///' >> alive-subs.txt
}
```

### Get js files with the output of gau or waybackurls or something else

Useage : getjs allurls.txt

```
getjs(){
        cat $1 | grep '\.js$' | httpx -status-code -mc 200 -content-type -no-color -silent | grep 'application/javascript' | sed -e 's/\[application\/javascript]//g' | sed 's/\[200]//g' | tee -a js.txt
}
```

### Grab all Urls and Endpoints with Gau , Waybacurls and httpx

Useage : grab_url domain

```
grab_url(){
          echo $1 | gau -subs >> gau.txt;cat gau.txt | sort -u | httpx -silent >> alivegau.txt;
          echo $1 | waybackurls >> waybacks.txt;cat waybacks.txt | sort -u | httpx -silent >> alivewaybacks.txt;
          cat alivegau.txt alivewaybacks.txt | sort -u >> allalive.txt
}
```
### Give your js files as input here to get urls and juicy contents 
 #### Nothing fancy...Just,merging 2 tools

Useage : jsfun doamin/*.js

```
jsfun(){
        mkdir -p js;
        cat $1 | while read url; do linkfinder -d -i $url -o cli;done >> js/linkfinder.txt;
        cat $1 | while read url; do secretfinder -i $url -o cli;done >> js/secrets.txt;
}
  ```
