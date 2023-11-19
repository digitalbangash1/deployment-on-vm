# Deploying an app over Azure VM through Caprover

If you have a domain name, make sure that your domain name points to your server.  In your host name you can write ***.** (star and period) which means that you could be connecting different name to your domain name e.g www.frontend.digitalbangash.info or www.backend.digitalbangash.info.


## SSH to your Azure VM
We will jump through all the settings for nginx and Certbot as we will be using Caprover which automatically does it for us.
If you have nginx running on VM surver. Stop it by typing

    sudo systemctl stop nignx


## Login to Docker

Install docker on your virtual machine and follow the guide for installation on [Docker Installation.](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04)
If everything is correctly installed, run the command `docker run hello-world` to check if everything is working properly. Restart the VM through your azure portal in case it needs restart.


## Azure portal
Through azure portal open for the following ports : `80,443,3000,996,7946,4789,2377` or through your VM by typing in powershell/command prompt

    ufw allow 80,443,3000,996,7946,4789,2377/tcp; ufw allow 7946,4789,2377/udp;
When all the ports are allowed inbound, you need to get the caprover on your VM by running the command:

    docker run -p 80:80 -p 443:443 -p 3000:3000 -v /var/run/docker.sock:/var/run/docker.sock -v /captain:/captain caprover/caprover


## Setting the Caprover on localmachine

Open a powershell/commandpromt and enter the following commands:

    npm install -g caprover 
    caprover serversetup
   The second command is to setup the caprover server via our localmachine. You might encounter the error "*You cannot run this script on the current system. For more information about running scripts*" by running the second command. 
To run scripts that are not digitally signed in PowerShell, you can temporarily change the execution policy. Here's how you can do it:
1.  **Open PowerShell as Administrator**: Right-click on the PowerShell icon and choose "Run as administrator" to open an elevated PowerShell session with administrative privileges.
    
2.  **Check the Current Execution Policy**: To check the current execution policy, run the following command:
    
    powershellCopy code
    
    `Get-ExecutionPolicy` 
    
    It will likely return something like "Restricted."
    
3.  **Set the Execution Policy to Allow Running Unsigned Scripts**: You can set the execution policy to allow running unsigned scripts using the following command:
        
    `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Unrestricted` 
    
    This command temporarily sets the execution policy to allow running unsigned scripts for the current user.
    
4.  **Run Your Script**: Now, you should be able to run your script without encountering the "not digitally signed" error. For example:
        
    `caprover serversetup` 
    
5.  **After Running the Script**: Once you've finished running your script, consider setting the execution policy back to a more restrictive setting for security. You can do this by running:
    
    powershellCopy code
    
    `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned` 
    
    The "RemoteSigned" policy allows running locally created and signed scripts but requires remote scripts to be digitally signed.
    

Remember that changing the execution policy should be done with caution and only for scripts that you trust. It's important to restore the more restrictive execution policy after running your script to maintain security.

## Next Step
If your caproversetup on localmachine is done correctly, you would be able to see a link such as :

    https://captain.yourDNS.com

