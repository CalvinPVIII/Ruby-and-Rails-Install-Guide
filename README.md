# _Ruby/Rails Install Guide for Windows 10_


## Things to know

Setting up ruby and rails on windows can be a real pain. Unfortunately I have had to endure this error riddled process twice. I think I have a relatively straightforward and mostly harmless way to install ruby and rails on a windows machine.

Some of these steps listed may be completely unnecessary, but this is the process I went through and both my laptop and pc work using Ruby, Rails, and PSQL.

Throughout this process I was getting tons of DPKG errors, after many hours of trying to fix them I just ignored them and everything ended up working anyway. Whenever you run into errors, try and see if you can narrow them down, but ultimately if you can't fully get rid of them, try ignoring them.

If you are still having any problems feel free to reach out to me and I can do my best to help.


## Installing Linux (WSL)
Installing Windows Subsystem for Linux makes this process infinitely easier, as without it I have no idea how any of this would work.
## Steps
* _Run Windows Powershell as admin_
* _Run the following command:_
<pre><code>Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
</code></pre>
* _Open up the Microsoft Store_
* _Search for and install Ubuntu_
* _Launch Ubuntu and follow the setup instructions_
* _When the setup is done, close the Ubuntu terminal_

You may need to restart your computer

Now that Ubuntu is installed, you will pretty much always want to type "bash" when you start your terminal. This will place you inside the Ubuntu terminal, and will be where you install and use basically everything else.

## Installing Ruby (RVM)
RVM is a handy tool that allows you to install and jump between Ruby versions quickly, at least on paper. Sometimes it likes to hang on the install, but ultimately it works well.
## Steps
* _Install GPG keys using the following command in the terminal_
<pre><code>gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
</code></pre>
_If that doesn't work, try running 'gpg2' instead of 'gpg'_
* _Run the following command to install RVM:_
<pre><code>\curl -sSL https://get.rvm.io | bash -s stable
</code></pre>

RVM should now be installed. To install a version of Ruby, type the following command, replacing the x's for whatever version you want.  <pre><code>RVM install 2.x.x
</code></pre>


During this process I received a ton of DPKG errors, but only on my laptop and not my pc. I spent many hours trying to figure how to fix them, but ultimately I ignored them and everything still seemed to work.


## Installing PSQL
Install PSQL regularly through the windows installer [here](https://www.postgresql.org/download/windows/)

## Installing Rails
Hopefully installing Ruby wasn't too much of  hassle, because this is the real 'fun' part.
## Steps
* _Run these following commands_
<pre><code>curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get update
sudo apt-get install yarn
sudo apt-get install -y nodejs
gem install rails 5.2.4
sudo apt-get install postgresql-client libpq5 libpq-dev
</code></pre>
You probably will have some errors somewhere in there. The best thing I found to do was to just run a 'sudo apt-get update' and try again, and that seemed to fix most problems I came across. The setup is not done, as yarn needs to be fully installed/updated and psql needs to be configured.

## Updating/Installing Yarn
## Steps
* _Run these following commands_
<pre><code>curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update
sudo apt-get install yarn
yarn install --check-files
</code></pre>

## Configuring PSQL
Psql is kind of annoying on linux, but once properly configured it's okay. You most likely will get some different errors then the ones I got, but the main problems that I faced were wrong port number, not being able to input password, no user, and no database. This should hopefully cover those issues.

## Steps
* _Run these following commands_
<pre><code>curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
sudo apt install postgresql postgresql-contrib
</code></pre>
* _Check to see if psql is fully installed by typing 'psql --version'_
* _Run the following command_
<pre><code>nano ~/.bashrc</code></pre>
* _Scroll all the way to the bottom of the file using the arrow keys and enter the following line_
<pre><code>export PGHOST=localhost
</code></pre>
* _When it's added, press CTRL + x to exit, then enter to save_
* _Double check it actually saved by going back into nano and scrolling to the bottom of the page. If it's still there, exit nano._

That should cover the port issues. For password issues, go to where you installed PSQL on windows and find the file 'pg_hba.conf'. It should be in the data folder of the install.

Where ever it says md5, change that to trust.

Now to make sure you have proper admin rights, go back into your terminal.
* _Run these following commands_
<pre><code>psql -h localhost -U postgres
CREATE USER (username)
CREATE DATEBASE (username)
ALTER USER (username) WITH SUPERUSER;
</code></pre>
The (username) should be whatever you inputted when initially installing the WSL ubuntu terminal.

And with all that, you should be good to go. If you type psql in console it should connect you to the database with your username, and rails should be able to create and migrate the database using psql.
## Switching Ruby Version
To switch your Ruby version run the following commands
<pre><code>rvm install (ruby version)
rvm use (ruby version)
</code></pre>

## Switching Rails Version
To switch your Rails version, run the following commans
<pre><code>gem uninstall rails -a
gem uninstall railties -a
gem install rails -v (rails version)
</code></pre>


## Closing

Hopefully this helps everybody out as this was a very tedious process and I'm almost certain that you will get errors that I didn't. But in the end this is what I did to get ruby, rails, and psql working on two different machines running Windows 10. If you run into any problems I have no problem helping out.

