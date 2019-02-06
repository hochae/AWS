# AWS
AWS practice

## EC2 launch
- As the memory usage of VS code on mate core is over 1G, we need to install at least t2.small
  - vscode + file browser(Caja) + terminal + chrome browser(2 open tabs) + Htop
- t2.small looks just managible for small java test program.
- t2.medium is recommended...

![Htop on t2.small](https://drive.google.com/file/d/1TBZHCuesZwgofKKODKBaf_oGBq0m56Zm/view?usp=sharing)

## GUI using VNC with Amazon EC2 Instances
- Install GUI on ubuntu server of EC2
  - using 'tasksel'
    ```
    sudo apt-get install tasksel
    ```
  - check the list of tasks
    ```
    tasksel --list-tasks
    ```
  - selected mate core and install it on the server
    ```
    sudo apt update
    sudo apt upgrade
    sudo tasksel install ubuntu-mate-core
    ```
  - install vnc4server
    ```
    sudo apt-get install vnc4server
    ```
  - set vncserver password
    ```
    vncserver
    ```
  - modify ~/.vnc/xstartup as below
    ```
    #!/bin/sh
    # Uncomment the following two lines for normal desktop:
    unset SESSION_MANAGER         ## uncomment
    exec /etc/X11/xinit/xinitrc   ## uncomment

    [ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
    [ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
    xsetroot -solid grey
    vncconfig -iconic &

    ## commented out following two lines
    #x-terminal-emulator -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &
    #x-window-manager &
    ```
  - Reboot the instance
  - ssh connection with turneling option (ex: rmote forwarding port:5901, vncserver port:5901)
    ```
    ssh -L 5901:localhost:5901 -i <filename.pem> ubuntu@<public ip or DNS>
    ```
  - vnc connection (ex: Mac)
    ```
    vnc://localhost:5901
    ```
  
- ref
  - https://linuxconfig.org/install-gui-on-ubuntu-server-18-04-bionic-beaver
  - https://medium.com/@Arafat./graphical-user-interface-using-vnc-with-amazon-ec2-instances-549d9c0969c5


## VNC connection to AWS linux instance from MS windows
- [ssh connection](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html)
  - application : [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
  - convert private key, .pem to .ppk : PuTTYgen
  - set turneling for vnc connection
    - [ssh turneling](https://www.ssh.com/ssh/tunneling/example)
- VNC: [tightvnc](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html)

## Using VS code on ubuntu mate core
- Issue
  - [VS Code not working on Ubuntu when connected using XRDP](https://github.com/Microsoft/vscode/issues/3451)

- My solution
  - modified ''/usr/lib/x86_64-linux-gnu/libxcb.so.1.1.0'' as above reference
    ```
    sudo sed -i 's/BIG-REQUESTS/_IG-REQUESTS/' /usr/lib/x86_64-linux-gnu/libxcb.so.1.1.0
    ```
  - reboot the instance
  - login with turneling option
  - [download vscode and install vscode](https://code.visualstudio.com/docs/setup/linux)
    ```
    sudo apt install ./<downloaded file>.deb
    ```
  - enjoy vscode

## Appendix: 
- [Editing files in your Linux Virtual Machine made a lot easier with Remote VSCode](https://medium.com/@prtdomingo/editing-files-in-your-linux-virtual-machine-made-a-lot-easier-with-remote-vscode-6bb98d0639a4)
- [XCB](https://xcb.freedesktop.org/tutorial/)
- [Compare RDP vs VNC in Simple Language](https://www.xtontech.com/blog/rdp-vs-vnc-access/)
