---
title: "Creating an OMERO server"
teaching: 25
exercises: 0
objectives:
- "Experience the installation of an OMERO server via Ansible"
keypoints:
- "OME publish example OMERO server playbooks"
- "Vagrantfile defines your VM, including NAT port mappings"
- "We can ask Vagrant about ports and ssh-keys at the CLI"
---
OME have published (and maintain) a set of example playbooks which use the same roles OME use in production which will build an OMERO server.

Here we're going to execute one of those examples - the "public user" example.

> ## Don't worry!
>
> Most of the following commands include some BASH shell expansions to simplify connecting to a Vagrant VM from different machines. It's really not as complicated as this makes it look.
>
{: .callout}

1 Acquire a copy of the examples (you may have done this already):

~~~
$ git clone https://github.com/ome/ansible-examples-omero.git 
# or
$ git clone git@github.com:ome/ansible-examples-omero.git
~~~
{: .bash} 

Change directory to the 'public-user' example in the clone of ansible-examples-omero
~~~
$ cd ansible-examples-omero/public-user
~~~
{: .bash} 

Install the prerequisite roles, defined in requirements.yml
~~~
$ ansible-galaxy install -r ../requirements.yml -p roles
~~~
{: .bash}

Add some port forwarding to the Vagrantfile to allow us to connect to OMERO afterwards - and also remove the "provision" step - since we're learning here how to do this ourselves.
~~~
$ rm Vagrantfile && wget https://gist.github.com/kennethgillen/648105ba0f78440ca41e45963c471744/raw/Vagrantfile
~~~
{: .bash} 

> ## Want to manually edit the Vagrantfile instead?
>
>  We want to end up with the following:
>  ...
>  ~~~
>  config.vm.provider "virtualbox" do |vb|
>  config.vm.network "forwarded_port", guest: 80, host: 8080
>    auto_correct: true
>  config.vm.network "forwarded_port", guest: 443, host: 8443
>    auto_correct: true
>  config.vm.network "forwarded_port", guest: 4063, host: 4063
>    auto_correct: true
>  config.vm.network "forwarded_port", guest: 4064, host: 4064
>    auto_correct: true
>    vb.customize....
>  ~~~
>  {: .code}
{: .solution}

Tell Vagrant to kick off the creation of our VM (Takes 30s)
~~~
$ vagrant up
~~~
{: .bash} 


Create an inventory file for the vagrant-driven local VM:
~~~
$ echo "localhost ansible_port=$(vagrant ssh-config | grep Port | awk '{print $2}') ansible_user=vagrant " > inventory-file
~~~
{: .bash}

It should look similar to:
~~~
$ cat inventory-file
~~~
{: .bash}
~~~
localhost ansible_port=2222 ansible_user=vagrant
~~~
{: .output}


> ## Asking Vagrant how to connect to the virtual machine
> 
> Vagrant takes care of running >1 machine at once by 
> assigning the VMs different port numbers for SSH for example.
> We can ask vagrant the SSH port of the current machine, which we
> need to give to the ansible-playbook command, via the host file.
> See [docs.ansible.com](http://docs.ansible.com/ansible/latest/intro_inventory.html) 
> for more. These shell snippets acheive the function of asking vagrant for these details.
>
<br/>
>What port is the current Vagrant VM using for SSH?
>~~~
>$ echo $(vagrant ssh-config | grep Port | awk '{print $2}')
>~~~
>{: .bash} 
>
>
<br/>
>Where is the SSH key used to connect to the current Vagrant VM's `vagrant` user?
>~~~
>$ echo $(vagrant ssh-config | grep IdentityFile | awk '{print $2}')
>~~~
>{: .bash} 
{: .solution}

Verify Ansible can connect to the VM - the setup module again.
~~~
ansible -i inventory-file -m setup localhost --private-key  $(vagrant ssh-config | grep IdentityFile | awk '{print $2}') 
~~~
{: .bash}
Type `yes` when presented with the below: 
~~~
The authenticity of host '[localhost]:2222 ([127.0.0.1]:2222)' can't be established.
RSA key fingerprint is 88:f6:8a:85:09:e1:32:33:2f:47:b3:70:b8:ae:77:b7.
Are you sure you want to continue connecting (yes/no)?
~~~
{: .output}
Type yes...
~~~
localhost | SUCCESS => {
    "ansible_facts": {
        "ansible_all_ipv4_addresses": [
            "10.0.2.15"
        ],
... 
~~~
{: .output}

>## Seeing ssh errors?
>
>If you've an `~/.ssh/known_hosts` entry for `localhost` then this new 
>VM is likely to conflict. You will have to delete the existing entry 
>with the corresponding line number in the output from Ansible.
>This can be done manually, or with the following snippets:
>~~~
># Looking in the output for known_hosts:line-number
>$ echo $(ansible -i inventory-file -m setup localhost --private-key  $(vagrant ssh-config | grep IdentityFile | awk '{print $2}')  | grep -oP '(?<=hosts:)\d+')
># Use sed to delete that line. Replace "LINENUMBER" with the line number from the output above.
>$ sed -i.bak -e "LINENUMBERd" ~/.ssh/known_hosts
>~~~
>{: .bash} 
> Now re-try the above Ansible connect attempt. 
{: .solution}

Putting it all together - running the 'public user' example playbook. Estimated 15m runtime.
~~~
$ ansible-playbook -i inventory-file --private-key  $(vagrant ssh-config | grep IdentityFile | awk '{print $2}') --become  playbook.yml
~~~
{: .bash}

#### 15m wait...

Now we should be able to connect to our local Vagrant-driven VM.

Open up a web browser, and connect to [http://localhost:8080](http://localhost:8080)

If this port was already taken when we issued `vagrant up`, [Vagrant should have 
auto-corrected and picked another port](https://www.vagrantup.com/docs/networking/forwarded_ports.html#auto_correct). We can ask for the current port mappings as follows:

~~~
$ vagrant ports
~~~
{: .bash}
~~~
The forwarded ports for the machine are listed below. Please note that
these values may differ from values configured in the Vagrantfile if the
provider supports automatic port collision detection and resolution.

    22 (guest) => 2222 (host)
  4063 (guest) => 4063 (host)
  4064 (guest) => 4064 (host)
    80 (guest) => 8080 (host)
   443 (guest) => 8443 (host)
~~~
{: .output}

{% include links.md %}
