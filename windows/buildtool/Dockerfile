# escape=`
FROM mcr.microsoft.com/windows:20H2 as winget-builder

# Restore the default Windows shell for correct batch processing.
SHELL ["cmd", "/S", "/C"]

USER ContainerAdministrator
ADD [ "https://aka.ms/vs/17/release/vc_redist.x64.exe", "C:\\TEMP\\" ]
RUN C:\TEMP\vc_redist.x64.exe /install /passive /norestart /log C:\TEMP\vc_redist.log

ADD [ "Install.cmd", "C:\\TEMP\\" ]
ADD [ "https://aka.ms/vscollect.exe", "C:\\TEMP\\collect.exe" ]
ADD [ "https://aka.ms/vs/17/release/channel", "C:\\TEMP\\VisualStudio.chman" ]
ADD [ "https://aka.ms/vs/17/release/vs_buildtools.exe", "C:\\TEMP\\vs_buildtools.exe" ]

# RUN C:\TEMP\Install.cmd C:\TEMP\vs_buildtools.exe --quiet --wait --norestart --nocache install --installPath "%ProgramFiles(x86)%\Microsoft Visual Studio\2022\BuildTools" --channelUri C:\TEMP\VisualStudio.chman --installChannelUri C:\TEMP\VisualStudio.chman --add Microsoft.VisualStudio.Workload.ManagedDesktopBuildTools --add Microsoft.VisualStudio.Workload.VCTools --add Microsoft.VisualStudio.Workload.UniversalBuildTools --add Microsoft.VisualStudio.Workload.MSBuildTools --add Microsoft.VisualStudio.Component.VC.Tools.x86.x64 --add Microsoft.VisualStudio.Component.Windows11SDK.22000 --add Microsoft.VisualStudio.Component.VC.14.32.17.2.x86.x64.Spectre

ADD [ "winget-cli-master.zip", "C:\\TEMP\\winget-cli.zip" ]
RUN powershell -Command "Expand-Archive -LiteralPath C:\TEMP\winget-cli.zip -DestinationPath C:\TEMP\ -Force"
RUN cd C:\TEMP && rename winget-cli-master winget-cli
# RUN cd "%ProgramFiles(x86)%\Microsoft Visual Studio\2022\BuildTools\VC\Auxiliary\Build" && vcvarsall.bat x64 && cd C:\TEMP\winget-cli && msbuild -t:restore -m -p:RestorePackagesConfig=true -p:Configuration=Release src\AppInstallerCLI.sln
# RUN cd "%ProgramFiles(x86)%\Microsoft Visual Studio\2022\BuildTools\VC\Auxiliary\Build" && cd C:\TEMP\winget-cli && msbuild -m -p:Configuration=Release src\AppInstallerCLI.sln
# RUN mkdir "C:\Program Files\winget-cli"
# RUN move "C:\TEMP\winget-cli\src\x64\Release\AppInstallerCLI\winget.exe" "C:\Program Files\winget-cli\winget.exe"
# RUN move "C:\TEMP\winget-cli\src\x64\Release\AppInstallerCLI\resources.pri" "C:\Program Files\winget-cli\"

# Define the entry point for the Docker container.
# This entry point starts the developer command prompt and launches the PowerShell shell.
# ENTRYPOINT ["C:\\Program Files (x86)\\Microsoft Visual Studio\\2022\\BuildTools\\Common7\\Tools\\VsDevCmd.bat", "&&", "powershell.exe", "-NoLogo", "-ExecutionPolicy", "Bypass"]

# RUN C:\TEMP\Install.cmd C:\TEMP\vs_buildtools.exe --norestart --nocache modify --installPath "%ProgramFiles(x86)%\Microsoft Visual Studio\2022\BuildTools" --channelUri C:\TEMP\VisualStudio.chman --installChannelUri C:\TEMP\VisualStudio.chman --add Microsoft.VisualStudio.Component.VC.14.29.16.11.x86.x64.Spectre