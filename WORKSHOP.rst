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

    - **Create a Router**
        Panels: Project -> Network -> Routers

        1. *Create Router*
        2. Type in the name of the router
        3. Select the external network from the dropdown list.
        4. Hit *Create Router*

    - **Attach Private Network's Interface to the Router**
        Panels: Project -> Network -> Routers -> (Click the name of the router we just created) -> Interfaces Tab

        1. *Add Interface*
        2. Select the subnet we just created
        3. Leave the rest to the default values
        4. Hit *Submit*

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
           Name -> <name of the flavor>,
           VCPUs -> 1,
           RAM -> 512 MB,
           Root Disk -> 1 GB,
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

Assign Floating IP and SSH
__________________________

In this step we are gonna assign the already allocated Floating IP to the Instance. As a subsequent step we are
going SSH to that Instance from host.

    - *Assign Floating IP to Instance*
       Panels: Project -> Compute -> Instances

       1. In the same line that we have the name of the instance we hit the arrow to make the dropdown menu to appear.
       2. We hit *Associate Floating IP*
       3. At the appeared Form we hit the *IP address* field and we choose the available floating IP.
       4. Hit *Associate*

    - *SSH to the Instance*
       After we associated the floating IP we are going back to the host VM and we are gonna try SSH to the Openstack Instance.

       .. code-block:: console

        ssh cirros@<Floating IP>

        pass cubswin:)

    Of course there is a more convenient way to access Instance's console. We can use the VNC console from the Horizon Dashboard.
    To do so we click on the Instance name and after that we click on the Console Tab.

Deploy an Instance Through HEAT Component
-----------------------------------------
In this step we are going to deploy an Instance by loading a HEAT template and creating a Stack. The outcome of this
procedure will be a fully functional instance. All the necessary resources which are needed for the deployment of that
instance will not be assigned to it by hand as we did in the previous steps but will be assigned automatically by HEAT component.

    - *Create the Stack*
       Panels: Project -> Orchestration -> Stacks
       1. *Launch Stack*
       2. Template Source -> Direct Input
       3. Copy and Paste the content of the example_heat.yml file
       4. Fill al the required fields
       5. Hit *Launch*

    When the Stack creation is complete  go to the Project -> Compute -> Instances panel and you will notice that there is
    a new instance with the name ``server0`` which has been created by Heat component.









