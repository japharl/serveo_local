# serveo_local - no https

Serveo.net is a way to ssh and redirect ports on the internet. These are instructions to get your local copy of the serveo server application running, which is not depedent upon serveo.net.

On a server:

0. Get a digital oceans account.  (This is only the site I used; you can use your own virtual machine provider, like aws or similar, but you may need to perform additional configuration details not covered, such as firewall configuration or simlar.)
1. Create a small ($5 / month) ubuntu dropplet.  (Optional https://m.do.co/c/674f3906831a referal link).  
2. ssh in to the box ( ssh as root, then use the ssh key you created or use the root password mailed to your account).
3. In a terminal: sudo apt update ; sudo apt upgrade -y ; sudo apt dist-upgrade -y ; sudo apt autoremove -y ; sudo reboot
4. create for a new domain on domains.google.com or similar.  (eg. serveoish.com or similar).  
5. Create a wild card A record ( * ) for the domain above to point to the ip address created by digital ocean.
6. wget https://storage.googleapis.com/serveo/download/2018-05-08/serveo-linux-amd64 -O serveo  # This url is the 64 bit linux serveo.
7. chmod 777 serveo
8. ./serveo  # Should result in an "failed to load private key" error message.
9. ssh-keygen -t rsa -f ssh_host_rsa_key
10. ./serveo --port=2222 -domain=serveoish.com #  Should result in something like: 2019/09/08 19:15:02 Listening on :2222...
11. Keep this window open while serveo runs.  Or use nohup ./serveo --port=2222 -domain=serveoish.com &

In a client:

1. assume you have a web server installed.  (If not, see https://jameshfisher.com/2018/12/31/how-to-make-a-webserver-with-netcat-nc/ and follow the how to make a persistant server step below , orrrr...:  python -m SimpleHTTPServer
2. ssh <digitalocean ip> -p 2222 -R 80:localhost:8000

You will see text like:
<pre>
Warning: no TLS certificate available for benigne.serveoish.com. You won't be able to use HTTPS, only HTTP.
Forwarding HTTP traffic from http://benigne.serveoish.com
Press g to start a GUI session and ctrl-c to quit.
</pre>

(Or use other appropriate ssh command here for a remote ssh tunnel or similar.)

To shutdown serveo server, just press control c in the server window.

# To enable https via lets encrypt with gooogle domains as the provider:

Does not seem to be working... Looks like serveo isn't recognizing the certs provided by acme...

0. Perform steps 1-9 above in the no-https section.  We are then going to install acme.sh 
1. curl https://get.acme.sh | sh
2. sudo apt install socat 
3. On the digital ocean screen, go to Manage -> api.
4. Generate a new api token.
5. Copy the api token to the keyboard.
6. Create a new script.sh, starting with:
<pre>
export DO_API_KEY=(key from website)
/root/.acme.sh/acme.sh --issue --dns dns_dgon -d serveoish.com -d *.serveoish.com
./serveo  -port=2222 --tcp_port_lower_bound 3000 --domain=serveoish.com --private_key_path=/root/.acme.sh/serveoish.com/serveoish.com.key
</pre>
7. Then: bash -x ./script.sh 
