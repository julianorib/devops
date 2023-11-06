# For and While
```
for ip in $(cat ips.txt); do ssh root@"$ip" yum -y update; done
```
```
for teste in $(cat teste.txt); do mkdir -p temp"$teste" ; mv $teste temp"$teste"/ ; done 
```
```
for url in $(cat urls.txt); do echo $url >> result.txt ; curl -I "$url" | grep HTTP | awk '{print $2}' >> result.txt ; done 
```
```
for i in $(ls); do touch $i/backup_$i.bkp ; done
```
``` 
while true; do dig  pos.bluead.com.br | egrep "SERVER:|Query|WHEN" ; sleep 0.3 ; done
```

# IF and Else

https://linuxhandbook.com/if-else-bash/

```
#!/bin/bash

if [ $(whoami) = 'root' ]; then
	echo "You are root"
fi
```