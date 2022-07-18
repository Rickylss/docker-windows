# escape=`
FROM mcr.microsoft.com/windows/server:ltsc2022 as buildtool

# Restore the default Windows shell for correct batch processing.
SHELL ["cmd", "/S", "/C"]

USER ContainerAdministrator
ADD [ "https://aka.ms/vs/17/release/vc_redist.x64.exe", "C:\\TEMP\\" ]
RUN C:\TEMP\vc_redist.x64.exe /install /passive /norestart /log C:\TEMP\vc_redist.log

ADD [ "https://raw.githubusercontent.com/MisterDA/Windows-OCaml-Docker/images/Install.cmd", "C:\\TEMP\\" ]
ADD [ "https://aka.ms/vscollect.exe", "C:\\TEMP\\collect.exe" ]
ADD [ "https://aka.ms/vs/17/release/channel", "C:\\TEMP\\VisualStudio.chman" ]
ADD [ "https://aka.ms/vs/17/release/vs_buildtools.exe", "C:\\TEMP\\vs_buildtools.exe" ]

# It takes a long time to download this workloads and component,
# you can do it after docker build.
RUN C:\TEMP\Install.cmd C:\TEMP\vs_buildtools.exe --quiet --wait --norestart --nocache install `
    --installPath "%ProgramFiles(x86)%\Microsoft Visual Studio\2022\BuildTools" `
    --channelUri C:\TEMP\VisualStudio.chman `
    --installChannelUri C:\TEMP\VisualStudio.chman `
    --add Microsoft.VisualStudio.Workload.ManagedDesktopBuildTools `
    --add Microsoft.VisualStudio.Workload.VCTools `
    --add Microsoft.VisualStudio.Workload.UniversalBuildTools `
    --add Microsoft.VisualStudio.Workload.MSBuildTools `
    --add Microsoft.VisualStudio.Component.VC.Tools.x86.x64 `
    --add Microsoft.VisualStudio.Component.Windows11SDK.22000 `
    --add Microsoft.VisualStudio.Component.VC.14.32.17.2.x86.x64.Spectre

# If you want more componets after docker build, 
# run vs_buildtools.exe modify in the container.
# RUN C:\TEMP\Install.cmd C:\TEMP\vs_buildtools.exe --quiet --wait --norestart --nocache modify `
#     --installPath "%ProgramFiles(x86)%\Microsoft Visual Studio\2022\BuildTools" `
#     --channelUri C:\TEMP\VisualStudio.chman `
#     --installChannelUri C:\TEMP\VisualStudio.chman `
#     --add Microsoft.VisualStudio.Component.VC.14.29.16.11.x86.x64.Spectre

ADD [ "https://github.com/microsoft/winget-cli/archive/master.zip", "C:\\TEMP\\winget-cli.zip" ]
RUN powershell -Command "Expand-Archive -LiteralPath C:\TEMP\winget-cli.zip -DestinationPath C:\TEMP\ -Force"
RUN cd C:\TEMP && rename winget-cli-master winget-cli
RUN cd "%ProgramFiles(x86)%\Microsoft Visual Studio\2022\BuildTools\VC\Auxiliary\Build" && vcvarsall.bat x64 && cd C:\TEMP\winget-cli && msbuild -t:restore -m -p:RestorePackagesConfig=true -p:Configuration=Release src\AppInstallerCLI.sln
RUN cd "%ProgramFiles(x86)%\Microsoft Visual Studio\2022\BuildTools\VC\Auxiliary\Build" && vcvarsall.bat x64 && cd C:\TEMP\winget-cli && msbuild -m -p:Configuration=Release src\AppInstallerCLI.sln
RUN mkdir "%ProgramFiles(x86)%\winget-cli" && move "C:\TEMP\winget-cli\src\x64\Release\AppInstallerCLI\winget.exe" "%ProgramFiles(x86)%\winget-cli\" && move "C:\TEMP\winget-cli\src\x64\Release\AppInstallerCLI\resources.pri" "%ProgramFiles(x86)%\winget-cli\"

# Define the entry point for the Docker container.
# This entry point starts the developer command prompt and launches the PowerShell shell.
# ENTRYPOINT ["C:\\Program Files (x86)\\Microsoft Visual Studio\\2022\\BuildTools\\Common7\\Tools\\VsDevCmd.bat", "&&", "powershell.exe", "-NoLogo", "-ExecutionPolicy", "Bypass"]
