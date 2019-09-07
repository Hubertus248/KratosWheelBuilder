FROM mcr.microsoft.com/windows/servercore:ltsc2019


# Restore the default Windows shell for correct batch processing below.
SHELL ["cmd", "/S", "/C"]

# Download the Build Tools bootstrapper.
RUN powershell.exe -Command \
	mkdir c:\TEMP; \
    wget https://aka.ms/vs/16/release/vs_buildtools.exe -OutFile c:\TEMP\vs_buildtools.exe

# Install Build Tools
RUN C:\TEMP\vs_buildtools.exe --quiet --wait --norestart --nocache \
    --add Microsoft.VisualStudio.Product.BuildTools \
    --add Microsoft.VisualStudio.Workload.VCTools \
    --add Microsoft.VisualStudio.Component.VC.CMake.Project \
    --add Microsoft.VisualStudio.Component.Windows10SDK.17763 \
    --add Microsoft.VisualStudio.Component.TestTools.BuildTools \
    --add Microsoft.VisualStudio.Component.VC.140 \
 || IF "%ERRORLEVEL%"=="3010" EXIT 0 ; \
 	$ErrorActionPreference = 'Stop'

#install 7zip
RUN powershell.exe -Command \
    [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; \
    wget https://www.7-zip.org/a/7z1900-x64.exe -OutFile c:\TEMP\7z.exe; \
    c:\TEMP\7z.exe /S /D="C:\7zip\\"

#install git
RUN powershell.exe -Command \
    [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; \
    wget https://github.com/git-for-windows/git/releases/download/v2.21.0.windows.1/Git-2.21.0-64-bit.exe -OutFile c:\TEMP\git.exe; \
    c:\TEMP\git.exe /SILENT


#install cmake
RUN powershell.exe -Command \
    [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; \
    wget https://github.com/Kitware/CMake/releases/download/v3.14.1/cmake-3.14.1-win64-x64.zip -OutFile c:\TEMP\cmake.zip; \
    mkdir c:\cmake; \
    c:\7zip\7z.exe x c:\TEMP\cmake.zip -o"c:\cmake"



#download and extract boost
RUN powershell.exe -Command \
    [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; \
    wget https://dl.bintray.com/boostorg/release/1.69.0/source/boost_1_69_0.zip -OutFile c:\TEMP\boost.zip; \
    mkdir c:\boost; \
    c:\7zip\7z.exe x c:\TEMP\boost.zip -o"c:\boost"


# Install python 3.4, clcache
#RUN powershell.exe -Command \
#    [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; \
#    wget https://www.python.org/ftp/python/3.4.4/python-3.4.4-amd64.exe -OutFile c:\temp\python34.exe; \
#    mkdir c:\python; \
#    mkdir c:\python\34; \
#    Start-Process c:\temp\python34.exe -ArgumentList '/quiet InstallAllUsers=1 PrependPath=0 TargetDir=c:\\python\\34' -Wait

#Install python 3.5
RUN powershell.exe -Command \
    [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; \
    wget https://www.python.org/ftp/python/3.5.4/python-3.5.4-amd64.exe -OutFile c:\temp\python35.exe; \
    mkdir c:\python\35; \
    Start-Process c:\temp\python35.exe -ArgumentList '/quiet InstallAllUsers=1 PrependPath=0 TargetDir=c:\\python\\35' -Wait; \
    c:\python\35\python.exe -m pip install --upgrade pip; \
    c:\python\35\python.exe -m pip install --upgrade setuptools wheel

#Install python 3.6
RUN powershell.exe -Command \
    [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; \
    wget https://www.python.org/ftp/python/3.6.8/python-3.6.8-amd64.exe -OutFile c:\temp\python36.exe; \
    mkdir c:\python\36; \
    Start-Process c:\temp\python36.exe -ArgumentList '/quiet InstallAllUsers=1 PrependPath=0 TargetDir=c:\\python\\36' -Wait; \
    c:\python\36\python.exe -m pip install --upgrade pip; \
    c:\python\36\python.exe -m pip install --upgrade setuptools wheel


#Install python 3.7
RUN powershell.exe -Command \
    [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; \
    wget https://www.python.org/ftp/python/3.7.4/python-3.7.4-amd64.exe -OutFile c:\temp\python37.exe; \
    mkdir c:\python\37; \
    Start-Process c:\temp\python37.exe -ArgumentList '/quiet InstallAllUsers=1 PrependPath=0 TargetDir=c:\\python\\37' -Wait; \
    c:\python\37\python.exe -m pip install --upgrade pip; \
    c:\python\37\python.exe -m pip install --upgrade setuptools wheel



#set env variables
ENV CMAKE c:\\cmake\\cmake-3.14.1-win64-x64\\bin\\cmake.exe
ENV SEVEN_ZIP c:\\7zip\\7z.exe
ENV VCVARS  \"C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\BuildTools\\VC\\Auxiliary\\Build\\vcvars64.bat\"
ENV BOOST c:\\boost\\boost_1_69_0
ENV BLAS c:\\libs\\libblas.lib
ENV LAPACK c:\\libs\\liblapack.lib
ENV PYTHON c:\\python

COPY build.ps1 c:\\scripts\\build.ps1
COPY configure.ps1 c:\\scripts\\configure.ps1
COPY configure.bat c:\\scripts\\configure.bat
COPY setup.py c:\\scripts\\setup.py
COPY __init__.py c:\\scripts\\__init__.py

# Default to PowerShell if no other command specified.
CMD ["powershell.exe", "-NoLogo", "-ExecutionPolicy", "Bypass", "-Command", "c:\\scripts\\build.ps1"]
