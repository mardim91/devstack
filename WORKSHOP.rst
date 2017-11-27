Openstack Tutorial
==================


Logging into Horizon
--------------------

   - **Open browser and type url**
        http://<management_IP>/dashboard

   - **Credentials**
        user: admin
        pass: devstack

Creating a Project(Tenant) and a User in Horizon
---------------------------------------
    - **Create Project**
        Panels: Identity->Projects

        1. *Create Project*
        2. Fill the necessary fields
        3. *Create Project*

    - **Create User**
        Panels: Identity -> Users

        1. *Create User*
        2. Give a name, a password, assign the user to the project you just created, give *member* role and hit *Create User*
        3. Sign out and log-in with the new user's credentials.

Booting an Instance
-------------------
First of all would be better to connect as admin user again so we can have increased flexibility. So sign-out again and log-in as admin/devstack.

To deploy an instance from Horizon the following steps will be followed:

    - **Create a Private Network**
        Panels: Project -> Network -> Networks

        1. *Create Network*
        2. At the Form in the *Network* tab just fill the name of the network you want to create and
           leave the rest to the default values. In the *Subnet* tab fill the name of the subnet and
           the Network Address in a CIDR format(11.11.11.0/24), leave the rest to the default values.
        3. Hit *Create*

    - **Allocate a Floating IP**
        Panels: Project -> Network -> Floating IPs

        1. *Allocate IP to Project*
        2. Leave default values
        3. Hit *Allocate IP*

    - **Define Security Group**
        Panels: Project -> Network -> Security Groups

        Devstack has already created for us a default Security Group. We are gonna manage the rules to
        that Security Group so we can have SSH access to the instance which we are going.

        1. *Manage Rules*
        2. *Add Rule*
        3. Select SSH at the *Rule* field.
        4. Leave the rest to the default values.
        5. Hit *Add*

    - **Load an Image to Glance service**
        Panels: Project  -> Compute -> Images

        Devstack has already created a Glance Image for us which we are gonna use to deploy our instance.
        We consider the following steps a reference in case we want to create a custom Glance Image.

        1. *Create Image*
        2. Give a name and also hit the Browse button to select the base image file which Glance is going
           to use to create a valid image for the instances.
        3. The user should select the format which his image is going to have. For a QEMU/KVM hypervisor we
           are going to select the QCOW2 format.
        4. As you can see there are several options which we can use to make our image even more customized.
           For now we are going to use the default values.
        4. Hit *Create Image*

    - **Create Flavor**
        Panels: Admin -> Compute -> Flavors

        We need increased permissions to create a flavor in Openstack which means that we are able to create
        a flavor only from the Admin dashboard in horizon.

        1. *Create Flavor*
        2. Flavor Tab:
           Name -> <name of the flavor>
           VCPUs -> 1
           RAM -> 512 MB
           Root Disk -> 1 GB
           Leave the rest to the default values
        3. Flavor Access Tab: Leave the default values(We are going to give access to all the projects to
           this flavor)
        4. Hit *Create Flavor*

    - **Boot an Instance**
        Panels: Project -> Compute -> Instances

        Now that we have all the required components in place we will deploy an instance.

        1. *Launch Instance*
        2. Details Tab:
           Name -> <instance name>
        3. Source Tab: Select the Glance Image.
        4. Flavor Tab: Select the flavor we just created in the previous section.
        5. Networks Tab: Select the network we created in the *Create a Private Network* section.
        6. Leave the rest to the default values.
        7. Hit *Launch Instance*








