# serveo_local

0. get a digital oceans account.  (This is only the site I used; you can use your own.)
1. Create a small ($5 / month) ubuntu dropplet.  (Optional https://m.do.co/c/674f3906831a referal link).
2. ssh in to the box ( ssh as root, then use the ssh key you created or use the root password mailed to your account).
3. In a terminal: sudo apt update ; sudo apt upgrade -y ; sudo apt dist-upgrade -y ; sudo apt autoremove -y ; sudo reboot
4. create for a new domain on domains.google.com or similar.  (eg. serveoish.com or similar).  
5. Create a wild card record for the domain above to point to the ip address created by digital ocean.
6. wget https://storage.googleapis.com/serveo/download/2018-05-08/serveo-linux-amd64 -O serveo 
6a. This url is the 64 bit linux serveo.
7. chmod 777 serveo
8. ./serveo 
8a. Should result in an "failed to load private key" error message.
9. ssh-keygen -t rsa -f ssh_host_rsa_key
10. ./serveo --port=2222 -domian=serveoish.net
10a. SHould result in something like: 2019/09/08 19:15:02 Listening on :2222...


In a client:
0. assume you have a web server installed.  (If not, see https://jameshfisher.com/2018/12/31/how-to-make-a-webserver-with-netcat-nc/ and follow the how to make a persistant server step )
1. ssh serveoish.com -p 2222 -R 80:localhost:8000
Warning: no TLS certificate available for benigne.serveoish.com. You won't be able to use HTTPS, only HTTP.
Forwarding HTTP traffic from http://benigne.serveoish.com
Press g to start a GUI session and ctrl-c to quit.

(Or use other appropriate ssh command here.)

To shutdown serveo server, just press control c in the server window.
