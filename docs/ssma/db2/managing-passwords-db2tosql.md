---
title: "Managing Passwords (DB2ToSQL)"
description: "Managing Passwords (DB2ToSQL)"
author: cpichuka
ms.author: cpichuka
ms.date: 07/05/2020
ms.service: sql
ms.subservice: ssma
ms.topic: conceptual
ms.collection:
  - sql-migration-content
---
# Managing Passwords (DB2ToSQL)
This section is about securing database passwords and the procedure to import or export them across servers:  
  
1.  Securing Password  
  
2.  Exporting or Importing Encrypted Password  
  
## Securing Password  
SSMA allows you to secure your password of a database.  
  
Use the following procedure to implement a secure connection:  
  
Specify a valid password using one of the following three methods:  
  
1.  **Clear Text:** Type the database password in the value attribute of the 'password' node. It is found under the server definition node in the Server section of the script file or server connection file.  
  
    Passwords in clear text are not secure. Therefore, you will encounter the following warning message in the console output: *"Server &lt;server-id&gt; password is provided in non-secure clear text form, SSMA Console application provides an option to protect the password through encryption, please see -securepassword option in SSMA help file for more information."*  
  
    **Encrypted Passwords:** The specified password, in this case, is stored in an encrypted form on the local machine in ProtectedStorage.ssma.  
  
    -   **Securing Passwords**  
  
        -   Execute the `SSMAforDB2Console.exe` with the `-securepassword` and add switch at command line passing the server connection or script file containing the password node in the server definition section.  
  
        -   At prompt, the user is asked to enter the database password and confirm it.  
  
            The server definition ids and its corresponding encrypted passwords are stored in a file on the local machine  
            
            Example 1:
            
            ```console
            Specify password
            C:\SSMA\SSMAforDB2Console.EXE -securepassword -add all -s "D:\Program Files\Microsoft SQL Server Migration Assistant for DB2\Sample Console Scripts\AssessmentReportGenerationSample.xml" -v "D:\Program Files\Microsoft SQL Server Migration Assistant for DB2\Sample Console Scripts\ VariableValueFileSample.xml"
            
            Enter password for server_id 'XXX_1': xxxxxxx
            
            Re-enter password for server_id 'XXX_1': xxxxxxx
            ```
            
            Example 2:
            
            ```console
            C:\SSMA\SSMAforDB2Console.EXE -securepassword -add "source_1,target_1" -c "D:\Program Files\Microsoft SQL Server Migration Assistant for DB2\Sample Console Scripts\ServersConnectionFileSample.xml" - v "D:\Program Files\Microsoft SQL Server Migration Assistant for DB2\Sample Console Scripts\ VariableValueFileSample.xml" -o
            
            Enter password for server_id 'source_1': xxxxxxx
            
            Re-enter password for server_id 'source_1': xxxxxxx
            
            Enter password for server_id 'target_1': xxxxxxx
            
            Re-enter password for server_id 'target _1': xxxxxxx  
            ```
    
    -   **Removing Encrypted Passwords**  
  
        Execute the `SSMAforDB2Console.exe` with the`-securepassword` and `-remove` switch at command line passing the server ids, to remove the encrypted passwords from the protected storage file present on the local machine.  
  
        Example:  

        ```console
        C:\SSMA\SSMAforDB2Console.EXE -securepassword -remove all
        C:\SSMA\SSMAforDB2Console.EXE -securepassword -remove "source_1,target_1"
        ```

    -   **Listing Server Ids whose passwords are encrypted**  
  
        Execute the `SSMAforDB2Console.exe` with the `-securepassword` and `-list` switch at command line to list all the server ids whose passwords have been encrypted.  
  
        Example:  

        ```console
        C:\SSMA\SSMAforDB2Console.EXE -securepassword -list
        ```

    > [!NOTE]  
    > 1.  The password in clear text mentioned in script or server connection file takes precedence over the encrypted password in secured file.  
    > 2.  When no password exists in the server section of the server connection file or the script file or if it has not been secured on the local machine, the console prompts you to enter the password.  
  
## Exporting or Importing Encrypted Passwords  
The SSMA Console application allows you to export encrypted database passwords present in a file on the local machine to a secured file and vice-versa. It helps in making the encrypted passwords machine independent.

_Export functionality_ reads the server id and password from the local protected storage. The system then saves the id and password in an encrypted file. The user is prompted to enter the password for the secured file. Make sure the password entered is 8 or more characters in length. This secured file is portable across different machines.

_Import functionality_ reads the server id and password information from the secured file. The user is prompted to enter the password for the secured file, and appends the information to the local protected storage.  

### Export example

1. Export the password.

2. Enter the password for protecting the exported file.

3. Run: &nbsp; `C:\SSMA\SSMAforDB2Console.EXE -securepassword -export all "machine1passwords.file"`

4. Enter the password for protecting the exported file: xxxxxxxx

5. Confirm the password: xxxxxxxx

6. Run: &nbsp; `C:\SSMA\SSMAforDB2Console.EXE -p -e "DB2DB_1_1,Sql_1" "machine2passwords.file"`

7. Enter the password for protecting the exported file: xxxxxxxx

8. Confirm the password: xxxxxxxx  

### Import example

1. Import an encrypted password.

2. Enter the password for protecting the imported file.

3. Run: &nbsp; `C:\SSMA\SSMAforDB2Console.EXE -securepassword -import all "machine1passwords.file"`

4. Enter the password to import the servers from encrypted file: xxxxxxxx

5. Confirm the password: xxxxxxxx

6. Run: &nbsp; `C:\SSMA\SSMAforDB2Console.EXE -p -i "DB2DB_1,Sql_1" "machine2passwords.file"`

7. Enter the password to import the servers from encrypted file: xxxxxxxx

8. Confirm password: xxxxxxxx

## See Also  
[Executing the SSMA Console](./executing-the-ssma-console-db2tosql.md)  
