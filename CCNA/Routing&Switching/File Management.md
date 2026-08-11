# File Management

> `write memory` or `copy running-config startup-config` can be used to make our device config persistant.
> Startup config is saved in NVRAM of device while running config is in RAM.
> *NVRAM => Non Volatile Random Access Memory*

> Flash is the disk that stores image IOS, VLAN's file and some other files in device. we can see its content with `dir flash:/`

1. `copy <src-path> <dst-path>`: copy or take backup of a file
2. `delete <file-path>`: to delete a file
3. `copy tftp: flash:` OR  `copy tftp://<TFTP-srv-IP>/<file-name> flash:<file-name>`: then give remote TFTP server ip and src file name and its dst name to copy device ios from TFTP to device
4. `more flash:<text-file>`: to read a text file in flash
5. `verify /md5 flash:<file-name>`: to see md5 hash of a file and verify its integrity
6. `copy ftp://<user-name>:<password>@<FTP-Srv-IP>/<file-name> flash:`: downloads a file from FTP server to device
7. `dir flash:/`: to see flash contents
8. `dir nvram:/`: to see nvram contents

> *FTP => File Transfer Protocol* -> works on port 21 (control), 20 (data)
> *SFTP => Secure FTP* -> Actually doesnt use FTP, it uses SSH (port 22) to transfer file
> *SCP => Secure CoPy* -> Same as SFTP
> *FTPS => FTP Secure* -> works with FTP but uses Certificates for security

> [!NOTE]
> FTP is much more faster than TFTP.
