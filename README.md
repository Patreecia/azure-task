# Create Virtual Network and Subnet

    Navigate to the Azure portal and select "Create a resource".
    Search for "Virtual Network" and click "Create".
    Basics tab:
        Subscription: Select your subscription.
        Resource Group: Select an existing resource group or create a new one.
        Name: Enter a name for the virtual network.
        Region: Select the region.
    IP Addresses tab:
        IPv4 Address space: Set the address space, e.g., 10.0.0.0/16.
        Subnet: Add a subnet, e.g., subnet1 with 10.0.1.0/24.
    Review and create the virtual network.

# Create Network Security Group

    Navigate to the Azure portal and select "Create a resource".
    Search for "Network Security Group" and click "Create".
    Basics tab:
        Subscription: Select your subscription.
        Resource Group: Select the same resource group as your virtual network.
        Name: Enter a name for the NSG.
        Region: Select the region.
    Inbound Security Rules tab:
        Add a rule to allow HTTP traffic:
            Source: Any
            Source port ranges: *
            Destination: Any
            Destination port ranges: 80
            Protocol: TCP
            Action: Allow
            Priority: 100
            Name: Allow-HTTP
    Review and create the NSG.
# Create the Virtual Machine
    Navigate to the Azure portal and select "Create a resource".
    Search for "Virtual Machine" and click "Create".
    Basics tab:
        Subscription: Select your subscription.
        Resource Group: Select your resource group.
        Virtual Machine Name: Enter a name for your VM.
        Region: Select the same region.
        Image: Select an Ubuntu image.
        Size: Choose an appropriate size.
        Authentication Type: SSH public key.
        Username: Enter a username.
        SSH Public Key: Paste your public key.
    Disks tab: Select the OS disk type.
    Networking tab:
        Virtual network: Select the virtual network created earlier.
        Subnet: Select the subnet created earlier.
        Public IP: Create a new public IP.
        NIC Network Security Group: Select the NSG created earlier.
    Review and create the VM.

# Install Nginx and Serve HTML

    Connect to the VM via SSH using its public IP:

Update package lists and install Nginx:

sh

sudo apt update
sudo apt install nginx -y

Create a static HTML file:

Using the konsole in the local machine 

This setup ensures that your VM is accessible via its public IP address,
HTTP traffic is allowed through the NSG, and Nginx serves a static HTML file.
give me a detailed documentation 

# Verifying the Setup

    Open a web browser and navigate to the public IP of your VM (e.g., http://<Public-IP>).
    You should see the static HTML content: <h1>Hello, Azure!</h1>.

# Conclusion

You have successfully created an Azure Virtual Machine, placed it in a subnet within a virtual network,
attached a Network Security Group that allows HTTP access, and installed Nginx to serve a static HTML file. 
This setup ensures that your VM is accessible via its public IP and can serve web content and used frontdoor to front the public ip.

# Set Up Azure Front Door
Using Azure Portal

    Navigate to the Azure Portal: Azure portal.
    Create a Front Door:
        Select "Create a resource".
        Search for "Front Door and CDN profiles" and select "Create" under "Front Door and CDN profiles".
        Select "Front Door" and click "Continue".
    Basics:
        Subscription: Select your subscription.
        Resource Group: Select the same resource group used for your virtual machine or create a new one.
        Name: Enter a name for your Front Door profile.
        Tier: Select "Standard" or "Premium" based on your needs.
        Click "Next: Configuration".
    Front Door Configuration:
        Click "Add a frontend host".
        Frontend host name: Enter a unique name. This will be your Front Door URL (e.g., myfrontend.azurefd.net).
        Click "Add".
        Click "Add a backend pool".
            Backend pool name: Enter a name for your backend pool.
            Backend: Click "Add a backend".
                Backend host type: Select "Custom host".
                Backend host name: Enter the public IP address or DNS name of your virtual machine.
                Priority: Set to 1.
                Weight: Set to 50.
                HTTP Port: 80.
                HTTPS Port: 443 (optional, if you have HTTPS enabled on your VM).
                Click "Add".
            Click "Add" to save the backend pool.
        Click "Add a routing rule".
            Routing rule name: Enter a name for your routing rule.
            Accepted protocols: Select "HTTP" (and "HTTPS" if applicable).
            Frontend hosts: Select the frontend host you created earlier.
            Backend pool: Select the backend pool you created earlier.
            Click "Add".
        Click "Review + create" and then "Create".

# Verify Azure Front Door
    Navigate to the Front Door URL: Open a web browser and navigate to the frontend host URL you created (e.g., http://myfrontend.azurefd.net).
    Check Content Delivery: You should see the static HTML content served by your VM: <h1>Hello, Azure!</h1>.

# Conclusion

By following these steps, you have successfully configured Azure Front Door to front the public IP of your virtual machine. 
Azure Front Door provides a scalable, secure, and high-performance entry point for your application, 
ensuring that user traffic is efficiently distributed and managed.


