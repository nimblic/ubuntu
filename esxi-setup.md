# Setting up ESXi

- Add license
-- Open Windows vSphere Client
-- Type IP address, username: root, pwd: nimblic (or as set during ESXi installation)
-- Click connect
-- Accept self-signed SSL certificate

- Add Datastore (HDD)
-- In left side menu, click ESXi host IP address
-- In right window, click Configuration tab
-- Within right window, another left-ish menu appears. Within top hardware section, click Storage
-- Banner appears asking to add datastore.
-- Follow prompts:
--- Storage Type: Disk/LUN. Click next.
--- Select Disk: Click on HDD where you want to create Datastore. Click next.
--- File System Version: Select VMFS-5. Click next.
--- 

- Upload Image to Datastore
-- In left side menu, click ESXi host IP address
-- In right menu, click Configuration tab
-- Within right window, another left-ish menu appears. Within top hardware section, click Storage
-- Within right window, the datastore you created earlier appears.
-- Right click on the datastore, click Browser Datastore
-- Window pops up. Within toolbar, click Upload files to this datastore. Click Upload Folder.
-- Find folder to upload, then click ok.
-- NOTE: If using VMware Fusion to run Windows VM, use the sharing settings to share between mac and windows
--- VMware Fusion menu > Virtual Machine > Sharing > Sharing Settings
--- Mirror all the folders (Desktop, Movies etc.)
--- If you want to mount (make available in VM) a external USB drive, click the + and select the folder / drive to mount.

- Create a Backup Template of your Uploaded Image
-- After image upload has completed, create a new folder called Templates-do-not-mount
-- Copy the uploaded folder with the image into the Templates-do-not-mount (this is so when you make changes to the existing VM, you can always just copy the template and start another VM)

- Create a VM from an Image
-- In left side menu, click ESXi host IP address
-- In right menu, click Configuration tab
-- Within right window, another left-ish menu appears. Within top hardware section, click Storage
-- Within right window, the datastore you created earlier appears.
-- Right click on the datastore, click Browser Datastore
-- Window pops up.
-- Click the folder with the image you want to create the VM from.
-- Find the VMX file. Right click on it and click Add to Inventory
-- Follow the prompts (you can pretty much only choose the VM name)
-- You should see the VM appear in the left side menu under the ESXi host IP address.

- Add Networking
-- In left side menu, click ESXi host IP address
-- In right window, click Configuration tab
-- Within right window, another left-sih menu appears. Within top hardware section, click Networking
-- Within right window, vSphere Standard switch appears - vSwitch0
-- By default, there is a Management network, which is for managing the ESXi host. There is also a VM Network created by default
-- Lets create a medtasker-host network by clicking Properties.
-- A window appears. In the left pane, it shows the vSwitch, the Managemnet Network and the default VM Network
-- In the window, click Add.
-- Follow the prompts:
--- Connection Type: select Virtual Machine. Click next.
--- Connection Settings: choose network name - ie. medtasker-host network. VLAN ID not required. Click next.
--- Click finish.

- Add Network Adapter to VM
-- In left side menu, click name of VM
-- In right window, the Getting Started tab is automatically opened.
-- Click Edit Virtual Machine Settings

- Starting the VM
-- In the left side menu, right click the name of the VM
-- Hover over Power, then click Power On

- Creating a Snapshot of the VM
-- Lets create a snapshot after starting the VM and after it has loaded to the login screen
-- In the left side menu, right clik the name of the VM
-- Hover over Snapshot, then click Take Snapshot
-- Provide a name for the snapshot - ie. Inital load to login screen
-- You will see in the Recent Tasks pane at the bottom of the vSphere Client that a task 'Create virtual machine snapshot appears'. You can watch the progress of the snapshot here. Do not enter data into the console until this has completed successfully.

- Ensure VMware tools installed
-- Required so network adapters work (essentially linux doesn't have drivers for virtual network adapters, VMware tools installs this)
-- In the left side menu, click the name of the VM
-- In the right window, click the Summary tab
-- Within the General group of properties, ensure VMware Tools is installed and Running.
-- NOTE: if you created a SNapshot of the VM, vSphere client will automatically turn on VMware Tools.
-- If VMware Tools is not running, but is installed, click the Console tab
-- Login using username / pwd (ie. nimblic/nimblic)
-- If using packer created image (which automatically installs VMware Tools as part of vmware.sh script), run `/usr/bin/vmware-toolbox-cmd`


