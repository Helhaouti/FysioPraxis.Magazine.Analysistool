# escape=`

FROM python:3.12

# Restore the default Windows shell for correct batch processing.
SHELL ["cmd", "/S", "/C"]

# Copy our Install script.
COPY Install.cmd C:\TEMP\
# Copy the requirements.txt file to the container.
COPY requirements.txt C:\TEMP\

# Download collect.exe in case of an install failure.
ADD https://aka.ms/vscollect.exe C:\TEMP\collect.exe

# Use the latest release channel. 
ADD https://aka.ms/vs/17/release/channel C:\TEMP\VisualStudio.chman

# Download the Build Tools bootstrapper.
RUN ` 
    curl -SL --output vs_buildtools.exe https://aka.ms/vs/17/release/vs_buildtools.exe `
    # Install Build Tools with required workloads and components for Visual C++ 14.0 or greater.
    && (call C:\TEMP\Install.cmd vs_buildtools.exe --quiet --wait --norestart --nocache `
    --installPath "%ProgramFiles(x86)%\Microsoft Visual Studio\2022\BuildTools" `
    --channelUri C:\TEMP\VisualStudio.chman `
    --installChannelUri C:\TEMP\VisualStudio.chman `
    # This workload includes the latest Visual C++ build tools for Desktop development
    --add Microsoft.VisualStudio.Workload.MSBuildTools ` 
    --add Microsoft.VisualStudio.Workload.VCTools ` 
    --add Microsoft.VisualStudio.Component.VC.Tools.x86.x64 `
    --add Microsoft.VisualStudio.Component.Windows11SDK.22621 `
    --add Microsoft.VisualStudio.Component.VC.CMake.Project `
    --add Microsoft.VisualStudio.Component.TestTools.BuildTools `
    --add Microsoft.VisualStudio.Component.VC.ASAN `
    ) `
    # Cleanup
    && del /q vs_buildtools.exe

# Install PyInstaller.
RUN pip install pyinstaller