Note - these instructions DO NOT WORK at present.  

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
