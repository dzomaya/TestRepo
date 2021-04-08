# PAEM Mass Configuration of PADM v20 LX Devices IPv4/IPv6 Settings Workaround
**⚠️⚠️⚠️ Do NOT read this unless you have spoken to Tripp Lite Technical Support about it first⚠️⚠️⚠️**

## Background
**Affected software:** PowerAlert Element Manager (PAEM) version 1.0.0.266 and LX Devices (e.g. WEBCARDLX) running PowerAlert Device Manager (PADM) firmware version 20.0.0.2526

PAEM version 1.0.0.266 and LX Devices running firmware version 20.0.0.2526 have a known issue where IPv4 and IPv6 settings in a configuration file are pushed to target devices.
In the case of a source device (i.e. the LX device the configuration was _imported_ _from_) with a static IP address, this issue can result in multiple LX devices with the same IP address. 

_Note: if all your LX devices use DHCP and the same DNS settings, this issue has no major practical impact. The settings will simply be reconfigured._

There are two workarounds to this problem that allow users to configure LX Devices using PAEM without mass configuring IP address settings:

1. Download the configuration .json file from PAEM, edit it, and reupload the edited file
2. Use a configuration file from an LX device with PADM firmware version 15.5.x. Note: This workaround limits supported configuration features to those supported in PADM 15.5.x

The process below details workaround #1. 

The process assumes familarity with using PAEM and a text editor (e.g. Notepad or Notepad++)

## How to edit a PAEM configuration file to work around the IPv4/IPv6 settings conflict 

**Prerequisites:**
* PAEM 1.0.0.266 installed and accessible
* PADM v20.0.0.2526 LX devices in PAEM inventory

1. Login to PAEM and import a configuration file from a PADM version 20.0.0.2526 LX Device with the settings you wish to mass configure
   ![image](https://user-images.githubusercontent.com/17661803/113943967-5bd77680-97c9-11eb-8f53-8e85ea287124.png)
2. Click **Manage** → **Mass Configuration** and then click the **pencil icon** next to the configuration file created in step 1
   ![image](https://user-images.githubusercontent.com/17661803/113944482-3303b100-97ca-11eb-8601-35a6ab231447.png)
3. In the **Device Configuration** pop-up, scroll down and click the **Export** button in the lower-left corner
    ![image](https://user-images.githubusercontent.com/17661803/113944662-8675ff00-97ca-11eb-8b99-45346439baad.png)
4. Download and save the .json file with a unique name
   ![image](https://user-images.githubusercontent.com/17661803/113944852-d2c13f00-97ca-11eb-89d6-8bad23b776b1.png)
5. Open the .json file in a text editor. _Example: Using Notepad, click File → Open, then change the file type to "All files (*.*)", navigate to the .json from step 4, and click Open._
6. Find the **network_config/ipv4/eth0** sections of the configuration file and delete these lines 
    ```json        
      "network_config/ipv4/eth0" : {
        "data" : [ {
          "attributes" : {
            "enabled" : true,
            "ip_method" : "IPV4METHOD_STATIC",
            "ip_address" : "10.22.0.13",
            "netmask" : "255.224.0.0",
            "default_gateway" : "10.0.0.1"
          }
        } ]
      },
      "network_config/ipv6/eth0" : {
        "data" : [ {
          "attributes" : {
            "enabled" : false,
            "ip_method" : "IPV6METHOD_DHCP",
            "default_gateway" : ""
          }
        } ]
      },
    ```
    Here is a before and after example of what the section of the configuration file should look like:
    ![image](https://user-images.githubusercontent.com/17661803/113946202-71e73600-97cd-11eb-8fbb-0c05eef8c3e1.png)
    **Note: If you do NOT want to mass configure DNS settings, you should remove the "network_config/dns" section as well.**
    
7. Save the configuration file with a new name.
8. In PAEM, from the  **Manage** → **Mass Configuration** section (close the Device Configuration pop-up if needed), click **Import**
![image](https://user-images.githubusercontent.com/17661803/113946681-94c61a00-97ce-11eb-9b64-8453ae024f40.png)
9. Select the configuration file you saved in step 7.
    ![image](https://user-images.githubusercontent.com/17661803/113949084-081e5a80-97d4-11eb-9da2-c047eef10faa.png)
10. \[Optional] To avoid errors, delete the original configuration file from step 1.
      ![image](https://user-images.githubusercontent.com/17661803/113947323-094d8880-97d0-11eb-947d-7850573997ce.png)

That's it! You can now use the configuration file imported in step 9 without creating IP address conflicts. 
    



    

    
