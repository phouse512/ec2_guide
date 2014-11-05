ec2_guide
=========

RedHat Install (RHEL) 

Local Machine Instructions:
--------------------------
To install postgres:

```sudo yum install postgresql postgresql-server postgresql-devel postgresql-contrib postgresql-docs```

Next you need to initialize the database by running the following:

```sudo service postgresql initdb```

Now you need to allow for users to connect from any ip address, and make them use md5 authentication for passwords. Use vim (install it if not yet existing) to edit the pg_hba.conf file as shown:

```sudo vim /var/lib/pgsql9/data/pg_hba.conf```

In there, add 0.0.0.0/0 to allow all ip's to use it, and change auth_type from peer/ident to md5.

Next, edit another config file to allow all ip's to access (it defaults to only localhost):

```sudo vim /var/lib/pgsql9/data/postgresql.conf```

You should see #listen_address = "localhost" or something of the like. Change this to remove the # and change localhost to *, to allow all connections.

Stop and start your postgres service. 

```sudo service postgresql stop```

```sudo service postgresql start```

Now run the following two commands to get into your postgres service with the user postgres. We will create your own custom user and database inside.

```sudo su - postgres```

```psql -U postgres```

By now you're inside the console of the postgres service, hoooray!! We'll create a super user that owns a database to demonstrate your ability to log in.  In the psql console, run these two commands (insert your own username/pw):

```CREATE USER username SUPERUSER;```

```ALTER USER username with password 'password';```

Your user is set up! Now create a db that you can log into with this user:

```CREATE DATABASE db_name WITH OWNER username;```

You can now quit out of the console and test locally your setup of the database and user.  This isn't a Postgres tutorial, so if you need help with that, let me know (or stack overflow it!). Run the following command:

```psql -U username -d db_name```

It'll prompt you for your password and if you successfully log in, you'll see the psql console as you did before, but now you're already connected to the database.  If you see something along the lines of 'peer authentication failed', you probably didn't save your config settings from above correctly, or you didn't restart the postgres service.  Try again!

You're all set! If you want to connect from another machine, you need to add a Custom TCP rule on the EC2 server page on port 5432 (the default) to the IP's that you want, but that'll be for another time. Email me at philiphouse2015 at northwestern.edu if you have questions!
