.NET FRAMEWORK CLEANUP TOOL USER'S GUIDE

The latest version of this user's guide can be found at http://blogs.msdn.com/astebner/pages/8904493.aspx.

-----------------------------------
INTRODUCTION
-----------------------------------

This .NET Framework cleanup tool is designed to automatically perform a set of steps to remove selected versions 
of the .NET Framework from a computer.  It will remove files, directories, registry keys and values and Windows 
Installer product registration information for the .NET Framework.  The tool is intended primarily to return your 
system to a known (relatively clean) state in case you are encountering .NET Framework installation, 
uninstallation, repair or patching errors so that you can try to install again.

There are a couple of very important caveats that you should review before using this tool to remove any version 
of the .NET Framework from your system:

  * This tool is designed as a last resort for cases where install, uninstall, repair or patch installation did not 
    succeed for unusual reasons.  It is not a substitute for the standard uninstall procedure.  You should try the 
    steps listed in this blog post before using this cleanup tool.
 
  * This cleanup tool will delete shared files and registry keys used by other versions of the .NET Framework.  If 
    you run the cleanup tool, you will need to perform a repair/re-install for all other versions of the .NET 
    Framework that are on your computer or they will not work correctly afterwards.

-----------------------------------
DOWNLOAD LOCATIONS
-----------------------------------

The .NET Framework cleanup tool is available for download at the following locations:

http://cid-27e6a35d1a492af7.skydrive.live.com/self.aspx/Blog_Tools/dotnetfx_cleanup_tool.zip 
http://blogs.msdn.com/astebner/attachment/8904493.ashx

The .zip file that contains the tool also contains a file named history.txt that lists when the most recent version 
of the tool was published and what changes have been made to the tool over time.

-----------------------------------
SUPPORTED PRODUCTS
-----------------------------------

The .NET Framework cleanup tool supports removing the following products:

  * .NET Framework - All Versions 
  * .NET Framework - All Versions (Tablet PC and Media Center) 
  * .NET Framework - All Versions (Windows Server 2003) 
  * .NET Framework - All Versions (Windows Vista and Windows Server 2008) 
  * .NET Framework 1.0 
  * .NET Framework 1.1 
  * .NET Framework 2.0 
  * .NET Framework 3.0 
  * .NET Framework 3.5
  * .NET Framework 4
  * .NET Framework 4.5

Not all of the above products will appear in the UI for the .NET Framework cleanup tool on every operating system.
The cleanup tool contains logic so that if it is run on an OS version that includes the .NET Framework as an OS 
component, it will not offer the option to clean it up.  This means that running the cleanup tool on Windows XP 
Media Center Edition or Tablet PC Edition will not offer the option to clean up the .NET Framework 1.0, running 
it on Windows Server 2003 will not offer the option to clean up the .NET Framework 1.1 and running it on Windows 
Vista or Windows Server 2008 will not offer the option to clean up the .NET Framework 2.0 or the .NET Framework 3.0.

When choosing to remove any of the above versions of the .NET Framework, the cleanup tool will also remove any 
associated hotfixes and service packs.  You do not need to run any separate steps to remove the service pack(s) 
for a version of the .NET Framework.

-----------------------------------
SILENT MODE
-----------------------------------

The .NET Framework cleanup tool supports running in silent mode.  In this mode, the tool will run without showing 
any UI, and the user must pass in a version of the .NET Framework to remove as a command line parameter.  To run 
the cleanup tool in silent mode, you need to download the cleanup tool, extract the file cleanup_tool.exe from 
the zip file, and then run it using syntax like the following: 

    cleanup_tool.exe /q:a /c:"cleanup.exe /p <name of product to remove>"

The value that you pass with the /p switch to replace <name of product to remove> in this example must exactly 
match one of the products listed in the Supported products section above.  For example, if you would like to run 
the cleanup tool in silent mode and remove the .NET Framework 1.1, you would use a command line like the following: 

    cleanup_tool.exe /q:a /c:"cleanup.exe /p .NET Framework 1.1"

One important note – as indicated above, the cleanup tool will not allow you to remove a version of the .NET 
Framework that is installed as part of the OS it is running on.  That means that even if you try this example 
command line on Windows Server 2003, the tool will exit with a failure return code and not allow you to remove 
the .NET Framework 1.1 because it is a part of that OS. 

Similarly, you cannot use the cleanup tool to remove the .NET Framework 1.0 from Windows XP Media Center Edition 
or Windows XP Tablet PC Edition or remove the .NET Framework 2.0 or 3.0 from Windows Vista or Windows Server 2008.
In addition, if you run the cleanup tool on an OS that has any edition of the .NET Framework installed as a part 
of the OS, it will prevent you from using the .NET Framework - All Versions option because there is at least one 
version that it cannot remove.

If you are planning to run the cleanup tool in silent mode, you need to make sure to detect what OS it is running 
on and not pass in a version of the .NET Framework with the /p switch that is a part of the OS or make sure that 
you know how to handle the failure exit code that you will get back from the cleanup tool in that type of scenario. 

-----------------------------------
UNATTENDED MODE
-----------------------------------

The .NET Framework cleanup tool supports running in silent mode.  In this mode, the tool will run and only show a 
progress dialog during removal, but will require no user interaction.  Unattended mode requires the user to pass 
in a version of the .NET Framework to remove as a command line parameter.  To run the cleanup tool in unattended 
mode, you need to download the cleanup tool, extract the file cleanup_tool.exe from the zip file, and then run it 
using syntax like the following: 

    cleanup_tool.exe /q:a /c:"cleanup.exe /p <name of product to remove> /u"

For example, if you would like to run the cleanup tool in unattended mode and remove the .NET Framework 1.1, you 
would use a command line like the following: 

    cleanup_tool.exe /q:a /c:"cleanup.exe /p .NET Framework 1.1 /u" 

-----------------------------------
EXIT CODES
-----------------------------------

The cleanup tool can returns the following exit codes:

  * 0    - cleanup completed successfully for the specified product 
  * 3010 - cleanup completed successfully for the specified product and a reboot is required to complete the 
           cleanup process 
  * 1    - cleanup tool requires administrative privileges on the machine 
  * 2    - the required file cleanup.ini was not found in the same path as cleanup.exe 
  * 3    - a product name was passed in that cannot be removed because it is a part of the OS on the system 
           that the cleanup tool is running on 
  * 4    - a product name was passed in that does not exist in cleanup.ini 
  * 100  - cleanup was able to start but failed during the cleanup process 
  * 1602 - cleanup was cancelled

-----------------------------------
LOG FILES
-----------------------------------

The cleanup tool creates the following log files:

  * %temp%\cleanup_main.log    - a log of all activity during each run of the cleanup tool; this is a superset 
                                 of the logs listed below as well as some additional information 
  * %temp%\cleanup_actions.log - a log of actions taken during removal of each product; it will list files that 
                                 it finds and removes, product codes it tries to remove, registry entries it 
                                 tries to remove, etc. 
  * %temp%\cleanup_errors.log  - a log of errors and warnings encountered during each run of the cleanup tool

-----------------------------------
.NET FRAMEWORK DOWNLOAD LOCATIONS
-----------------------------------

If you plan to re-install the .NET Framework after running the cleanup tool, you can download the various versions
of the .NET Framework from the following locations:

.NET Framework 1.0

    http://www.microsoft.com/downloads/details.aspx?familyid=d7158dee-a83f-4e21-b05a-009d06457787

.NET Framework 1.0 SP3

    http://www.microsoft.com/downloads/details.aspx?familyid=6978d761-4a92-4106-a9bc-83e78d4abc5b

.NET Framework 1.1

    http://www.microsoft.com/downloads/details.aspx?FamilyId=262D25E3-F589-4842-8157-034D1E7CF3A3

.NET Framework 1.1 SP1

    http://www.microsoft.com/downloads/details.aspx?familyid=a8f5654f-088e-40b2-bbdb-a83353618b38

.NET Framework 2.0

    http://www.microsoft.com/downloads/details.aspx?FamilyID=0856eacb-4362-4b0d-8edd-aab15c5e04f5

.NET Framework 2.0 with SP1

    http://www.microsoft.com/downloads/details.aspx?FamilyId=79BC3B77-E02C-4AD3-AACF-A7633F706BA5

.NET Framework 2.0 with SP2

    http://www.microsoft.com/downloads/details.aspx?FamilyID=5b2c0358-915b-4eb5-9b1d-10e506da9d0f

.NET Framework 3.0

    http://www.microsoft.com/downloads/details.aspx?FamilyID=10CC340B-F857-4A14-83F5-25634C3BF043

.NET Framework 3.0 with SP1

    http://www.microsoft.com/downloads/details.aspx?FamilyId=EC2CA85D-B255-4425-9E65-1E88A0BDB72A

.NET Framework 3.5

    http://www.microsoft.com/downloads/details.aspx?FamilyId=333325FD-AE52-4E35-B531-508D977D32A6

.NET Framework 3.5 with SP1

    http://www.microsoft.com/downloads/details.aspx?familyid=AB99342F-5D1A-413D-8319-81DA479AB0D7

.NET Framework 4 Full

    http://www.microsoft.com/downloads/details.aspx?FamilyID=0a391abd-25c1-4fc0-919f-b21f31ab88b7

.NET Framework 4 Client Profile

    http://www.microsoft.com/downloads/details.aspx?FamilyID=e5ad0459-cbcc-4b4f-97b6-fb17111cf544
