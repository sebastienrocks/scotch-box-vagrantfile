# scotch-box-vagrantfile
A custom VagrantFile for scotch-box. Allows multiple domains, automatic database creation, automatic database import.

## Usage

### Adding a new website

Simply add a new domain name you would like to use on line 14 in the DOMAINS var. Put names under quotes, seperated with a space.

### Create a database

On line 15, do the same as adding a new website in the DBS var. This will be your database name. If you wish to not have a database for your domain, add a empty string ( "" ).

### Importing a database

If you have a sql file you would like to import for your project, rename it so it has the same name as your database, and put it in the /dbs/ folder. Once the provision is done it will be automatically moved to /dbs/imported.

## Running

Once you have added all your websites, run vagrant provision.

You will now have one folder per domain name added, add website files to the public folder generated.
