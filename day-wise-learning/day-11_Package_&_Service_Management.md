# ***Day 11 : Package and Service Management***

## apt update (or yum update)
Refreshes the local package index with the latest changes made in the repositories.
Note: Always run this before installing new software to ensure you get the latest version.

## apt install <package_name>
Downloads and installs a specific software package and its dependencies.
Example: sudo apt install nginx (Installs the Nginx web server).

## apt remove <package_name>
Uninstalls a previously installed software package.
Example: sudo apt remove nginx

## systemctl start <service_name>
Starts a background service immediately.
Example: sudo systemctl start nginx

## systemctl status <service_name>
Displays the current state of a service, including whether it is running, stopped, or has encountered errors.
Note: Crucial for troubleshooting. Look for the "active (running)" status.

## systemctl enable <service_name>
Configures a service to start automatically whenever the system boots up.
Note: This is a key command for DevOps to ensure high availability after server restarts.

## systemctl stop <service_name>
Stops a currently running service.
Example: sudo systemctl stop nginx

## systemctl restart <service_name>
Stops and then immediately starts a service.
Note: Very useful when you have modified a configuration file and need the service to apply the new changes.
