
#Steps:

1. On your local VIRL server, run the command

   `sudo salt-call -l debug state.sls virl.terraform`
   
   This will install terraform, clone the repo, create an ssh key, copy in minion keys and replace many variables in the variables.tf file.
   
2. Register with www.packet.net for an account

3. Log in to app.packet.net:
  1. Add your ssh public rsa key.  
  
     To get your public key from your local VIRL server, use the command

     `cat /home/virl/.ssh/id_rsa.pub`
     
     Paste the contents into the field on the Packet.net page.
     
  2. Create new project
  3. Create api key token

4. `cd virl_packet`

5. get your project id using the command

   `curl -H 'X-Auth-Token:<putAPIkeyhere>' https://api.packet.net/projects`

    The command will return a set of output. Look for the field starting "id": Make a note of the UUID that follows that.

6. edit `variables.tf` and alter at the value in the 'default' fields for at least the following variables
  1. packet_api_key
  2. packet_project_id
	**Do NOT alter the salt_master value**

7. Run the command 

   `terraform plan .`
   
   This will validate the terraform .tf file.
   
8. Run the command 

   `terraform apply .`     
   
   This will spin up your Remote VIRL server and install the VIRL software stack. If this runs without errors, expect it to take ~30 minutes. When it completes, the system will report the IP address of your Remote VIRL server. Login using
   
    `ssh root@<ip address>`

9. To see more information about your Remote VIRL server, run the command 

   `terraform show` 
   
   The output will provided details of your Remote VIRL server instance.


10. When logged in, to run commands such as 'nova service-list' you need to be operating as the virl user. To do this, use the command
 
    `su -l virl`

11. When you're ready to terminate your remote VIRL server instance, on your LOCAL VIRL server, issue the command 
 
    `terraform destroy .`

To start up again, repeat step 8.

[NOTE] Your uwmadmin and guest passwords are in variables.tf. If you can't remember them, this is where you can find them, or by running terraform output
 