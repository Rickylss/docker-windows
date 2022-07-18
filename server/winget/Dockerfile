# escape=`
FROM mcr.microsoft.com/windows/server:ltsc2022 as winget

# Restore the default Windows shell for correct batch processing.
SHELL ["cmd", "/S", "/C"]

USER ContainerAdministrator
ADD [ "https://aka.ms/vs/17/release/vc_redist.x64.exe", "C:\\TEMP\\" ]
RUN C:\TEMP\vc_redist.x64.exe /install /passive /norestart /log C:\TEMP\vc_redist.log

RUN mkdir "%ProgramFiles(x86)%\winget-cli"
ADD [ "winget.exe", "%ProgramFiles(x86)%\winget-cli" ]
ADD [ "resources.pri", "%ProgramFiles(x86)%\winget-cli" ]

# Define the entry point for the Docker container.
# This entry point starts the developer command prompt and launches the PowerShell shell.
# ENTRYPOINT ["C:\\Program Files (x86)\\Microsoft Visual Studio\\2022\\BuildTools\\Common7\\Tools\\VsDevCmd.bat", "&&", "powershell.exe", "-NoLogo", "-ExecutionPolicy", "Bypass"]
