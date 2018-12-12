---
description: Setting up MoteChat
---

# MoteChat HelloWorld

**Required Environment**

_Node.js_

* **Step A**  


  * _Node.js_ source URL  `https://nodejs.org/en/`

  ​  


  * Download and install _Nodejs_ under '_C:\Program Files\nodejs'_ folder  __
  * Test the installation 
    * Open _&lt;Cmd&gt;_ shell and test if the installation was successful   `node -v //shows version of nodejs`

\*\*\*\*

* **Step B**  


  * Open _&lt;Cmd&gt;_ shell and change directory to the _Node.js_ folder   `cd C:\Program Files\nodejs`  
  * Initialize Nodejs' _Node Package Manager \(NPM\)_   
  
    `npm init //initialize the node package manager`

    ​

     ...c_ontinuing to press 'enter' until finished with all the arguments that the installation throws up_  
  

  *  Install **MoteBus** using NPM  `npm install motebus --save //install MoteBus`

  


  * Install **MoteChat** using NPM  `npm install motechat --save //install MoteChat`

  


  * Apply for an '**AppID'** by project name  
  * Configure the **setup** _JSON files_, namely, _config.json, device.json_ and _mote.json_ files located in _"/conf"_ folder  


    * **config.json**  
  

      ![](../.gitbook/assets/mc_api_config_json%20%281%29.png)

    \*\*\*\*

    \*\*\*\*

    * **device.json**   ![](../.gitbook/assets/mc_api_device_json%20%281%29.png) 

  
    ****

    * **mote.json**   ![](../.gitbook/assets/mc_api_mote_json%20%281%29.png) 

