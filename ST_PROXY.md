How to install the python packages with pip through ST Proxy
===
Author : Leo WU ([leo.wu@st.com](mailto:leo.wu@st.com))

Version : 1.0.0 (2023/08/19)

### NOTE

I assume that you know how to create the python environment and the basic commands of python and pip, and the basic knowledge of internet, 
but in this tutorial you can ignore all of it, just follow the procedures

Here, I will list some basic knowledge, if you are not familiar with these, you can search it on Google

- Anaconda - The tool which managing the python environment
- URL format - The format which is used to transfer the data through web
- Proxy connection format - The command

### Limitation
- Strongly recommend DO NOT use '@' in your password, if you are using '@' in password, please change your password without using it, 
because it will generate some conflicts in proxy connection string


Abstract 
---
We are all struggling from the proxy connection issues in ST when we want to install some needed packages in our python environment  
But, there is almost no official documents to teach us how to use/follow the proxy policy to connect the server outside ST  
I spent a lot of time to figure out how to set up the configuration with ST proxy server, it was painful  
So, I want to share to all of you how to set up the configuration and save you time to find how to fix this issue  
I prepare some case studies to help you to configurate the setting, you can just choose the one which you prefer

Introduce ST Proxy
---
In ST, if you want to connect to internet, it will always connect to ST proxy server first then connecting to outside internet server

The connection script would in this link : http://lps-eu.st.com/

>You can download connection script from this link, but for this tutorial it's not necessary 

The IP address of ST proxy server is : 
- 165.225.76.32:80 (Europe)
- 165.225.112.8:80 (Pacific)
- 165.225.94.12:80 (Pacific)
> If you really want to know which IP is close too your region, you can download the connection script and check it  

The issue when you want to install the python package with pip
---
When you want to install some python packages through pip (python package manager), sometimes you may encounter this error message

![Proxy Error](Proxy_error.JPG)

This is because ST refuses you to connect to outside server without using ST proxy server, so you need to do some settings to connect to ST Proxy server first
then you can connect to outside server

Materials (You don't need to check it now, you can come back to find what you need after)
---
- Proxy connection strings   
Use this website (https://www.urlencoder.org) to encode the connection strings, it can help you to replace invalid strings  
Because if you want to proxy connection strings, you need to follow string protocol of Internet  
The proxy connection strings is    
  ```commandline
  http://USERNAME:PASSWORD@PROXY_SERVER:PORT
  ```
  So, for example, if your username is `leo wu` and password is `1234!QWER#`  
  Put it into the website, you can get the encoded string username would be `leo%20wu`, and the password would be `1234%21QWER%23`  
  Replace the `USERNAME`,`PASSWORD`,`PROXY_SERVER` and `PORT` to

  ```commandline
  http://leo%20wu:1234%21QWER%23@163.225.76.32:80
  ```
  This connection strings are the valid proxy connection strings  


- Windows Credential Mangers  
    Press `windows + R` to open the run dialog then key `control.exe keymgr.dll` in the string bar, it will open `Windows Credential Manager`  
    >Or you can open `control panel` find `Credential Manager` and open it
  
    Choose `Windows Credentials`  
    ![Windows Credential](Windows_Crendential.JPG)  
    Click `Add a Windows Credential`  
    ![Windows Credential_Add](Windows_Crendential_Add.jpg)  
    Key `165.225.76.32` in `Internet or network address`  
    Key your `ST windows login username` into `User Name`, your `password` into `Password`  
    > Here you don't need to replace 'space' or 'special character' the credential manager will help you to transfer it automatically   
  > 
    ![Windows Credential_info](Windows_Crendential_info.JPG)

    Then click `ok` to save it



Case : Using Anaconda to manage the python environment (Recommend)
---
There is a problem when you choose Anaconda to manage your python environment  
Because Anaconda has its own package manager, it will download the packages from their server.  
For most time, you can download all the famous packages from their server by using the `conda` command, but you still need to install pip,
in case they don't have the packages you want which always happen

But the different package manager would need to use different setting to connect to proxy server, it can't be avoided.  
Please follow the procedure to set up the configuration.

- For Windows Credential Part  
Please check `Windows Credential Mangers` in `Materials`  


- For conda part
please open your `user` folder ex:`C:\Users\leo wu`  
find a file named `.condarc` and open it with text editor (if it doesn't exist please create it by yourself)  
please copy below string into the text editor and save it  
  ```commandline
  ssl_verify: true
  channels:
    - defaults

  proxy_servers:
    http: http://165.225.76.32:80
    https: http://165.225.76.32:80

  ```


- For pip part (Thanks [Michael TANG](mailto:michael.tang@st.com) for providing this useful method)

    please open your `user` folder ex:`C:\Users\leo wu`    
    create a folder and name it `pip`  
    enter the `pip` folder and create a file named `pip.ini` then open it  
    Copy below strings into the text editor and save it  
    ```
    [global]
    proxy=http://165.225.76.32
    trusted-host = pypi.python.org
                    files.pythonhosted.org
                    pypi.org
    ```
    > But this setting will NOT let you download the packages from ST internal server, when you point the package source to ST internal server    
    
    > ST internal python package server : https://imgweb.gnb.st.com/python_packages/
    
    > If you want to install the package from ST internal server, I will suggest you download it and use `pip install` command to install the package 

    NOTICE :   
    Because ST will ask you change your password every 3 months, 
    so this method can easily help you to change the password without modifying the proxy connection string in serval places 
    when you change the password, you can just open the `Windows Credential Managers` and modify your password, the proxy setting will take effect immediately


And now, you can open conda command line window and activate your python environment and download the packages you want, 
no matter you are using `conda` or `pip`, it should not have any issue 

Case : Using python execution file to install the python environment
---
- Open the command, activate the python environment, then check the `Proxy connection strings` in `Materials`  
  For example, if your proxy connection strings are  
  ```commandline
  http://leo%20wu:1234%21QWER%23@163.225.76.32:80
  ```
  Then enter the following command into command line windows

  ```commandline
  SET HTTP_PROXY=http://leo%20wu:1234%21QWER%23@163.225.76.32:80
  ```
  ```commandline
  SET HTTPS_PROXY=http://leo%20wu:1234%21QWER%23@163.225.76.32:80
  ```
  Then you should be able to use pip to download the packages

  > The only problem of this method is it would be very annoying when you open a new command line window, you need to enter it again.   

  > Or you can add these parameter into system `Environment Variables`, but you need to have an administrator account 

Case : I don't want to know the details, just tell me what's the best solution and how to do !
---

Download Mini-Anaconda and install it : [Mini-Anaconda3](https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe)  
Follow the `Case : Using Anaconda to manage the python environment`




