<h1>proftpd - Installing an FTP daemon for extra accounts</h1>

        
In SSH do the commands described in this FAQ. If you do not know how to SSH into your slot use this FAQ: <a href="https://www.feralhosting.com/faq/view?question=12">SSH basics - Putty</a><br>
<br>
Your FTP &#x2F; SFTP &#x2F; SSH login information can be found on the Slot Details page for the relevant slot. Use this link in your Account Manager to access the relevant slot:<br>
<br>
<img src="https://raw.github.com/feralhosting/feralfilehosting/master/Feral%20Wiki/0%20Generic/slot_detail_link.png"><br>
<br>
You login information for the relevant slot will be shown here:<br>
<br>
<img src="https://raw.github.com/feralhosting/feralfilehosting/master/Feral%20Wiki/0%20Generic/slot_detail_ssh.png"><br>
<br>
 <strong>Important note:</strong> If you just want to find out which Ports to use for connecting, see Step 5. If did not complete the user creation&#x2F;password part of the script see Step 6 for the <code>ftpasswd</code> commands you need to 1: create your main user and 2: other jailed users.<br>
<br>
<h2>Bash script installation</h2><br>
This bash script will perform the <strong>basic set-up</strong> outlined in Steps 1,2,3,4,5,6. Including creating your main user account.<br>
<br>
 <strong>Important note:</strong> This script can also update proftpd and add or modify users. When updates become available and the script update your installation without losing any settings, jails or users you have configured.<br>
<br>
Run this command in SSH:<br>
<br>
<pre><code>wget -qO ~&#x2F;install.proftpd <a href="http://git.io/nQJBxw">http:&#x2F;&#x2F;git.io&#x2F;nQJBxw</a> &amp;&amp; bash ~&#x2F;install.proftpd</code></pre><br>
You can easily add or modify users from the installation script at any time.<br>
<br>
<pre><code>install.proftpd adduser</code></pre><br>
You can also pass a username directly to the script and it will use that automatically.<br>
<br>
<pre><code>install.proftpd adduser username</code></pre><br>
<strong>Important note:</strong> Use the option <code>help</code> for important information on start up commands, configured ports, connection info and debugging:<br>
<br>
<pre><code>install.proftpd help</code></pre><br>
<h2>Manual Installation Steps</h2><br>
Follow these steps to manually download and install proftpd. The bash script will complete Steps 1,2,3,4,5,6 for you.<br>
<br>
<h2>Step 1: Get the package and extract it:</h2><br>
<pre><code>mkdir -p ~&#x2F;proftpd&#x2F;etc&#x2F;sftp&#x2F;authorized_keys ~&#x2F;proftpd&#x2F;etc&#x2F;keys ~&#x2F;proftpd&#x2F;ssl
wget -qO proftpd.tar.gz <a href="ftp://ftp.proftpd.org/distrib/source/proftpd-1.3.5.tar.gz">ftp:&#x2F;&#x2F;ftp.proftpd.org&#x2F;distrib&#x2F;source&#x2F;proftpd-1.3.5.tar.gz</a>
tar xf ~&#x2F;proftpd.tar.gz -C ~&#x2F; &amp;&amp; rm -f ~&#x2F;proftpd.tar.gz
cd ~&#x2F;proftpd-1.3.5</code></pre><br>
<h2>Step 2: Configure and then install it:</h2><br>
<pre><code>install_user=$(whoami) install_group=$(whoami) .&#x2F;configure --prefix=$HOME&#x2F;proftpd --enable-openssl --enable-dso --enable-nls --enable-ctrls --with-shared=mod_ratio:mod_readme:mod_sftp:mod_tls:mod_ban:mod_shaper
make &amp;&amp; make install
cd &amp;&amp; rm -rf ~&#x2F;proftpd-1.3.5</code></pre><br>
<h2>Step 3.1: Download and create some required configuration files we need:</h2><br>
Do these commands to download the preconfigured configuration files.<br>
<br>
<pre><code>wget -qO ~&#x2F;proftpd&#x2F;etc&#x2F;proftpd.conf <a href="http://git.io/CbaIJQ">http:&#x2F;&#x2F;git.io&#x2F;CbaIJQ</a>
wget -qO ~&#x2F;proftpd&#x2F;etc&#x2F;sftp.conf <a href="http://git.io/SFHs5g">http:&#x2F;&#x2F;git.io&#x2F;SFHs5g</a>
wget -qO ~&#x2F;proftpd&#x2F;etc&#x2F;ftps.conf <a href="http://git.io/ee86Hw">http:&#x2F;&#x2F;git.io&#x2F;ee86Hw</a></code></pre><br>
<h2>Step 3.2: Optional Configuration:</h2><br>
Use these commands if you would like the conf files configured using SSH commands. Skip this section if you wish to do it manually.<br>
<br>
Do these next commands to configure the <code>proftpd.conf</code>.<br>
<br>
<pre><code>sed -i &#x27;s|&#x2F;media&#x2F;DiskID&#x2F;home&#x2F;my_username|&#x27;$HOME&#x27;|g&#x27; ~&#x2F;proftpd&#x2F;etc&#x2F;proftpd.conf
sed -i &#x27;s|User my_username|User &#x27;$(whoami)&#x27;|g&#x27; ~&#x2F;proftpd&#x2F;etc&#x2F;proftpd.conf
sed -i &#x27;s|Group my_username|Group &#x27;$(whoami)&#x27;|g&#x27; ~&#x2F;proftpd&#x2F;etc&#x2F;proftpd.conf
sed -i &#x27;s|AllowUser my_username|AllowUser &#x27;$(whoami)&#x27;|g&#x27; ~&#x2F;proftpd&#x2F;etc&#x2F;proftpd.conf</code></pre><br>
Do these next commands to configure the <code>sftp.conf</code>:<br>
<br>
<pre><code>sed -i &#x27;s|&#x2F;media&#x2F;DiskID&#x2F;home&#x2F;my_username|&#x27;$HOME&#x27;|g&#x27; ~&#x2F;proftpd&#x2F;etc&#x2F;sftp.conf
sed -i &#x27;s|Port 23001|Port &#x27;$(shuf -i 10001-49999 -n 1)&#x27;|g&#x27; ~&#x2F;proftpd&#x2F;etc&#x2F;sftp.conf
sed -nr &#x27;s&#x2F;^Port (.*)&#x2F;\1&#x2F;p&#x27; ~&#x2F;proftpd&#x2F;etc&#x2F;sftp.conf</code></pre><br>
Make a note of this port, This is your SFTP port.<br>
<br>
Do these next commands to configure the <code>ftps.conf</code>:<br>
<br>
<pre><code>sed -i &#x27;s|&#x2F;media&#x2F;DiskID&#x2F;home&#x2F;my_username|&#x27;$HOME&#x27;|g&#x27; ~&#x2F;proftpd&#x2F;etc&#x2F;ftps.conf
sed -i &#x27;s|Port 23002|Port &#x27;$(shuf -i 10001-49999 -n 1)&#x27;|g&#x27; ~&#x2F;proftpd&#x2F;etc&#x2F;ftps.conf
sed -nr &#x27;s&#x2F;^Port (.*)&#x2F;\1&#x2F;p&#x27; ~&#x2F;proftpd&#x2F;etc&#x2F;ftps.conf</code></pre><br>
Make a note of this port, This is your FTPS port.<br>
<br>
Once these commands have been completed the conf files have all been configured ready for use.<br>
<br>
Continue with FAQ.<br>
<br>
<h2>Step 4: Generate some keys and certs:</h2><br>
Here you need to generate the SFTP keys and&#x2F;or the SSL certs for FTPS<br>
<br>
<strong>For SFTP:</strong><br>
<br>
Pass-phrases are optional. Proftpd asks for the pass-phrases when you start the daemon if there are any.<br>
<br>
<strong>Copy and paste this command below.</strong> It is required to create our SSH keys.<br>
<br>
<pre><code>ssh-keygen -qt rsa -f ~&#x2F;proftpd&#x2F;etc&#x2F;keys&#x2F;sftp_rsa &amp;&amp; ssh-keygen -qt dsa -f ~&#x2F;proftpd&#x2F;etc&#x2F;keys&#x2F;sftp_dsa</code></pre><br>
Press enter 4 times to generate keyfiles with no passphrase. Using a Pass-phrases is optional. Proftpd will ask for the pass-phrases when you start the daemon if one was used.<br>
<br>
<strong>For FTPS:</strong><br>
<br>
<strong>Copy and paste this command below.</strong> It is required to generate the SSL certificates we need for FTPS.<br>
<br>
<pre><code>openssl req -new -x509 -nodes -days 365 -subj &#x27;&#x2F;C=GB&#x2F;ST=none&#x2F;L=none&#x2F;CN=none&#x27; -newkey rsa:2048 -keyout ~&#x2F;proftpd&#x2F;ssl&#x2F;proftpd.key.pem -out ~&#x2F;proftpd&#x2F;ssl&#x2F;proftpd.cert.pem</code></pre><br>
<h2>Step 5: The Configuration files:</h2><br>
 <strong>Important note:</strong> These configuration files just work. They do not need to changed to get the functionality described in this guide, apart from where you are directed to do so. <br>
<br>
They have been configured with 3 default <strong>READ ONLY</strong> jails:<br>
<br>
<pre><code>private&#x2F;rtorrent&#x2F;data
private&#x2F;deluge&#x2F;data
private&#x2F;transmission&#x2F;data</code></pre><br>
They are <strong>READ ONLY</strong> by default, accessible to all non main users so all you need to do is create some standard users to use them. No edits to the configuration files are required. <br>
<br>
 <strong>Important note:</strong> To use non default folders or restrict user access, please see the Jail section in <code>Step 8</code> of this FAQ for further information.<br>
<br>
These configuration files have been set-up to work in a specific way. <br>
<br>
<strong>1:</strong> The <code>proftpd.conf</code> is home to the global settings that are loaded&#x2F;included in both the <code>sftp.conf</code> and the <code>ftps.conf</code> when they are called. <br>
<br>
<strong>2:</strong> The <code>sftp.conf</code> and <code>ftps.conf</code> contain only protocol specific settings.<br>
<br>
 <strong>Important note:</strong> These files have already been downloaded, if you used the bash script or followed the commands in <code>Step 1</code>, and configured, if you used the bash script or followed the steps in <code>Step 3.2</code>. <br>
<br>
For manual set-up or altering, they can be found in the <code>~&#x2F;proftpd&#x2F;etc&#x2F;</code> of the installation directory, for example:<code>~&#x2F;proftpd&#x2F;etc&#x2F;proftpd.conf</code><br>
<br>
<h2>Manual Configuration of conf files</h2><br>
You only if you did not use the bash script or skipped using the <code>sed</code> commands in step 3.2<br>
<br>
<h2>proftpd.conf</h2><br>
In the Global Section of the <code>proftpd.conf</code><br>
<br>
<pre><code>User my_username
Group my_username</code></pre><br>
In the &lt;Limit ALL&gt; section of the <code>proftpd.conf</code> change <code>my_username</code> to your Feral username to give yourself a full access account. This is the <code>Main User</code> account.<br>
<br>
<pre><code>AllowUser my_username</code></pre><br>
These are the partial line changes you want to change with your own path in the <code>proftpd.conf</code><br>
<br>
<pre><code>&#x2F;media&#x2F;DiskID&#x2F;home&#x2F;my_username</code></pre><br>
<h3>sftp.conf</h3><br>
Change the port to a different number (between 6000 and 50000)<br>
<br>
<pre><code>Port 23001</code></pre><br>
These are the partial line changes you want to change with your own path in the <code>sftp.conf</code><br>
<br>
<pre><code>&#x2F;media&#x2F;DiskID&#x2F;home&#x2F;my_username</code></pre><br>
<h3>ftps.conf</h3><br>
Change the port to a different number (between 6000 and 50000)<br>
<br>
<pre><code>Port 23002</code></pre><br>
These are the partial line changes you want to change with your own path in the <code>ftps.conf</code><br>
<br>
<pre><code>&#x2F;media&#x2F;DiskID&#x2F;home&#x2F;my_username</code></pre><br>
To get your ports again at a later date use these commands. You may want to note these down or create aliases for them:<br>
<br>
<pre><code>echo&nbsp; &quot;SFTP port = $(sed -nr &#x27;s&#x2F;^Port (.*)&#x2F;\1&#x2F;p&#x27; ~&#x2F;proftpd&#x2F;etc&#x2F;sftp.conf)&quot;
echo&nbsp; &quot;FTPS port = $(sed -nr &#x27;s&#x2F;^Port (.*)&#x2F;\1&#x2F;p&#x27; ~&#x2F;proftpd&#x2F;etc&#x2F;ftps.conf)&quot;</code></pre><br>
<h2>Step 6: Create our main user with full access</h2><br>
This command below will automatically create a user using:<br>
<br>
<strong>1:</strong> Your Feral slot username.<br>
<br>
<strong>2:</strong> Your Feral slot user&#x27;s <code>UID</code> and <code>GID</code>. <br>
<br>
 <strong>Important note:</strong> This user is already configured in the <code>proftpd.conf</code> for full access if you used the bash script or Step 3.2. <br>
<br>
Use this command to create the main user and enter a password when prompted.<br>
<br>
<pre><code>~&#x2F;proftpd&#x2F;bin&#x2F;ftpasswd --passwd --name $(whoami) --file ~&#x2F;proftpd&#x2F;etc&#x2F;ftpd.passwd --uid $(id -u $(whoami)) --gid $(id -g $(whoami)) --home $HOME&#x2F; --shell &#x2F;bin&#x2F;false</code></pre><br>
Create a corresponding group entry (required):<br>
<br>
<pre><code>~&#x2F;proftpd&#x2F;bin&#x2F;ftpasswd --group --name $(whoami) --file ~&#x2F;proftpd&#x2F;etc&#x2F;ftpd.group --gid $(id -g $(whoami)) --member $(whoami)</code></pre><br>
 <strong>Important note:</strong> This user (with your username) will not be jailed. This is a full access account. This is for your use and not public sharing.<br>
<br>
<strong>Explanation: using <code>UID</code> and <code>GID</code>:</strong><br>
<br>
Do these commands in SSH, where <code>my_username</code> is your Feral username<br>
<br>
<pre><code>id -u my_username</code></pre><br>
<pre><code>id -g my_username</code></pre><br>
For example:<br>
<br>
<pre><code>id -u superguy</code></pre><br>
Returns:<br>
<br>
<pre><code>1032</code></pre><br>
And then:<br>
<br>
<pre><code>id -g superguy</code></pre><br>
Returns:<br>
<br>
<pre><code>1032</code></pre><br>
This will give you <strong>your</strong> <code>UID</code> and <code>GID</code>, which in this example, are both <strong>1032</strong>. When creating a user if you provide the <code>UID</code> and <code>GID</code> listed from those commands you will have your full permissions when accessing the slot. The username does not matter as long as the <code>UID</code> and <code>GID</code> match. This is important for using some programs like WinSCP or when creating an account for yourself.<br>
<br>
<h2>Step 7: Creating new users and groups</h2><br>
These users, when created, are all <strong>jailed by default</strong> based on the configuration file settings.<br>
<br>
There is a bash add user script (optional) linked near the end of this section. The users created with this script are also jailed by default.<br>
<br>
 <strong>Important note:</strong> You will get a directory listing error when connecting if your user&#x27;s <code>HOME</code>, when created, does not match an existing jail path. You can get around this by manually specifying a remote path which is supported by most FTP programs. See Step 8 of this FAQ for more info on specifying a users&#x27; root&#x2F;home directory.<br>
<br>
 <strong>Important note:</strong> There are three existing and default jail directories you can use without needing to edit the configuration files.<br>
<br>
<pre><code>private&#x2F;rtorrent&#x2F;data
private&#x2F;deluge&#x2F;data
private&#x2F;transmission&#x2F;data</code></pre><br>
If you use one of these jail paths as your user&#x27;s <code>HOME</code> or as a remote directory in your FTP program your user will already have access and get a successful directory listing when connecting.<br>
<br>
<h2>Creating or editing users in SSH using ftpasswd:</h2><br>
In the command below, replace <code>my_username</code> with the username of the user you want to add. Add your Jail path after <code>$HOME&#x2F;</code>, using only lowercase letters , for example <code>$HOME&#x2F;private&#x2F;jail</code><br>
<br>
<pre><code>~&#x2F;proftpd&#x2F;bin&#x2F;ftpasswd --passwd --name my_username --file ~&#x2F;proftpd&#x2F;etc&#x2F;ftpd.passwd --uid 5001 --gid 5001 --home $HOME&#x2F; --shell &#x2F;bin&#x2F;false</code></pre><br>
To change an existing user&#x27;s password use this command, where <code>my_username</code> is the name of the user you wish to edit:<br>
<br>
<pre><code>~&#x2F;proftpd&#x2F;bin&#x2F;ftpasswd --passwd --name=my_username --change-password --file ~&#x2F;proftpd&#x2F;etc&#x2F;ftpd.passwd</code></pre><br>
To delete an existing users use this command, where <code>my_username</code> is the name of the user you wish to remove:<br>
<br>
<pre><code>~&#x2F;proftpd&#x2F;bin&#x2F;ftpasswd --passwd --name=my_username&nbsp; --delete-user --file ~&#x2F;proftpd&#x2F;etc&#x2F;ftpd.passwd</code></pre><br>
Please see then end of this section for an easy way to add users using a bash script.<br>
<br>
<h2>Creating or editing groups in SSH using ftpasswd:</h2><br>
Now we can add that user to a group (optional):<br>
<br>
In the command below, replace <code>my_username</code> with the username of the user you want to add to the group <code>proftpdgroup</code>. You can change <code>proftpdgroup</code> to another name. This new name will be the new group name the user is added to.<br>
<br>
<pre><code>~&#x2F;proftpd&#x2F;bin&#x2F;ftpasswd --group --name proftpdgroup --file ~&#x2F;proftpd&#x2F;etc&#x2F;ftpd.group --gid 5001 --member my_username</code></pre><br>
Delete an existing group, where <code>my_group</code> is the name of the group you wish to remove:<br>
<br>
<pre><code>~&#x2F;proftpd&#x2F;bin&#x2F;ftpasswd --group --name my_group --delete-group --file ~&#x2F;proftpd&#x2F;etc&#x2F;ftpd.group</code></pre><br>
<h2>Create user bash script</h2><br>
<pre><code>wget -qO ~&#x2F;proftpdadduser.sh <a href="http://git.io/NGk2Aw">http:&#x2F;&#x2F;git.io&#x2F;NGk2Aw</a></code></pre><br>
Then this command to execute it:<br>
<br>
<pre><code>bash ~&#x2F;proftpdadduser.sh</code></pre><br>
It will find the next available ID and then ask you for a name, or you can invoke it with a name.<br>
<br>
You will need to:<br>
<br>
<strong>1:</strong> Enter a username for your new user<br>
<br>
<strong>2:</strong> Enter a relative path from the slot&#x27;s root to the users <code>HOME</code>. Do not use <code>~&#x2F;</code> in the path<br>
<br>
<strong>3:</strong> Enter a password.<br>
<br>
And that should be your new user created and ready to connect once you have started the servers in step 9.<br>
<br>
<h2>Step 8: Jail your users:</h2><br>
 <strong>Important note:</strong> Don&#x27;t forget changes made to the<code> proftpd.conf</code>, <code>sftp.conf</code> and <code>ftps.conf</code> will require the proftpd running process to restarted for the changes to take effect. See <code>Step 9</code> for restart commands.<br>
<br>
Inside the:<br>
<br>
<pre><code>## Permissions</code></pre><br>
Section of the <code>proftpd.conf</code> you will find that three <strong>READ ONLY</strong> jails have already been set up. The <code>proftpd.conf</code> uses a <strong>READ ONLY</strong> jail for all users who are not specifically named with <code>AllowUser</code> inside the <code>&lt;Limit ALL&gt;</code>section. Any users you create will be jailed by default and restricted to only the default jails.<br>
<br>
 <strong>Important note:</strong> A user&#x27;s HOME is the start location where they are placed upon connecting to the server. If they do not have a HOME that corresponds to an existing jail they will most like get a &quot;Directory listing failed&quot; or similar error. You can manually set these paths in most FTP programs, such as Filezilla (default remote directory) since having a HOME is not that important, as long as they have permission to access the remote directory set in your FTP program.<br>
<br>
<img src="https://raw.github.com/feralhosting/feralfilehosting/master/Feral%20Wiki/Software/proftpd%20-%20Installing%20an%20FTP%20daemon%20for%20extra%20accounts/remotedirectory.png"><br>
<br>
One of the existing jail examples is for <code>private&#x2F;rtorrent&#x2F;data</code> so all you need to do is create a user whose <code>--home</code> is:<br>
<br>
<pre><code>--home $HOME&#x2F;private&#x2F;rtorrent&#x2F;data</code></pre><br>
When using the SSH command in the previous section.<br>
<br>
Or if you are using the bash script, just do this when prompted for the users <code>HOME</code> path:<br>
<br>
<pre><code>private&#x2F;rtorrent&#x2F;data</code></pre><br>
This way when they log in they will be put into this folder. If you don&#x27;t give them a <code>--home</code> that corresponds to the configured jails they will get a blank listing and may not be able to navigate to the jail.<br>
<br>
To add more jails look at the example described next to see how we do this.<br>
<br>
<h2>Adding or editing custom jails</h2><br>
<strong>Step 1:</strong> Choose or create a folder to lock them in (their HOME location). Then use the relative path to that folder in the commands below for <code>--home</code>. This is an optional step, but if their <code>HOME</code> corresponds to the jail path you wish to set for them, they will connect directly to their <code>HOME</code> folder by default, using an FTP client, with no need to manually specify the location.<br>
<br>
<pre><code>~&#x2F;proftpd&#x2F;bin&#x2F;ftpasswd --passwd --name my_username --file ~&#x2F;proftpd&#x2F;etc&#x2F;ftpd.passwd --uid 5001 --gid 5001 --home $HOME&#x2F;path&#x2F;to&#x2F;jail --shell &#x2F;bin&#x2F;false</code></pre><br>
For example:<br>
<br>
<pre><code>~&#x2F;proftpd&#x2F;bin&#x2F;ftpasswd --passwd --name my_username --file ~&#x2F;proftpd&#x2F;etc&#x2F;ftpd.passwd --uid 5001 --gid 5001 --home $HOME&#x2F;private&#x2F;downloads&#x2F; --shell &#x2F;bin&#x2F;false</code></pre><br>
<strong>Step2:</strong> Edit your:<br>
<br>
<pre><code>~&#x2F;proftpd&#x2F;etc&#x2F;proftpd.conf</code></pre><br>
Then add the following. The example code below makes a <strong>READ ONLY</strong> jail some with SFTP specific commands.<br>
<br>
You will notice there are already three existing and configured configured jails in the <code>proftpd.conf</code><br>
<br>
<pre><code># Note: You need to specify a users root&#x2F;home directory when using ftpasswd
&lt;Directory &#x2F;media&#x2F;DiskID&#x2F;home&#x2F;my_username&#x2F;private&#x2F;downloads&#x2F;&gt;
# STAT LSTAT are specific to SFTP only. See below
&lt;Limit STAT LSTAT DIRS READ&gt;
AllowAll
&lt;&#x2F;Limit&gt;
&lt;&#x2F;Directory&gt;</code></pre><br>
<h2>Using symlinks to other directories</h2><br>
If you want to be able to follow symlinks outside the allowed directory you will need to add a new <code>&lt;Directory&gt;</code> section for each folder inside the <code>proftpd.conf</code>, for example:<br>
<br>
You have created a new folder called <code>proftpd_pub</code> in the root of your slot using this command:<br>
<br>
<pre><code>mkdir -p ~&#x2F;proftpd_pub</code></pre><br>
You have also created a special <code>completed</code> directory for your finished torrents opened in Deluge located at:<br>
<br>
<pre><code>~&#x2F;private&#x2F;deluge&#x2F;completed</code></pre><br>
You have then created this symlink to the Deluge completed directory inside the <code>proftpd_pub</code> directory like this:<br>
<br>
<pre><code>ln -s ~&#x2F;private&#x2F;deluge&#x2F;completed ~&#x2F;proftpd_pub&#x2F;downloads</code></pre><br>
To allow the jailed users access via the symlink you would need to add the following to <code>proftpd.conf</code> to give them read only access to both required directories.<br>
<br>
<pre><code>&lt;Directory &#x2F;media&#x2F;DiskID&#x2F;home&#x2F;username&#x2F;proftpd_pub&#x2F;&gt;
# STAT LSTAT are specific to SFTP only. See below
&lt;Limit STAT LSTAT DIRS READ&gt;
AllowAll
&lt;&#x2F;Limit&gt;
&lt;&#x2F;Directory&gt;
#
&lt;Directory &#x2F;media&#x2F;DiskID&#x2F;home&#x2F;username&#x2F;private&#x2F;deluge&#x2F;completed&#x2F;&gt;
# STAT LSTAT are specific to SFTP only. See below
&lt;Limit STAT LSTAT DIRS READ&gt;
AllowAll
&lt;&#x2F;Limit&gt;
&lt;&#x2F;Directory&gt;</code></pre><br>
Then you will need to restart the proftpd running process for the changes to take effect.<br>
<br>
<h2>AllowAll or AllowUser</h2><br>
The use of <code>AllowAll</code> means that any non main user has read only access to the default jails in the default <code>proftpd.conf</code>. <br>
<br>
You can remove this and use <code>AllowUser</code> insted in this way:<br>
<br>
<pre><code>&lt;Directory&#x2F;media&#x2F;DiskID&#x2F;home&#x2F;username&#x2F;private&#x2F;deluge&#x2F;completed&#x2F;&gt;
# STAT LSTAT are specific to SFTP only. See below
&lt;Limit STAT LSTAT DIRS READ&gt;
AllowUser my_username
&lt;&#x2F;Limit&gt;
&lt;&#x2F;Directory&gt;</code></pre><br>
In this example only <code>my_username</code> has read only access to this jail. You can add more than one user.<br>
<br>
This will let you fine tune access to the jails.<br>
<br>
<h2>Make a jail writeable so users can upload.</h2><br>
To make a directory writeable so users can upload to it you would add <code>WRITE</code> permissions.<br>
<br>
The default set-up is read only by default on all example jails using this set-up.<br>
<br>
<pre><code>&lt;Limit STAT LSTAT DIRS READ&gt;</code></pre><br>
To make it so we can upload we simply need to add <code>WRITE</code> to the allowed permissions for this jail. So it becomes.<br>
<br>
<pre><code>&lt;Limit WRITE STAT LSTAT DIRS READ&gt;</code></pre><br>
That is it, this jail is now writeable and users can upload. <br>
<br>
If you wanted to fine tune their upload permissions you would break down the <code>WRITE</code> permission into it component parts. What this means is that <code>WRITE</code> is a group of commands covering:<br>
<br>
<pre><code>APPE, DELE, MKD, RMD, RNTO, STOR, STOU, XMKD, XRMD </code></pre><br>
So instead of <code>WRITE</code> you could allow the specific permissions only.<br>
<br>
Please see the Useful links section at the end of the FAQ for info on directives you can use in the proftpd.conf and the use of Limits.<br>
<br>
<h2>Step 9: Let&#x27;s start ProFTPD:</h2><br>
 <strong>Important note:</strong> If you make changes to the <code>proftpd.conf</code> you will need to restart it for these changes to take effect.<br>
<br>
<strong>Before you start proftpd make sure you have configured your jails. Configuration file changes require you restart proftpd to take effect.</strong><br>
<br>
This is how you would have both sftp and ftps daemons running at the same time. You will need to run two separate instances of proftpd using different configurations using these commands.<br>
<br>
<strong>SFTP</strong><br>
<br>
<pre><code>~&#x2F;proftpd&#x2F;sbin&#x2F;proftpd -c ~&#x2F;proftpd&#x2F;etc&#x2F;sftp.conf</code></pre><br>
<strong>FTPS</strong><br>
<br>
<pre><code>~&#x2F;proftpd&#x2F;sbin&#x2F;proftpd -c ~&#x2F;proftpd&#x2F;etc&#x2F;ftps.conf</code></pre><br>
 <strong>Important note:</strong> These warning are normal and to be expected. They are not errors. Proftpd will have started and be running and you should be able to now SFTP&#x2F;FTPS to the port defined in your configuration files.<br>
<br>
<pre><code>[server ~] ~&#x2F;proftpd&#x2F;sbin&#x2F;proftpd -c ~&#x2F;proftpd&#x2F;etc&#x2F;sftp.conf
2015-00-00 00:00:00,000 server proftpd[0000] server.feralhosting.com: SocketBindTight in effect, ignoring DefaultServer
2015-00-00 00:00:00,000 server proftpd[0000] server.feralhosting.com: unable to set daemon groups: Operation not permitted</code></pre><br>
 <strong>Important note:</strong> If you see an error about truncated data on certain lines this is due to there being no new,blank line at the end of the relevant configuration files. You should not see this error with the configuration files included in this FAQ unless you have modified them and removed the blank line at the end.<br>
<br>
Do this command to see if it&#x2F;they are running:<br>
<br>
<pre><code>ps x | grep proftpd | grep -v grep</code></pre><br>
You will see one or more results like this:<br>
<br>
<pre><code>4880 ?&nbsp; &nbsp; &nbsp; &nbsp; Ss&nbsp; &nbsp;  0:00 proftpd: (accepting connections)</code></pre><br>
We are only interested in the results that end with:<br>
<br>
<pre><code>proftpd: (accepting connections)</code></pre><br>
These are the actual SFTP and&#x2F;or FTPS proftpd processes.<br>
<br>
To kill the process so you can restart them use this:<br>
<br>
<pre><code>kill -9 PID</code></pre><br>
Where PID is the number listed in the <code>ps x | grep proftpd | grep -v grep</code> command we used.<br>
<br>
or <br>
<br>
<pre><code>killall -9 proftpd -u $(whoami)</code></pre><br>
To kill all processes.<br>
<br>
<h2>Crontab automatic restart on server reboot</h2><br>
To edit your crontab you must do this command:<br>
<br>
<pre><code>crontab -e</code></pre><br>
Now all you need to do is add whichever of these lines you want to restart on a reboot:<br>
<br>
<strong>SFTP</strong><br>
<br>
<pre><code>@reboot $HOME&#x2F;proftpd&#x2F;sbin&#x2F;proftpd -c $HOME&#x2F;proftpd&#x2F;etc&#x2F;sftp.conf</code></pre><br>
<strong>FTPS</strong><br>
<br>
<pre><code>@reboot $HOME&#x2F;proftpd&#x2F;sbin&#x2F;proftpd -c $HOME&#x2F;proftpd&#x2F;etc&#x2F;ftps.conf</code></pre><br>
Then press and hold <code>CTRL</code> then press <code>X</code> to save then press <code>Y</code> say yes and exit<br>
<br>
<h2>Step 10: Connecting to my Server</h2><br>
The process is the same as normal:<br>
<br>
<a href="https://www.feralhosting.com/faq/view?question=187">FTP and SFTP basics - Filezilla</a><br>
<br>
<strong>Host</strong>: server.feralhosting.com<br>
<strong>username</strong>: whatever user names you created<br>
<strong>port</strong>: as you defined in the config files.<br>
<br>
<h2>SFTP Public&#x2F;Private Keys (optional)</h2><br>
The <code>sftp.conf</code> has been configured to accept and use key pairs. There is just one things you must do to make it work. You must convert it to <code>SSH RFC 4716</code> format. This is not as complicated as it sounds.<br>
<br>
The <code>sftp.conf</code> has been configured to look for keyfiles already, all you need to do is:<br>
<br>
<strong>1:</strong> Convert the public key to&nbsp; <code>SSH RFC 4716</code> format.<br>
<strong>2:</strong> Upload it to the <code>~&#x2F;proftpd&#x2F;etc&#x2F;sftp&#x2F;authorized_keys&#x2F;</code> directory<br>
<strong>3:</strong> Rename the file to match the username of the user it is authenticating. You can have multiple copies of the same files with different usernames.<br>
<br>
It will then work the next time a user tries to authenticate using a matching private key-file.<br>
<br>
<strong>On Windows:</strong><br>
<br>
If you are on Windows and you used <a href="https://www.feralhosting.com/faq/view?question=13">Puttygen</a> to create your keyfile, just open it up in Puttygen and save the public key as shown, with no file extension.<br>
<br>
<img src="https://raw.github.com/feralhosting/feralfilehosting/master/Feral%20Wiki/Software/proftpd%20-%20Installing%20an%20FTP%20daemon%20for%20extra%20accounts/puttygen1.png"><br>
<img src="https://raw.github.com/feralhosting/feralfilehosting/master/Feral%20Wiki/Software/proftpd%20-%20Installing%20an%20FTP%20daemon%20for%20extra%20accounts/puttygen2.png"><br>
<br>
Now upload this file <strong>named after your proftpd user</strong> to:<br>
<br>
<pre><code>~&#x2F;proftpd&#x2F;etc&#x2F;sftp&#x2F;authorized_keys&#x2F;</code></pre><br>
So for example, if the uploaded filed is named after <code>my_username</code> it will look like this after you have uploaded it.<br>
<br>
<pre><code>~&#x2F;proftpd&#x2F;etc&#x2F;sftp&#x2F;authorized_keys&#x2F;my_username</code></pre><br>
Then use this command in SSH to give all files in that directory 600 permissions.<br>
<br>
<pre><code>find ~&#x2F;proftpd&#x2F;etc&#x2F;sftp&#x2F;authorized_keys&#x2F; -type f -exec chmod 600 {} \;</code></pre><br>
<strong>On Unix:</strong><br>
<br>
<pre><code>ssh-keygen -e -f &#x2F;path&#x2F;to&#x2F;file</code></pre><br>
You might have to have only 1 key in a file for this command to work. See the note below.<br>
<br>
For example:<br>
<br>
<pre><code>ssh-keygen -e -f ~&#x2F;.ssh&#x2F;authorized_keys</code></pre><br>
 <strong>Important note:</strong> I could only make it return the first line of multiple lines in this command. If anyone has a solutions to this please update the FAQ. I suggest using the command directly on the OpenSSH format private or public keys.<br>
<br>
<h2>Quick FAQs:</h2><br>
Q: I get permission errors uploading files or trying to change permissions.<br>
<br>
A: This is disabled in the proftp.conf in the SITE CHMOD section. Use the <code>AllowUser Username</code> directive inside the Limit tags to overcome this on a per user basis<br>
<br>
Q: &quot;Failed to retrieve directory listing&quot; and &quot;Failed to parse returned path.&quot;<br>
<br>
A: Your users Jail home must correspond to an existing Jail path. IF you have made changes to any of the conf files don&#x27;t forget to restart proftpd as shown in Step 9<br>
<br>
<h2>Useful Links:</h2><br>
<a href="http://www.proftpd.org/docs/faq/linked/faq.html">Proftpd FAQ</a><br>
<br>
<a href="http://www.proftpd.org/docs/howto/index.html">Howto index</a><br>
<br>
<a href="http://www.proftpd.org/docs/howto/Limit.html">Limits</a><br>
<a href="http://www.proftpd.org/docs/howto/Controls.html">Controls via mod_ctrls</a><br>
<a href="http://www.proftpd.org/docs/directives/linked/configuration.html">directives list</a><br>
<br>
<a href="http://www.proftpd.org/docs/modules/index.html">Core Modules</a><br>
<a href="http://www.proftpd.org/docs/contrib/index.html">Contrib Modules</a><br>
<br>
<a href="http://www.proftpd.org/docs/example-conf.html">example-conf</a><br>
<br>
