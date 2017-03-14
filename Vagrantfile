# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

    config.vm.box = "scotch/box"
    config.vm.network "private_network", ip: "192.168.33.10"
    config.vm.hostname = "scotchbox"
    config.vm.synced_folder ".", "/var/www", :nfs => { :mount_options => ["dmode=777","fmode=666"] }

    config.vm.provision "shell", inline: <<-SHELL

        ## EDIT HERE ##
        DOMAINS=("portfolio.local") ## Add domains of various projects
        DBS=("") ## Add database names of various projects, leave empty if domain does not need one
        ## STOP EDITING ##

        YELLOW='\033[1;33m'
        CYAN='\033[1;36m'
        NC='\033[0m'

        for ((i=0; i < ${#DOMAINS[@]}; i++)); do

            DOMAIN=${DOMAINS[$i]}
            DB=${DBS[$i]}
            DBFILE="/var/www/dbs/$DB.sql"
            IMPORTED="/var/www/dbs/imported/"

            echo "Creating directory for $DOMAIN"
            mkdir -p /var/www/$DOMAIN/public

            echo "Creating vhost config for $DOMAIN"
            sudo cp /etc/apache2/sites-available/scotchbox.local.conf /etc/apache2/sites-available/$DOMAIN.conf

            echo "Updating vhost config for $DOMAIN"
            sudo sed -i s,scotchbox.local,$DOMAIN,g /etc/apache2/sites-available/$DOMAIN.conf
            sudo sed -i s,/var/www/public,/var/www/$DOMAIN/public,g /etc/apache2/sites-available/$DOMAIN.conf

            if [ -n "$DB" ];
            then
                echo "${CYAN}Creating database if it doesn't exist.${NC}"
                sudo mysql -u root -proot -e "CREATE DATABASE IF NOT EXISTS $DB"

                echo "${CYAN}Checking if a database needs to be imported...${NC}"
                if [ -f "$DBFILE" ];
                then
                    printf "${CYAN}File $DB.sql Exists. Importing${NC}"
                    sudo mysql -u root -proot $DB < $DBFILE
                    mv $DBFILE $IMPORTED
                else
                    printf "${YELLOW}No database to import for $DB.${NC}"
                fi

            else
                echo "No database created for $DOMAIN"
            fi

            echo "Enabling $DOMAIN."
            sudo a2ensite $DOMAIN.conf
        done

        sudo service apache2 restart

    SHELL

end
