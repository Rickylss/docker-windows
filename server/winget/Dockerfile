# escape=`
FROM mcr.microsoft.com/windows/server:ltsc2022 as winget

# Restore the default Windows shell for correct batch processing.
SHELL ["cmd", "/S", "/C"]

USER ContainerAdministrator
ADD [ "https://aka.ms/vs/17/release/vc_redist.x64.exe", "C:\\TEMP\\" ]
ADD [ "https://github.com/microsoft/winget-cli/releases/latest/download/Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle", "C:\\TEMP\\Microsoft.DesktopAppInstaller.zip" ]

RUN C:\TEMP\vc_redist.x64.exe /install /quiet /norestart /log C:\TEMP\vc_redist.log
RUN powershell -Command "Expand-Archive -LiteralPath C:\TEMP\Microsoft.DesktopAppInstaller.zip -DestinationPath C:\TEMP\winget-cli -Force"
RUN powershell -Command "ren C:\TEMP\winget-cli\AppInstaller_x64.msix AppInstaller_x64.zip"
RUN powershell -Command "Expand-Archive -LiteralPath C:\TEMP\winget-cli\AppInstaller_x64.zip -DestinationPath '%ProgramFiles(x86)%\winget-cli\' -Force"
RUN powershell -Command "[Environment]::SetEnvironmentVariable('Path', $Env:PATH + ';%ProgramFiles(x86)%\winget-cli', 'Machine')"

# Define the entry point for the Docker container.
# This entry point starts the developer command prompt and launches the PowerShell shell.
# ENTRYPOINT ["C:\\Program Files (x86)\\Microsoft Visual Studio\\2022\\BuildTools\\Common7\\Tools\\VsDevCmd.bat", "&&", "powershell.exe", "-NoLogo", "-ExecutionPolicy", "Bypass"]
