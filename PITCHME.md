---?image=assets/images/gitpitch-audience.jpg
@title[Writing_UEFI_App_WIN_Lab]
<br><br><br><br><br>
## <span class="gold"   >UEFI & EDK II Training</span>

#### How to Write a UEFI Application
<span style="font-size:0.8em" >with Windows Labs</span>
<br>
<span style="font-size:0.75em" ><a href='http://www.tianocore.org'>tianocore.org</a></span><br>
<span style="font-size:0.5em" >See also <a href="https://github.com/tianocore-training/Writing_UEFI_App_WIN_Lab/blob/master/LabGuide.md">LabGuide.md</a> for Copy&Paste examples in labs</span>

Note:
  PITCHME.md for UEFI / EDK II Training  How to Write a UEFI Application Lab

  Copyright (c) 2020, Intel Corporation. All rights reserved.<BR>

  Redistribution and use in source (original document form) and 'compiled'
  forms (converted to PDF, epub, HTML and other formats) with or without
  modification, are permitted provided that the following conditions are met:

  1) Redistributions of source code (original document form) must retain the
     above copyright notice, this list of conditions and the following
     disclaimer as the first lines of this file unmodified.

  2) Redistributions in compiled form (transformed to other DTDs, converted to
     PDF, epub, HTML and other formats) must reproduce the above copyright
     notice, this list of conditions and the following disclaimer in the
     documentation and/or other materials provided with the distribution.

  THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR
  IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
  MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO
  EVENT SHALL TIANOCORE PROJECT  BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
  SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
  PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS;
  OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
  WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
  OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF
  ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.



---  
@title[Lesson Objective]
<BR>
### <p align="center"<span class="gold"   >Lesson Objective </span></p><br>

<!---  Add bullets using https://fontawesome.com/cheatsheet certificate
-->
<br>
<ul style="list-style-type:none">
 <li>@fa[certificate gp-bullet-green]<span style="font-size:0.9em">&nbsp;&nbsp;UEFI Application with PCDs</span> </li>
 <li>@fa[certificate gp-bullet-cyan]<span style="font-size:0.9em">&nbsp;&nbsp;Simple UEFI Application</span></li>
 <li>@fa[certificate gp-bullet-yellow]<span style="font-size:0.9em">&nbsp;&nbsp;Add functionality to UEFI Application</span> </li>
 <li>@fa[certificate gp-bullet-magenta]<span style="font-size:0.9em">&nbsp;&nbsp;Using EADK with UEFI Application</span></li>
</ul>

---?image=assets/images/binary-strings-black2.jpg
@title[UEFI Application w/ PCDs Section]
<br><br><br><br><br><br><br>
### <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;UEFI Application w/ PCDs </span>
<span style="font-size:0.9em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>


---?image=/assets/images/slides/Slide4.JPG
@title[EDK II PCD’s Purpose and Goals]
<br>
<p align="center"><span class="gold" ><b>EDK II PCD’s Purpose and Goals</b></span></p>
@fa[github gp-bullet-gold]<span style="font-size:0.7em">&nbsp;&nbsp;Documentaton :  <a href="https://github.com/tianocore/edk2/blob/master/MdeModulePkg/Universal/PCD/Dxe/Pcd.inf"> MdeModulePkg/Universal/PCD/Dxe/Pcd.inf  </a> </span>
<br>
<div class="left1">
<span style="font-size:01.0em" ><font color="cyan">Purpose</font></span>
<ul>
  <li><span style="font-size:0.8em" >Establishes platform common definitions </span>  </li>
  <li><span style="font-size:0.8em" >Build-time/Run-time aspects </span>  </li>
  <li><span style="font-size:0.8em" >Binary Editing Capabilities </span>  </li>
</ul>
</div>
<div class="right1">
<span style="font-size:01.0em" ><font color="cyan">Goals</font></span>
<ul>
  <li><span style="font-size:0.8em" >Simplify porting </span>  </li>
  <li><span style="font-size:0.8em" >Easy to associate with a module or platform </span>  </li>
</ul>
</div>

Note:

### Common definitions are established for platform component settings
- Standardize exposure of platform & module settings
- Help with Platform Porting
### Build-time aspects
- Component information is collected from PCD definitions that are associated with a given module
### Run-time aspects
- APIs are provided which allow access to component settings during the operation of the platform
### Binary Editing Capabilities
- A module can carry its own PCD data in the binary image and have it exposed so the data can be edited in the flash image



---?image=/assets/images/slides/Slide4.JPG
@title[PCD Syntax review]
### <p align="center"><span class="gold" >PCD Syntax</span></p>
<span style="font-size:0.9em">PCDs can be located anywhere within the Workspace even though a different package will use those PCDs for a given project</span>


@snap[west span-30 ]
<br>
<br>
<br>
<p align="center"><span style="font-size:01.49em; font-weight: bold;">@color[yellow](.DEC)</span></p>
@box[bg-gold2 text-black waved  ](<span style="font-size:01.280em; font-weight: bold;" >Define <br>PCD</span>)
<br>
@snapend

@snap[midpoint span-30 ]
<br>
<br>
<br>
<p align="center"><span style="font-size:01.49em; font-weight: bold;">@color[yellow](.INF)</span></p>
@box[bg-green-pp text-black waved   ](<span style="font-size:01.280em; font-weight: bold;" >Reference PCD</span>)
<br>
@snapend


@snap[east span-30 ]
<br>
<br>
<br>
<p align="center"><span style="font-size:01.49em; font-weight: bold;">@color[yellow](.DSC)</span></p>
@box[bg-lt-blue-pp text-black waved  ](<span style="font-size:01.280em; font-weight: bold;" >Modify <br>PCD</span>)
<br>
@snapend


@snap[west span-100 fragment]
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<p align="left"><span style="font-size:01.125em; font-weight: bold;"><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Package &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Module&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Platform</span></p>
@snapend


Note:

- The Platform configuration database is generated by the build process parsing the build description files that define and specify PCD entries.

- What we see on this slide is how the PCD data is being used in various levels of the build description files

- First we have the DEC file – this Defines a list of PCD tokens that modules can use. 
 	It Defines the PCD entries that will exist under the GUID for that  package, the PCD restriction, valid types for the PCD, and a default value for the PCD. There is a whole syntax and how to define a PCD in the DEC file.

- Next we have PCB entries in the INF file- and this Defines the usage of PCD tokens by the module.
	It Defines what PCD entries are being used within the module, the PCD restriction (or DYNAMIC for none), and a Optional default value for the PCD within this module only.

- Next is the DSC file – This is at the Platform level and describes the contents of the build for a specific platform. 
	PCD entries are assigned values and types for the platform build. You would define a value here to be used by that platform. The value could be different when it is defined in the DEC file but the value in the DSC would be the final value . And They Cannot conflict with established restrictions.

- Not on this slide but also there is the FDF build description File – and this file would have flash layout related values 


#### additional notes 
##### DEC:
- Defines list of PCD tokens that modules can use.

- Defines the PCD entries that will exist under the GUID for that  package, the PDC restriction, valid types for the PCD, and a default value for the PCD.
- The DEC file is part of a package.  Any package may define PCD entries. Any module that depends on a package may use the PCD entries defined in that packages' DEC file  

##### INF:
- Defines usage of PCD tokens by the module

- Defines what PCD entries are being used within the module, the PCD restriction (or DYNAMIC for none), and a default value for the PCD within this module only.
- The INF file also carries descriptive text for a given PCD entry


##### DSC:
- Platform level file which describes the contents of the build for a specific platform.
- PCD entries are assigned values and types for the platform build.  Cannot conflict with established restrictions.
- In most cases, PCD entries do not have SKU enabled and have a single value associated with them. However, a SKU PCD entry may have multiple values.



---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 1: Writing UEFI Applications with PCDs]
<br>
<br>
<p align="Left"><span class="gold" >Lab 1: Writing UEFI Applications with PCDs</span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab, you’ll  learn how to write UEFI applications with PCDs.</span>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:




---?image=/assets/images/slides/Slide7_1.JPG
@title[EDK II HelloWorld  App  Lab ]
<p align="right"><span class="gold" ><b>EDK II HelloWorld  App  Lab  </b></span></p>
<span style="font-size:0.8em" >First Setup for Building EDK II, See <a href="https://gitpitch.com/tianocore-training/Platform_Build_Win_Emulator_Lab/master#/9">Lab Setup for EmulatorPkg </a></span>
<p style="line-height:60%"><span style="font-size:0.8em" >Locate and Open </span><span style="font-size:0.6em" ><br>
<font face="Consolas">MdeModulePkg/Application/HelloWorld/HelloWorld.c</font></span></p>
<div class="left1">
<span style="font-size:0.8em" >Notice the PCD values</span><br>
<br>
<br>
<br>
<span style="font-size:0.78em" >Build EmulatorPkg Emulation </span><br>
<p style="line-height:70%"><span style="font-size:0.8em" >Then Run HelloWorld at the Shell command interface</span></p>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>
Note:

- So the steps for getting the source code, hopefully everyone did this prior to this training because it does take some time to download.
- So here are the steps:

- First create a directory, and for our example case we are using the directory "~src/Fw"

- Use instructions on wiki here: https://github.com/tianocore/tianocore.github.io/wiki/UDK2018-How-to-Build or use the lab material edk2 




 
---
@title[EDK II HelloWorld  App  Lab steps]
<p align="right"><span class="gold" ><b>EDK II HelloWorld  App  Lab  </b></span></p>
<span style="font-size:0.8em" >Open a VS  Command Prompt and type: <font face="Consolas">cd C:\FW\edk2</font> then </span>
```shell
 C:/FW/edk2-ws> Setenv.bat
 C:/FW/edk2-ws> cd edk2
 C:/FW/edk2-ws/ed2> edksetup
```
<span style="font-size:0.8em" >Build the EmulatorPkg Emulation</span>
```shell
  C:/FW/edk2-ws/edk2> Build –D ADD_SHELL_STRING 
  C:/FW/edk2-ws/edk2> RunEmulator.bat
```
<span style="font-size:0.8em" >At the UEFI Shell prompt</span>
```shell
Shell> Helloworld
UEFI Hello World!
Shell> 
```

<span style="font-size:0.79em" ><font color="cyan">How can we force the HelloWorld application to print out 3 times ?</font></span>
<br>
<span style="font-size:0.55em" >Note: RunEmulator.bat will run WinHost.exe from Build/EmulatorX64/DEBUG_<i>TAG</i>/X64 </span>

Note:

Same as slide

---?image=/assets/images/slides/Slide9.JPG
@title[EDK II HelloWorld  App  Lab location]
<p align="right"><span class="gold" ><b>EDK II HelloWorld  App  Lab  </b></span></p>
<br>
@fa[github gp-bullet-gold]<span style="font-size:0.7em"><a href="https://github.com/tianocore/edk2/tree/master/MdeModulePkg/Application/HelloWorld"> MdeModulePkg/Application/HelloWorld </a></span>


Note:

First let's look at the source code for the HellowWorld Application

---
@title[EDK II HelloWorld  App  Lab code]
<p align="right"><span class="gold" ><b>EDK II HelloWorld  App  Lab  </b></span></p>
<span style="font-size:01.0em" >Source: <font color="yellow">Helloworld.c</font></span>
```C
EFI_STATUS
EFIAPI
UefiMain (
  IN EFI_HANDLE        ImageHandle,
  IN EFI_SYSTEM_TABLE  *SystemTable
  )
{
	UINT32 Index;
	Index = 0;
  // Three PCD type (FeatureFlag, UINT32 
  // and String) are used as the sample.
  if (FeaturePcdGet (PcdHelloWorldPrintEnable)) {
  	for (Index = 0; Index < PcdGet32   (PcdHelloWorldPrintTimes); Index ++) {
  	  
  	  // Use UefiLib Print API to print
      // string to UEFI console
  	  
    	Print ((CHAR16*)PcdGetPtr (PcdHelloWorldPrintString));
    }
  }

  return EFI_SUCCESS;
}
```
@[12](PCD that is a boolean for if this feature is enabled)
@[13](PCD that is an integer <font face="Consolas">For</font> loop for how many times to print to the the string)
@[18](PCD that is a pointer for the string to print out) 

Note:


Source from Helloworld.c 



---?image=/assets/images/slides/Slide11.JPG
@title[EDK II HelloWorld  App  Lab solution]
<p align="right"><span class="gold" ><b>EDK II HelloWorld  App  Solution </b></span></p>
<p style="line-height:60%" align="left" ><span style="font-size:0.7em;" >1. Edit the file <font face="Consolas">C:/FW/edk2-ws/edk2/EmulatorPkg/EmulatorPkg.dsc</font><br><br>
After the section <font face="Consolas">[PcdsFixedAtBuild] </font> (search for "<font face="Consolas">PcdsFixedAtBuild</font>" or "Hello")
<br>
<br>
<br>
<br>
</span></p>

<p style="line-height:45%" align="left" ><span style="font-size:0.52em; font-family:Consolas;" ><font color="black"><br>
[PcdsFixedAtBuild]<br>
gEfiMdeModulePkgTokenSpaceGuid.PcdHelloWorldPrintTimes|3
</font>
</span></p>

<p style="line-height:60%" align="left" ><span style="font-size:0.7em;" ><br><br>2. 
Re-Build – Cd to <font face="Consolas">C:/FW/edk2-ws/edk2</font>
</span></p>

```shell
 $> Build –D ADD_SHELL_STRING
```

Note:

- look in file: <font face="Consolas">C:\fw\edk2\MdeModulePkg\MdeModulePkg.Dec</font>

- This PCD defines the print string.
-  This PCD is a sample to explain String typed PCD usage.
-  `gEfiMdeModulePkgTokenSpaceGuid.PcdHelloWorldPrintString|L"UEFI Hello World!\n"|VOID*|0x40000004`


1. Edit the file EmulatorPkg/EmulatorPkg.dsc
  - After the section [PcdsFixedAtBuild], add the new line :  
  - `[PcdsFixedAtBuild]`
  - `gEfiMdeModulePkgTokenSpaceGuid.PcdHelloWorldPrintTimes|3`

2. Re-Build – Cd to FW/edk2 dir 
  - `bash$ build -D ADD_SHELL_STRING`
  - RunEmulator.bat



---
@title[EDK II HelloWorld  App  Lab solution 02]
<p align="right"><span class="gold" ><b>EDK II HelloWorld  App  Solution </b></span></p>

<p style="line-height:60%" align="left" ><span style="font-size:0.7em;" >3. 
Run Emulation (run WinHost.exe from Build/EmulatorX64/ . . ./X64 )
</span></p>
```shell
  C:/FW/edk2-ws/edk2> RunEmulator.bat
```


<p style="line-height:60%" align="left" ><span style="font-size:0.7em;" >4. 
At the Shell prompt
</span></p>
```shell
Shell> Helloworld
UEFI Hello World!
UEFI Hello World!
UEFI Hello World!
Shell> 

```

<p style="line-height:60%" align="left" ><span style="font-size:0.7em;" >5.
Exit Emulation
</span></p>

```shell
Shell> Reset
```
<p style="line-height:70%" align="left" ><span style="font-size:0.8em;" >
@color[#A8ff60](How can we change the string of the HelloWorld application?)<br>
@size[.8em]( Also see  <font face="Consolas">../edk2/MdeModulePkg/MdeModulePkg.Dec</font>)

</span></p>


Note:
3. RunEmulator

4. At the Shell prompt
  - `Shell> Helloworld`
  - `UEFI Hello World!`
  - `UEFI Hello World!`
  - `UEFI Hello World!`
  - `Shell> `
5. Exit with Reset at the shell

- How can we change the string of the HelloWorld application?
- Also see  .../edk2/MdeModulePkg/MdeModulePkg.Dec




---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 2: Write a Simple UEFI Application]
<br>
<br>
<p align="Left"><span class="gold" >Lab 2: Write a Simple UEFI Application</span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab, you’ll learn how to write simple UEFI applications.</span>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:


---
@title[Lab 2 - Write A Simple App]
<p align="right"><span class="gold" ><b>LAB 2 - Writing a Simple UEFI Application</b></span></p>
<span style="font-size:0.9em"  >In this lab, you’ll learn how to write simple UEFI applications. </span>

@snap[north-west span-50 ]
<br>
<br>
<br>
<p style="line-height:45%" align="left" ><span style="font-size:0.9em" ><font color="cyan">"C" file</font></span></p>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:59% "><span style="font-size:0.9em;" ><br><br><br><br><br><br><br><br><br><br><br>&nbsp;</span></p>)
@snapend

@snap[north-east span-48]
<br>
<br>
<br>
<p style="line-height:45%" align="left" ><span style="font-size:0.9em" ><font color="yellow">&nbsp;&nbsp;.inf file</font></span></p>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:59% "><span style="font-size:0.9em;" ><br><br><br><br><br><br><br><br><br><br><br>&nbsp;</span></p>)
@snapend



@snap[south-west span-95 fragment ]
<ul style="line-height:0.8;">
  <li><span style="font-size:0.7em" >What goes into the Simplest "C"</span></li>
  <li><span style="font-size:0.7em" >Start with what should go into the Simplest .INF file</span></li>
</ul>  
<br>
@snapend
 
 
@snap[north-east span-98 ]
<br>
<br>
<br>
<p style="line-height:45%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" ><br><br>
EFI_STATUS <br>
EFIAPI <br>
UefiMain ( <br>
  IN EFI_HANDLE        ImageHandle, <br>
  IN EFI_SYSTEM_TABLE *SystemTable <br>
) <br>
{  <br>
  return EFI_SUCCESS; <br>
} <br>
</span></p>
@snapend
 
@snap[north-east span-46 ]
<br>
<br>
<br>
<p style="line-height:45%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" ><br><br>
[Defines] <br>
  INF_VERSION    &nbsp;&nbsp;&nbsp;=  <br>
  BASE_NAME    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=  <br>
  FILE_GUID     &nbsp;&nbsp;&nbsp;&nbsp; =  <br>
  MODULE_TYPE    &nbsp;&nbsp;&nbsp;=  <br>
  VERSION_STRING&nbsp;=  <br>
  ENTRY_POINT   &nbsp;&nbsp; =  <br>
  <br>
[Sources] <br>
  <br>
[Packages] <br>
  <br>
[LibraryClasses] <br>
</span></p>
@snapend


Note:

- Start with what should go into the Simplest .INF file
- What goes into a Simplest "C"


---?image=/assets/images/slides/Slide15.JPG
@title[Lab 2: Application Lab –start with .c and .inf template]
<p align="right"><span class="gold" ><b>Application Lab –start with .c and .inf template</b></span></p>

<p style="line-height:60%" align="left" ><span style="font-size:0.7em;" >
Copy the <font face="Consolas">LabSampleCode/SampleApp directory to C:/FW/edk2-ws/edk2</font>
</span></p>

@snap[south-west span-100]
<p style="line-height:60%" align="left" ><span style="font-size:0.7em;" >
<b>Edit</b> <font face="Consolas">SampleApp.inf</font>
</span></p>
<ul style="line-height:0.8;">
  <li><span style="font-size:0.7em" > Look in the INF for "XXXXXXXXXXX" sections that will need information  </span></li>
  <li><span style="font-size:0.7em" > Create <font face="Consolas">Name & GUID</font>, and then fill in the <font face="Consolas">MODULE_TYPE</font> </span></li>
</ul>  
<br>
<br>
@snapend


Note:

##### Steps
1. Copy the LabSampleCode/SampleApp directory to FW/edk2
2. Edit SampleApp.inf
  - Look in the INF for "XXXXXXXXXXX" sections that will need information  
  - Create Name & GUID, and then fill in the MODULE_TYPE 


---?image=/assets/images/slides/Slide16.JPG
@title[Lab 2: Sample Application INF file]
<p align="right"><span class="gold" ><b>Lab 2: Sample Application INF file</b></span></p>

<p style="line-height:40%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" ><font color="black">
<br><br>
<br><br>
[Defines] <br>
  INF_VERSION &nbsp;&nbsp;     =  0x00010005<br>
  BASE_NAME   &nbsp;&nbsp;&nbsp;&nbsp;     =  XXXXXXXXXXXXXXX <br>
  FILE_GUID   &nbsp;&nbsp;&nbsp;&nbsp;     =  XXXXXXXXXXXXXXX <br>
  MODULE_TYPE &nbsp;&nbsp;     =  XXXXXXXXXXXXXXX <br>
  VERSION_STRING =  1.0<br>
  ENTRY_POINT  &nbsp;&nbsp;    =  UefiMain<br>
<br>
[Sources]<br>
  XXXXXXXXX<br>
<br>
[Packages]<br>
  &num;XXXXXXXX<br>
  <br>
[LibraryClasses]<br>
  &num;XXXXXXXXXXXXX<br>
    <br>
[Guids]<br>
 &num; . . .<br>
</font></span></p>

<p align="right"><span style="font-size:0.5em">Get a GUID: <a href="http://www.guidgenerator.com/">guidgerator.com</a></span></p>

@snap[north-east span-49 ]
<br>
<br>
<br>
<br>
<p style="line-height:40%" align="left" ><span style="font-size:0.55em; font-family:Consolas;" ><br><br>
@color[red](&larr; ------) &nbsp;SampleApp <br> 
@color[red](&larr; ------) &nbsp;Get a GUID <a href="http://www.guidgenerator.com/">guidgerator.com</a> <br> 
@color[red](&larr; ------) &nbsp;UEFI_APPLICATION <br> 
<br>
<br>
<br>
<br>

@color[red](&larr; ------) &nbsp;SampleApp.c
<br><br>&nbsp;
</span></p>
@snapend



Note:

#####  to get a Guid - http://www.guidgenerator.com/

- Now here is a sample INF file
- This is for an application called SampleApp
- When the program starts is going to call the function called UefiMain inside the application. 
- The prototype for UefiMain is predetermined and you can see an example of a prototype by downloading a sample application. 
- So a couple of the things that the prototype will require are a pointer to the system table and a image handle parameter. 

Add the following:

```
 SampleApp
 UEFI_APPLICATION
 SampleApp.c
```




---?image=/assets/images/slides/Slide17.JPG
@title[Lab 2: Sample Application C file]
<p align="right"><span class="gold" ><b>Lab 2: Sample Application ‘C’ file</b></span></p>

<p style="line-height:40%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" >
<br><br>
<br><br>
<br><br><font color="blue"> 
/&ast;&ast; @file <br>
  This is a simple shell application <br>
&ast;&ast;/ </font><br><font color="black">
 EFI_STATUS <br>
 EFIAPI <br>
 UefiMain ( <br>
   IN EFI_HANDLE        ImageHandle, <br>
   IN EFI_SYSTEM_TABLE  *SystemTable <br>
   ) <br>
 { <br>
   @color[red](return) EFI_SUCCESS; <br>
 } <br>
</font>
</span></p>


@snap[south-east span-45 ]
<p style="line-height:45%" align="left" ><span style="font-size:02.02em">@color[#A8ff60](&larr;)<br> </span></p>
<br><br><br><br>
@snapend

@snap[south-east span-39 ]
@box[bg-navy text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.79em;" >@color[yellow](Does not do anything but return Success)<br>&nbsp;</span></p>)
<br>
<br>
<br>
@snapend


Note:

<pre>
/** @file
  This is a simple shell application
**/
 EFI_STATUS
 EFIAPI
 UefiMain (
   IN EFI_HANDLE        ImageHandle,
   IN EFI_SYSTEM_TABLE  *SystemTable
   )
 {
   return EFI_SUCCESS;
 }
</pre>

---
@title[Lab 2: Will it compile now?]
<p align="center"><span class="gold" ><b>Lab 2: Will it compile now?</b></span></p>
<br>
Not yet . . .
<br>

1. Need to add headers to the .C file 
2. Need to add a reference to INF from the platform DSC 
3. Need to add a few Package dependencies and libraries to the .INF 


Note:

- So the question is will it compile now?
- And the answer is no it will not compile yet
- First you need to add some headers to the.C. we need to be able to let some things.
- We need to add a reference to the INF from the platform in DSC.  Because If you build it now the build is going to say I don’t have a platform and so the build is going to break. 
- Then the next thing we need to add is a few package dependencies and libraries to the INF file because, for instance, features like the UEFI Application entry point will need to be added, because it doesn’t know how to do an entry point until you’ve added that. 

---
@title[Lab 2: Application Lab – Update Files]
<p align="right"><span class="gold" ><b>Lab 2: Application Lab – Update Files</b></span></p>
<ul style="list-style-type:none; line-height:0.95;">
 <li><span style="font-size:0.8em" >1.&nbsp;&nbsp; <font color="yellow" face="Consolas">.DSC </font> (EmulatorPkg/EmulatorPkg.dsc)</span>  </li>
  <ul style="list-style-type:none; line-height:0.75;">
     <li><span style="font-size:0.7em" ><font face="Consolas">[Components . . .]</font></span>  </li>
     <li><span style="font-size:0.7em" >&nbsp;&nbsp;Add INF to components section, before build options </span>  </li>
     <li><span style="font-size:0.7em" >&nbsp;&nbsp;Hint: add after comment&nbsp;</span><span style="font-size:0.6em" >"<font color="#00CC99">&num; Add new modules here</font>"</span>
	  <br><span style="font-size:0.7em" > &nbsp;&nbsp;<span style="background-color: #101010">&nbsp;<font face="Consolas">&nbsp;&nbsp;@size[.8em](SampleApp/SampleApp.inf)&nbsp;&nbsp;</font><br> <br></span> </span> </li>
 </ul>
 <li><span style="font-size:0.8em" >2.&nbsp;&nbsp; <font face="Consolas" color="yellow">.INF</font> file (SampleApp/SampleApp.inf) </span>  </li>
  <ul style="list-style-type:none; line-height:0.75;">
     <li><span style="font-size:0.7em" >Packages (all depend on MdePkg)</span>  </li>
     <li><span style="font-size:0.7em" ><span style="background-color: #101010">&nbsp;&nbsp;<font face="Consolas">@size[.8em]([Packages]&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MdePkg/MdePkg.dec)&nbsp;&nbsp;&nbsp;</font> </span> </span> </li>
     <li><span style="font-size:0.7em" ><span style="background-color: #101010">&nbsp;&nbsp;<font face="Consolas">@size[.8em]([LibraryClasses]&nbsp;&nbsp;UefiApplicationEntryPoint)&nbsp;&nbsp;</font></span><br> <br>&nbsp; </span><br> </li>
 </ul>
 <li><span style="font-size:0.8em" >3.&nbsp;&nbsp; <font face="Consolas" color="yellow">.C </font> file - Header references File (SampleApp/SampleApp.c) </span>  </li>
  <ul style="list-style-type:none; line-height:0.75;">
     <li><span style="font-size:0.7em" ><span style="background-color: #101010"><font face="Consolas">@size[.8em](&num;include &lt;Uefi.h&gt;)&nbsp;&nbsp;</font></span> </span> </li>
     <li><span style="font-size:0.7em" ><span style="background-color: #101010"><font face="Consolas">@size[.8em](&num;include &lt;Library/UefiApplicationEntryPoint.h&gt;)&nbsp;&nbsp;</font></span></span>  </li>
 </ul>
</ul>


Note:

- So what are our steps for adding that
- So first we need to add the MDE package to the INF file and you need to reference the file by the DEC file so under the [packages] section you Are going to add "MdePkg/MdePkg.dec" 
- Under the [LibraryClasses] section of the INF you’re going to add a reference to "UefiApplicationsEntryPoint" . And just as an interesting note is actually dependent on the "UefiBootServiecesTableLib".
- Next in the .C. file you are going to add some header references, the "Uefi.h" and "Library/UefiApplicationEntryPoint.h"  
- Then in the DSC file under the "[components]" section you’re going to add a reference to your new sample INF file.


---?image=/assets/images/slides/Slide20.JPG
@title[Lab 2: Lab cont. Solution ]
<p align="right"><span class="gold" ><b>Lab 2: Lab cont. Solution </b></span></p>


Note:

<pre>
EmulatorPkg/EmulatorPkg.dsc in the components section of the file towards the bottom
SampleApp/SampleApp.inf 


 SampleApp/SampleApp.inf

[Packages] 
 MdePkg/MdePkg.dec
[LibraryClasses] 
 UefiApplicationEntryPoint


SampleApp/SampleApp.c - near the top of the file
#include &lt;Uefi.h&gt; 
#include &lt;Library/UefiApplicationEntryPoint.h&gt;


</pre>




---
@title[Lab 2: Will it compile now? ]
<p align="right"><span class="gold" ><b>Lab 2 : Will it compile now?</b></span></p>
<p style="line-height:90%"><span style="font-size:0.8em" >Yes, At the VS Command Prompt</span></p>

```shell
  C:/FW/edk2-ws/edk2> Build -D ADD_SHELL_STRING
  C:/FW/edk2-ws/edk2> RunEmulator.bat
```
<span style="font-size:0.8em" >Run the application from the shell</span>
```shell
 Shell> SampleApp
```
<p style="line-height:70%"><span style="font-size:0.8em" >Notice that the program will immediately unload because the main function is empty</span></p>

<span style="font-size:0.8em" >Exit</span>
```shell
 Shell> Reset
```

Note:



- And the answer is yes. 
- It will compile and it will even run at this point but we haven’t really added any functionality to this sample code at this point and so since the main function is empty it will unload as soon as it is called.
- So to test it after it has build successfully you then type build run in the EDK2 directory and to run your application type in the base name that you gave it in your INF file, type that name at the shell and it will run, but it won’t do anything because there is nothing for it to do.


- another note:   The program will immediately unload because the main function is empty
 



---?image=/assets/images/slides/Slide22.JPG
@title[Possible Build Errors ]
<p align="right"><span class="gold" ><b>Possible Build Errors</b></span></p>
<p style="line-height:60%" align="left" ><span style="font-size:0.7em;" >
<br>
Error on SampleApp.inf

<br><br><br>&nbsp;
<br><br><br>&nbsp;
<br><br><br>
<br><br><br>
<br><br><br>
The FILE_GUID was invalid or not updated from "XXX…" to a proper formatted GUID

</span></p>

Note:
The <font face="Consolas">FILE_GUID</font> was invalid or not updated from "<font face="Consolas">XXX…</font>" to a proper formatted GUID
- left is no FILE_GUID
- right - left the "XXXX" 




---?image=/assets/images/slides/Slide23.JPG
@title[Possible Build Errors 02 ]
<p align="right"><span class="gold" ><b>Possible Build Errors</b></span></p>

<p style="line-height:60%" align="left" ><span style="font-size:0.7em;" >
<br>
Error on SampleApp.inf

<br><br><br>&nbsp;
<br><br><br>&nbsp;
<br><br><br>&nbsp;
<br><br><br>
<br><br><br><br>
The <font face="Consolas">[Packages]</font> was invalid  or did not specify MdePkg/MdePkg.dec properly

</span></p>

Note:
The `[Packages]` was invalid  or did not specify MdePkg/MdePkg.dec properly



---?image=/assets/images/slides/Slide24.JPG
@title[Possible Build Errors 03]
<p align="right"><span class="gold" ><b>Possible Build Errors</b></span></p>
<p style="line-height:60%" align="left" ><span style="font-size:0.7em;" >
<br>
Compiler Error on SampleApp.c

<br><br><br>&nbsp;
<br><br><br>&nbsp;
<br><br><br>&nbsp;
<br><br><br>
<br><br><br>
The <font face="Consolas">#include &lt;Library/UefiApplicationEntryPoint.h&gt;</font>  has a typo ("Application" not "Applications")

</span></p>

Note:
- The `#include <Library/UefiApplicationEntryPoint.h>`  has a typo ("Application" not "Applications")



---?image=/assets/images/slides/Slide25.JPG
@title[Possible Build Errors 04 ]
<p align="right"><span class="gold" ><b>Possible Build Errors</b></span></p>
<p style="line-height:60%" align="left" ><span style="font-size:0.7em;" >
<br>
Compile Linker Error on unresolved reference


<br><br><br>&nbsp;
<br><br><br>&nbsp;
<br><br><br>&nbsp;
<br><br><br>
<br><br><br>
The SampleApp.inf section <font face="Consolas">[LibraryClasses]</font> did not reference <font face="Consolas">UefiApplicationEntryPoint</font>

</span></p>


Note:
The SampleApp.inf section `[LibraryClasses]` did not reference `UefiApplicationEntryPoint`



---?image=/assets/images/slides/Slide26.JPG
@title[Possible Build Errors 05]
<p align="right"><span class="gold" ><b>Possible Build Errors</b></span></p>
<p style="line-height:60%" align="left" ><span style="font-size:0.7em;" >
<br>
Error at the Shell prompt


<br><br><br>&nbsp;
<br><br><br>&nbsp;
<br><br><br>&nbsp;
<br><br><br>
<br><br><br>
Ensure the SampleApp.inf BaseName is SampleApp 
</span></p>

Note:
Ensure the SampleApp.inf BaseName is SampleApp 

---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 2.1: Build Switches]
<br>
<br>
<p align="Left"><span class="gold" >Lab 2.1: Build Switches</span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab, you’ll change the build switch <font face="Consolas">ADD_SHELL_STRING</font> to be always TRUE</span>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:

---?image=/assets/images/slides/Slide28.JPG
@title[Lab 2.1: Build MACRO Switches  ]
<p align="right"><span class="gold" ><b>Build MACRO Switches </b></span></p>

<p style="line-height:60%" align="left" ><span style="font-size:0.7em;" ><br>
The build for EmulatorPkg is using build MACRO Switch:<br>
@color[yellow](-D &nbsp;ADD_SHELL_STRING) – used to change a string in the UEFI Shell application, only used for EDK II Training (requires ShellPkg be re-built on a change of this switch)
</span></p>
<p style="line-height:40%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" ><font color="black"><br><br><br>
&nbsp; &num; For UEFI / EDK II Training <br>
&nbsp; &num; This flag is to enable a different ver string for building of the ShellPkg<br>
&nbsp; &num; These can be changed on the command line.<br>
&nbsp;   DEFINE ADD_SHELL_STRING      = FALSE 
</font></span></p>

<p style="line-height:60%" align="left" ><span style="font-size:0.7em;" ><br><br>
First delete directory   Build/EmulatorX64/DEBUG_tag/X64/ShellPkg
<br><br><br>&nbsp;
</span></p>

Note:



---?image=/assets/images/slides/Slide29.JPG
@title[Lab 2.1: Compiling w/out Build Switch ]
<p align="right"><span class="gold" ><b>Lab 2.1: Compiling w/out Build Switch</b></span></p>
<span style="font-size:0.8em" >At the VS Command Prompt, Build <font color="yellow">without</font> the <b>"-D"</b> Switch</span>

@snap[north-west span-60]
<br>
<br>
<br>
<br>
<br>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" ><br>&nbsp;</span></p>)
<br>
<br>
<br>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" ><br>&nbsp;</span></p>)
@snapend

@snap[north-west span-58 ]
<br>
<br>
<br>
<p style="line-height:40%" align="left" ><span style="font-size:0.45em; font-family:Consolas;" ><br>
<font face="Arial"><b>Delete</b> </font>Build/EmulatorX64/DEBUG_@color[cyan](<i>tag</i>)/X64/@color[yellow](ShellPkg) <br><br>&nbsp;&nbsp;
  C:/FW/edk2-ws/edk2> Build<br> &nbsp;&nbsp;
  C:/FW/edk2-ws/edk2> RunEmulator.bat<br><br>
</span></p>
<p style="line-height:70%" align="left" ><span style="font-size:0.7em;" >
Check the Shell version with "Ver" command <br>
Build with  the <font face="Consolas">–D ADD_SHELL_STRING</font>
</span></p>

<p style="line-height:40%" align="left" ><span style="font-size:0.45em; font-family:Consolas;" >
<font face="Arial"><b>Delete</b> </font>Build/EmulatorX64/DEBUG_@color[cyan](<i>tag</i>)/X64/@color[yellow](ShellPkg)<br><br>&nbsp;&nbsp;
  C:/FW/edk2-ws/edk2> Build -D Add_SHELL_STRING<br> &nbsp;&nbsp;
  C:/FW/edk2-ws/edk2> RunEmulator.bat<br>
</span></p>

<p style="line-height:70%" align="left" ><span style="font-size:0.7em;" >
Check the Shell version with "Ver" command <br>
</span></p>
@snapend

@snap[south-west span-100 ]
<p style="line-height:50%" align="left" ><span style="font-size:0.57em;" >
@color[yellow](NOTE:) You may need to Delete directory:   <font face="Consolas">Build/EmulatorX64/DEBUG_@color[cyan](<i>tag</i>)/X64/@color[yellow](ShellPkg) </font>
Between each build
</span></p>
@snapend


Note:


---?image=/assets/images/slides/Slide30.JPG
@title[Lab 2.1: Compiling w/out Build Switch 02]
<p align="right"><span class="gold" ><b>Lab 2.1: Compiling w/out Build Switch</b></span></p>
<p style="line-height:70%"><span style="font-size:0.7em" ><b>Edit</b> the file <font face="Consolas">C:/FW/edk2-ws/edk2/EmulatorPkg/EmulatorPkg.dsc</font></span></p>

@snap[north-west span-52]
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" ><br>&nbsp;</span></p>)
@snapend

@snap[north-west span-60]
<br>
<br>
<br>
<p style="line-height:70%"><span style="font-size:0.7em" >
<b>Change</b><br>
<font face="Consolas">DEFINE ADD_SHELL_STRING = @color[yellow](TRUE)</font> <br>
@size[.8em](&lpar;Save the file&rpar;)<br>
@size[.8em](Delete directory:&nbsp;&nbsp; <font face="Consolas">Build/ . . ./ Shellpkg)</font> <br>

Re-Build – Cd to <font face="Consolas">/FW/edk2-ws/edk2<br>&nbsp;&nbsp;

   @size[.7em](C:/FW/edk2-ws/edk2> Build)<br>&nbsp;&nbsp;
   @size[.7em](C:/FW/edk2-ws/edk2> RunEmulator.bat)<br><br>
</font>
<br><br><br>
Check the Shell version with "<font face="Consolas">Ver</font>"  command 
</span></p>
@snapend


@snap[south-west span-100 ]
<p style="line-height:45%" align="left" ><span style="font-size:0.5em;" >
@color[yellow](NOTE:) You may need to Delete directory:   <font face="Consolas">%WORKSPACE%/Build/EmulatorX64/DEBUG_@color[cyan](<i>tag</i>)/X64/@color[yellow](ShellPkg) </font>
Between each build
</span></p>
@snapend

Note:

---
@title[Lab 2: What we learned from LAB 2]
<p align="right"><span class="gold" ><b>What we learned from LAB 2</b></span></p>
<ol style="line-height: 60%">
 <li><span style="font-size:0.7em">How to write a simple native UEFI Application </span></li>
 <li><span style="font-size:0.7em">Each module requires a .inf file with a unique GUID (use http://www.guidgenerator.com/ )  </span></li>
 <li><span style="font-size:0.7em">The module created will be the base name defined in the .inf file </span></li>
 <li><span style="font-size:0.7em">The module’s .inf  file is required to be included in the platform .dsc file </span></li>
 <li><span style="font-size:0.7em">The [Packages] section is required at minimum to include MdePkg/dePkg.dec </span></li>
 <li><span style="font-size:0.7em">When using a Build Switch (-D) on the command line it overrides the value in the .DSC file </span></li>
</ol>

Note:

---
@title[Lab 2: If there are Build Errors ]
<p align="right"><span class="gold" ><b>Lab 2: If there are build errors …</b></span></p>
<br>
<span style="font-size:0.9em" >See class files for the solution </span>
<ul>
  <li><span style="font-size:0.8em" >. . .FW/LabSampleCode/LabSolutions/LessonB.2 </span>  </li>
  <li><span style="font-size:0.8em" >Copy the .inf and .c files to  C:/FW/edk2-ws/edk2/SampleApp </span>  </li>
  <li><span style="font-size:0.8em" >Search sample DSC for reference to SampleApp.inf and add this line to your workspace DSC file<br>&nbsp;&nbsp;&nbsp;&nbsp; <font face="Consolas">C:/FW/edk2-ws/edk2/EmulatorPkg/EmulatorPkg.dsc</font> </span>  </li>
</ul>
<p style="line-height:45%" align="left" ><span style="font-size:0.7em; font-family:Consolas;" >
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="background-color: #101010">&nbsp;&nbsp;@size[.8em](SampleApp/SampleApp.inf)&nbsp;&nbsp;</span>
<br><br>&nbsp;
</span></p>


<span style="font-size:0.8em" >Invoke <b>"<font face="Consolas">build</font>"</b> again and check the solution </span>

Note:

same as slide



---?image=assets/images/binary-strings-black2.jpg
@title[Add more Functionality Section]
<br><br><br>
### <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Add Functionality </span>
<span style="font-size:0.9em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Add Functionality to the Simple UEFI Application : </span>
<span style="font-size:0.9em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Next 3 Labs</span>
<ul style="list-style-type:none">
 <li><span style="font-size:0.8em" >&nbsp;&nbsp;&nbsp;&nbsp;<font color="cyan">Lab 3:&nbsp;&nbsp;</font>Print the UEFI System Table </span>  </li>
 <li><span style="font-size:0.8em" >&nbsp;&nbsp;&nbsp;&nbsp;<font color="cyan">Lab 4:&nbsp;&nbsp;</font>Wait for an Event </span>  </li>
 <li><span style="font-size:0.8em" >&nbsp;&nbsp;&nbsp;&nbsp;<font color="cyan">Lab 5:&nbsp;&nbsp;</font>Create a Simple Typewriter function </span>  </li>
</ul>
<br>
<br>
<span style="font-size:0.7em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#a0a0a0">Solutions in .../FW/LabSampleCode/LabSolutions/LessonB.<i>n</i></font></span>

---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 3: Print the UEFI System Table]
<br>
<br>
<p align="Left"><span class="gold" >Lab 3: Print the UEFI System Table</span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >Add code to print the hex address of the EFI System Table pointer to the console.</span>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:



---?image=/assets/images/slides/Slide35.JPG
@title[Lab 3 : Add System Table Code]
<p align="right"><span class="gold" ><b>Lab 3 : Add System Table Code</b></span></p>
<p style="line-height:60%" align="left" ><span style="font-size:0.7em;" >
Add code to print to the console the hex address of the system table pointer
</span></p>
<ul style="list-style-type:disc; line-height:0.7;">
  <li><span style="font-size:0.7em;" >  Where is the "print" function? </span></li>
  <li><span style="font-size:0.7em;" >  Where does the app get the pointer value? @size[.8em](&lpar;compared to mem command below&rpar;) </span></li>

</ul>

Note:

- So let’s extend this and give it something useful to do
 - so for this example we are going to have our sample application print out the system table pointer
- So how do we do that. Well remember to find a function we want we can use the help documentation or CHM file.so what we will find if we do this is that the print function is part of the UefiLib. So in order to add the print functionality we would need to add the UefiLib  to our list of library classes in our INF file
- To see this example look in the files in our sample lab code SampleApp.c and SampleApp.inf
- So also as an exercise you can look at the file in the sample lab code Min.dsc, this is a platform description file without a platform or any packages that go with it,  and this demonstrates the minimal contents for a DSC file that can build this application. 
So it will build a single application orientated toward the one we just created except nothing else. So unlike the EmulatorPkg platform description file, if you were to look at it, There are huge amounts of other components, library classes, and all of that, this Min.dsc only does the minimum requirements.


---?image=/assets/images/slides/Slide36.JPG
@title[Locating the "Print" Function ]
<p align="right"><span class="gold" ><b>Lab 3 : Locating the <font face="Consolas">Print()</font> Function </b></span></p>
<br>
<ul style="list-style-type:none; line-height:0.7;">
  <li><span style="font-size:0.7em;" >  1. Search the <font face="Consolas">MdePkg.chm</font> and find that the Print function by clicking<br>&nbsp;&nbsp;&nbsp;&nbsp; on the "<u>I</u>ndex" tab</span></li>
  <li><span style="font-size:0.7em;" >  2. Type "Print" and double click</span></li>
  <li><span style="font-size:0.7em;" >  3. Scroll to the top in the right window to see that the print function is<br>&nbsp;&nbsp;&nbsp;&nbsp; in the <font face="Consolas">UefiLib.h</font> file</span></li>
</ul>

Note:

- "MdePkg Document With Libraries.chm" located in ... Lab_Material_FW/FW/Documentation
1. Search the MdePkg.chm and find that the Print function by clicking on the "Index" tab
2. Type "Print" and double click
3. Scroll to the top in the right window to see that the print function is in the UefiLib.h file
- * NOTE -:  Install a CHM Viewer for Ubuntu
- bash$ sudo aptitude install kchmviewer


---?image=/assets/images/slides/Slide37.JPG
@title[Modifying .C & .INF Files ]
<p align="right"><span class="gold" ><b>Lab 3 : Modifying .C & .INF Files</b></span></p>

@snap[north-west span-60]
<br>
<br>
<br>
<p style="line-height:40%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" ><font color="black"><br>
 <br><br>
&num;include &lt;Uefi.h&gt; <br>
&num;include &lt;Library/UefiApplicationEntryPoint.h&gt; <br>
@color[red](&num;include &lt;Library/UefiLib.h&gt;) <br>
 <br>
EFI_STATUS <br>
EFIAPI <br>
UefiMain ( <br>&nbsp;&nbsp;
  IN EFI_HANDLE        ImageHandle, <br>&nbsp;&nbsp;
  IN EFI_SYSTEM_TABLE  *SystemTable <br>&nbsp;&nbsp;
  ) <br>
{ <br>&nbsp;&nbsp;
  @color[red](Print&lpar;L"System Table: 0x%p\n", SystemTable&rpar;; ) <br>&nbsp;&nbsp;
  return EFI_SUCCESS; <br>
} <br>
</font>
</span></p>

@snapend


@snap[north-east span-40]
<br>
<br>
<br>
<p style="line-height:40%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" ><font color="black"><br>
 <br><br>
 [LibraryClasses] <br>&nbsp;&nbsp;
  UefiApplicationEntryPoint <br>&nbsp;&nbsp;
  @color[red](UefiLib)
</font>
</span></p>
@snapend

@snap[south span-60]
<p style="line-height:60%" align="left" ><span style="font-size:0.47em;" >Note: 
Solution files are in the lab materials directory
</span></p>
@snapend


Note:

- SampleApp.c
<pre>
  #include <Uefi.h>
  #include <Library/UefiApplicationEntryPoint.h>
  #include <Library/UefiLib.h>

EFI_STATUS
EFIAPI
UefiMain (
  IN EFI_HANDLE        ImageHandle,
  IN EFI_SYSTEM_TABLE  *SystemTable
  )
{
  Print(L"System Table: 0x%08x\n", SystemTable); 
  return EFI_SUCCESS;
}

- SampleApp.inf
 [LibraryClasses]
  UefiApplicationEntryPoint
  
  UefiLib

</pre>
 




---
@title[Build and Test SampleApp]
<p align="right"><span class="gold" ><b>Lab 3 : Build and Test SampleApp</b></span></p>
<span style="font-size:0.8em" >At the VS Command Prompt </span>


```shell
  C:/FW/edk2-ws/edk2> Build
  C:/FW/edk2-ws/edk2> RunEmulator.bat
```

<span style="font-size:0.8em" >Run the application from the shell</span>

```shell
 Shell> SampleApp
 System Table: 0x07E34018
 Shell> 
```

<p style="line-height:70%"><span style="font-size:0.8em" >Verify by using the Shell "<font face="Consolas">mem</font>" command </span></p>
<span style="font-size:0.8em" >Exit</span>

```shell
 Shell> Reset
```


Note:

Same as slide

End of LAB 3

---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 4: Waiting for an Event]
<br>
<br>
<p align="Left"><span class="gold" >Lab 4: Waiting for an Event</span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab, you’ll learn how to locate code and .chm files to help write EFI code for waiting for an event</span>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:


---?image=/assets/images/slides/Slide40.JPG
@title[Lab 4 : Add Wait for Event ]
<p align="right"><span class="gold" ><b>Lab 4 : Add Wait for Event</b></span></p>
<br>
<p style="line-height:80%"><span style="font-size:0.8em" >Add code to make your application wait for a key press event (<font face="Consolas">WaitForEvent / WaitForKey</font>)</span></p>
<br>
<br>
<br>
<br>
<ul>
  <li><span style="font-size:0.8em" >Where are these functions located?</span> </li>
  <li><span style="font-size:0.8em" >What else can you do with the key press? </span> </li>
</ul>  


Note:

- Add code to make your application wait for an event (WaitForEvent) and use the (WaitForKey) as the event

- Hint: use the MdePkg.chm to find where the "WaitForEvent" and the "WaitForKey" functions are located
- Another Hint: The system table is passed in as a parameter to your sample application
- Search the EDK II code for "WaitForEvent" 
- Test by running your application in the Shell

---
@title[Lab 4 : How to locate functions ]
<p align="right"><span class="gold" ><b>Lab 4 : HOW?</b></span></p>
<span style="font-size:0.9em" >Locate Functions:  </span><span style="font-size:0.7em" > <font face="Consolas"> WaitForEvent / WaitForKey</font></span>
<ul style="line-height:0.8;">
  <li><span style="font-size:0.8em" >Search MdePkg.chm</span><span style="font-size:0.5em" > - Note: "MdePkg Document With Libraries.chm" located in ... Lab_Material_FW/FW/Documentation</span></li>
  <ul style="list-style-type:disc">
    <li><span style="font-size:0.65em" >Locate <font face="Consolas">WaitForEvent</font> in Boot Services</span> </li>
    <li><span style="font-size:0.65em" >Locate <font face="Consolas">WaitForKey</font> and find ( <font face="Consolas">EFI_SIMPLE_TEXT_INPUT_PROTOCOL</font> will be part of <font face="Consolas">ConIn</font> ) </span> </li><br>
   </ul>
  <li><span style="font-size:0.8em" >Check the <a href="http://uefi.org">UEFI Spec</a> for parameters needed:</span> </li>
   <ul style="list-style-type:disc">
	<li><span style="font-size:0.65em" ><font face="Consolas">WaitForEvent</font> is referenced via Boot Services pointer, which is referenced via EFI System Table </span> </li>
	<li><span style="font-size:0.65em" ><font face="Consolas">WaitForKey</font>	 can be referenced through the EFI System Table passed into the application</span> </li>
    </ul>
  <li><span style="font-size:0.8em" ><font color="yellow"><b>OR</b></font><br> Search the working space for <font face="Consolas">WaitForEvent</font> for an example</span> </li>
    <ul style="list-style-type:disc">
	<li><span style="font-size:0.65em" >One can be found in <a href="https://github.com/tianocore/edk2/blob/master/MdePkg/Library/UefiLib/Console.c">MdePkg/Library/UefiLib/Console.c</a>  ~ ln 569: </span> </li>
    </ul>
   
</ul> 


Note:

1. Search the MdePkg.chm and find where the "WaitForEvent" is located.  It is part of the "Boot Services".
2. Search the MdePkg.chm and find where the "WaitForKey" is located.  It is part of the "EFI_SIMPLE_TEXT_INPUT_PROTOCOL as part of ConIn.
3. The WaitForEvent can be referenced through the Boot Services pointer which can be referenced through the System Table
4. The WaitForKey can be referenced through the System Table passed into our Sample application
5. Check the UEFI Spec for the parameters needed
6. An example can be found in MdePkg\Library\UefiLib\Console.c :
  - gBS->WaitForEvent (1, &gST->ConIn->WaitForKey, &EventIndex);



---?image=/assets/images/slides/Slide42.JPG
@title[Lab 4 :Update the C File for WaitForKey ]
<p align="right"><span class="gold" ><b>Lab 4 : Update the C File for <font face="Consolas">WaitForKey</font></b></span></p>
<p style="line-height:55%" align="left" ><span style="font-size:0.6em;" >
Search the work space and find the following <font face="Consolas">@size[.8em](MdePkg/Library/UefiLib/Console.c )</font> ~ ln 563:
</span></p>

<p style="line-height:40%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" ><font color="black"><br><br>
   &nbsp;&nbsp;UINTN  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; EventIndex;<br>
  @color[green](. . . )<br>
 <br>
	@color[blue](// If we encounter error, continue to read another key in.) <br>
    @color[blue](// ) <br>&nbsp;&nbsp;
       if (Status != EFI_NOT_READY) { <br>&nbsp;&nbsp;&nbsp;&nbsp;
        continue; <br>&nbsp;&nbsp;
      } <br>&nbsp;&nbsp;
      gBS-&gt;WaitForEvent (1, &gST-&gt;ConIn-&gt;WaitForKey, &EventIndex); <br>
    } <br>
  @color[green](. . .)	 <br>
<br>
<font face="Arial">
@color[white](@size[1.2em]( Add the following to SampleApp.c))
</font>
<br>
 <br>
 <br><br>
 @color[red](UINTN  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; EventIndex; ) <br>
 Print(L"System Table: 0x%p\n",SystemTable);  <br>
 @color[red](Print&lpar;L"\nPress any Key to continue : \n"&rpar;;) <br>
 @color[red](gBS-&gt;WaitForEvent &lpar;1, &gST-&gt;ConIn-&gt;WaitForKey, &EventIndex&rpar;; )<br>

<br><br><br>&nbsp;
</font>
</span></p>



Note:


Add the following "Lab 4" statements to SampleApp.c

 
---?image=/assets/images/slides/Slide43.JPG
@title[Lab 4 :Test Compile ]
<p align="right"><span class="gold" ><b>Lab 4 :Test Compile</b></span></p>
<p style="line-height:65%" align="left" ><span style="font-size:0.7em;" >
However, this won’t compile . . . <font face="Consolas">gBS and gST </font> are not defined.
</span></p>

@snap[south-west span-100]
<p style="line-height:40%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" >
<font face="Arial">
<b>Search</b> the </font>MdePkg.chm for "gBS" and "gST"<font face="Arial"> – they are located in</font> UefiBootServicesTableLib.h<br>
<br>
<font face="Arial">
<b>Add</b> the boot services lib to</font> SampleApp.c . . .<br>
@color[yellow](&num;include &lt;Library/UefiBootServicesTableLib.h&gt;) <br><br>
<font face="Arial">@size[.8em](&lpar;hint: Lesson B4 has the solution&rpar;)</font>
</span></p>
<br>
@snapend


Note:
- However, this won’t compile … gBS and gST are not defined.

- Search the MdePkg.chm for "gBS" and "gST" – they are located in UefiBootServicesTableLib.h

  - Add the boot services lib to SampleApp.c …
  - #include <Library/UefiBootServicesTableLib.h>

-  (hint: Lesson B.4 has the solution)

 
---?image=/assets/images/slides/Slide44.JPG
@title[Lab 4 : Update SampleApp.c for gBS & gST ]
<p align="right"><span class="gold" ><b>Lab 4 : Update for <font face="Consolas">gBS & gST</font></b></span></p>

@snap[north-west span-100]
<br><br>
<p style="line-height:40%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" ><font color="black"><br><br><br>
&num;include &lt;Uefi.h&gt; <br>
&num;include &lt;Library/UefiApplicationEntryPoint.h&gt; <br>
&num;include &lt;Library/UefiLib.h&gt; <br>
@color[red](&num;include &lt;Library/UefiBootServicesTableLib.h&gt;) &nbsp;&nbsp;&nbsp; <br>
@color[blue](// . . . )<br>
EFI_STATUS <br>
EFIAPI <br>
UefiMain ( <br>&nbsp;&nbsp;
  IN EFI_HANDLE        ImageHandle, <br>&nbsp;&nbsp;
  IN EFI_SYSTEM_TABLE  *SystemTable <br>&nbsp;&nbsp;
  ) <br>
{  <br>&nbsp;&nbsp;
  @color[red](UINTN                EventIndex;)<br>&nbsp;&nbsp;
  Print(L"System Table: 0x%p\n", SystemTable);  <br>&nbsp;&nbsp;
  @color[red](Print&lpar;L"\nPress any Key to  continue :\n"&rpar;; )<br>&nbsp;&nbsp;
  @color[red](gBS-&gt;WaitForEvent &lpar;1, &amp;gST-&gt;ConIn-&gt;WaitForKey, &amp;EventIndex&rpar;; )<br>&nbsp;&nbsp;
  return EFI_SUCCESS;  <br>
} <br>
<br><br><br>&nbsp;
</font>
</span></p>
@snapend


@snap[north-east span-22]
<br><br>
<p style="line-height:40%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" ><font color="black"><br><br><br>
 <br>
 <br>
<br>
@color[#7030a0](@size[1.4em](&larr;) Lab 4) <br>
<br>
<br>
 <br>
<br>&nbsp;&nbsp;
  <br>&nbsp;&nbsp;
  <br>&nbsp;&nbsp;
   <br>
 <br>
  @color[#7030a0](@size[1.4em](&larr;) Lab 4)<br>&nbsp;&nbsp;
<br>
  @color[#7030a0](@size[1.4em](&larr;) Lab 4)<br>
  @color[#7030a0](@size[1.4em](&larr;) Lab 4)<br>&nbsp;&nbsp;
  <br>
 <br>
<br><br><br>&nbsp;
</font>
</span></p>
@snapend



Note:

- add:
- `#include <Library/UefiBootServicesTableLib.h>`


SampleApp.c Should have the following for Lab 4: 

```C++
#include <Uefi.h>
#include <Library/UefiApplicationEntryPoint.h>
#include <Library/UefiLib.h>
// Lab 4
#include <Library/UefiBootServicesTableLib.h>

EFI_STATUS
EFIAPI
UefiMain (
  IN EFI_HANDLE        ImageHandle,
  IN EFI_SYSTEM_TABLE  *SystemTable
  )
{
// Lab 4
  UINTN          EventIndex;
  
// Lab 3
 Print(L"System Table: 0x%08x\n",SystemTable); 

//Lab 4
 Print( L"\nPress any Key to continue : \n\n");
 gBS->WaitForEvent (1, &gST->ConIn->WaitForKey,    	&EventIndex);
 return EFI_SUCCESS;
}

// End of lab
```



---
@title[Lab 4 Build and Test SampleApp]
<p align="right"><span class="gold" ><b>Lab 4: Build and Test SampleApp</b></span></p>
<span style="font-size:0.8em" >At the VS Command Prompt </span>
```shell
  C:/FW/edk2-ws/edk2> Build
  C:/FW/edk2-ws/edk2> RunEmulator.bat
```
<span style="font-size:0.8em" >Run the application from the shell</span>
```shell
 Shell> SampleApp
 System Table: 0x07E34018
 Press any key to continue:
 Shell> 
```
<span style="font-size:0.8em" >Notice that the SampleApp will wait until a key press to continue.</span><br>

<span style="font-size:0.8em" >Exit</span>
```shell
 Shell> Reset
```


Note:

Same as slide



---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 5: Creating a Simple Typewriter Function]
<br>
<br>
<p align="Left"><span class="gold" >Lab 5: Creating a Simple Typewriter Function</span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab, you'll learn how to create a simple typewriter function that retrieves the keys you type and subsequently prints each one back to the console</span>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:

 
---?image=/assets/images/slides/Slide47.JPG
@title[Lab 5 :Create a Simple Typewriter Function]
<p align="right"><span class="gold" ><b>Lab 5 : Typewriter Function</b></span></p>
<br>
<p style="line-height:80%"><span style="font-size:0.9em" >Create a Simple Typewriter Function using the SampleApp from Lab 4 </p></span>
<span style="font-size:0.9em" ><font color="cyan"><b>Requirements:</b></font></span>
<div class="left1">
<ul style="line-height:0.8;">
  <li><span style="font-size:0.8em" >Retrieve keys entered from keyboard (<i>Like</i> Lab 4)</span>  </li>
  <li><span style="font-size:0.8em" >Print back each key entered to the console</span>  </li>
  <li><span style="font-size:0.8em" >To Exit, press  "." (DOT) and  then &lt;Enter&gt; </span>  </li>
</ul>
</div>
<div class="right1">
<span style="font-size:01.0em" ><font color="cyan"></font></span>
</div>



Note:
Same as Slide



 
---?image=/assets/images/slides/Slide47.JPG
@title[Lab 5 :Create a Simple Typewriter Function How]
<p align="right"><span class="gold" ><b>Lab 5 : Typewriter Function</b></span></p>
<br>
<p style="line-height:80%"><span style="font-size:0.9em" >Create a Simple Typewriter Function using the SampleApp from Lab 4 </p></span>
<span style="font-size:01.0em" ><font color="cyan"><b>How:</b></font></span>
<div class="left1">
 <ol style="line-height: 60%">
  <li><span style="font-size:0.7em" >Add a Loop using <font face="Consolas">WaitForEvent with WaitForKey</font></span> </li>
  <li><span style="font-size:0.7em" >Use the <font face="Consolas">ReadKeyStroke</font> function from <font face="Consolas">ConIn</font></span>  </li>
  <li><span style="font-size:0.7em" >Print back each key to console</span> </li>
  <li><span style="font-size:0.7em" >Exit the loop when DOT "." character followed by an &lt;<font face="Consolas">Enter</font>&gt; key  </span>  </li>
</ol>
</div>
<div class="right1">
<span style="font-size:01.0em" ><font color="cyan"></font></span>
</div>



Note:
Same as Slide


 
---
@title[Lab 5 : How Hints]
<p align="right"><span class="gold" ><b>Lab 5 : How Process (Hints)</b></span></p>
<ul style="line-height:0.8;">
  <li><span style="font-size:0.8em" >Use the same procedure as with Lab 4 to find "<font face="Consolas">ReadKeyStroke</font>" in the work space: 	<a href="https://github.com/tianocore/edk2/blob/master/MdePkg/Library/UefiLib/Console.c">  MdePkg/Library/UefiLib/Console.c</a>  ~ ln 558</span>  </li>
  <ul style="list-style-type:none">
   <li><span style="font-size:0.6em" ><font color="white"><span style="background-color: #101010"><font face="Consolas">Status = gST-&gt;ConIn-&gt;ReadKeyStroke (gST-&gt;ConIn, Key);</font></span></font></span></li><br>
  </ul>
  <li><span style="font-size:0.8em" ><font face="Consolas">ReadKeyStroke</font> uses buffer called <font face="Consolas">EFI_INPUT_KEY</font>&nbsp;&nbsp; ~ ln 399</span>  </li>
  <ul style="list-style-type:none">
   <li><span style="font-size:0.6em" ><font color="white"><span style="background-color: #101010"><font face="Consolas">OUT EFI_INPUT_KEY  *Key,</font></span></font></span></li><br>
  </ul>
  <li><span style="font-size:0.8em" >TIP: Good Idea to zero out a buffer in your function  </span>  </li>
   <ul style="list-style-type:disc">
      <li><span style="font-size:0.7em" >Use MdePkg.chm to find <font face="Consolas">ZeroMem()</font> function</span>  </li>
      <li><span style="font-size:0.7em" >Use <font face="Consolas">ZeroMem()</font> on your variable buffer "<font face="Consolas">Key</font>" of type <font face="Consolas">EFI_INPUT_KEY</font></span>  </li><br>
   </ul> 
  <li><span style="font-size:0.8em" >Use Boolean flag "<font face="Consolas">ExitLoop</font>" to exit your loop once the user enters a DOT "." character.</span>  </li>
</ul>

Note:
same as slide


 
---?image=/assets/images/slides/Slide50.JPG
@title[Lab 5 :Typewriter Function Solution]
<p align="right"><span class="gold" ><b>Lab 5 : Solution</b></span></p>

@snap[north-west span-55 ]
<br>
<p style="line-height:28%" align="left" ><span style="font-size:0.38em; font-family:Consolas;" ><font color="black"><br>
&num;include &lt;Uefi.h&gt; <br>
&num;include &lt;Library/UefiApplicationEntryPoint.h&gt; <br>
&num;include &lt;Library/UefiLib.h&gt; <br>
&num;include &lt;Library/UefiBootServicesTableLib.h&gt; <br>
// Lab 5 <br>
&num;include &lt;Library/BaseMemoryLib.h&gt; <br>
&num;define CHAR_DOT  0x002E    // '.' in Unicode <br>
 <br>
EFI_STATUS <br>
EFIAPI <br>
UefiMain ( <br>&nbsp;&nbsp;
  IN EFI_HANDLE        ImageHandle, <br>&nbsp;&nbsp;
  IN EFI_SYSTEM_TABLE  *SystemTable <br>&nbsp;&nbsp;
  ) <br>
{ <br>&nbsp;&nbsp;
  UINTN &nbsp;&nbsp;&nbsp;&nbsp;         EventIndex; <br>
// Lab 5 <br>&nbsp;&nbsp;
  BOOLEAN        ExitLoop; <br>&nbsp;&nbsp;
  EFI_INPUT_KEY  Key; <br>
   <br>
// Lab 3 <br>&nbsp;&nbsp;
 Print(L"System Table: 0x%p \n",SystemTable);  <br>
 <br>
//Lab 4 <br>&nbsp;&nbsp;
 Print( L"\nPress any Key to continue : \n\n"); <br>&nbsp;&nbsp;
 gBS-&gt;WaitForEvent (1, &gST-&gt;ConIn-&gt;WaitForKey,<br>&nbsp;  &nbsp;&nbsp;  	&EventIndex); <br>
<br><br><br>&nbsp;
</font>
</span></p>
@snapend

@snap[north-east span-45 ]
<br>
<br>
<p style="line-height:31%"><span style="font-size:0.45em;" >(hint: Lesson B.5 has the solution)</span)</p>
<br>
<br>
<p style="line-height:26%" align="left" ><span style="font-size:0.35em; font-family:Consolas;" ><font color="black"><br>
<b>// Lab 5 </b> <br>&nbsp;&nbsp;
 Print(L"Enter text. Include a dot ('.') in a \<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sentence then <Enter> to exit:\n "); //  <br>&nbsp;&nbsp;
 ZeroMem (&Key, sizeof (EFI_INPUT_KEY));  <br>&nbsp;&nbsp;
 gST-&gt;ConIn->ReadKeyStroke (gST-&gt;ConIn, &Key);  <br>&nbsp;&nbsp;
 ExitLoop = FALSE;  <br>&nbsp;&nbsp;
 do {    // Do loop until "DOT" and "enter"   <br>&nbsp;&nbsp;&nbsp;&nbsp;
	 gBS-&gt;WaitForEvent (1, &gST-&gt;ConIn-&gt;WaitForKey, &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&EventIndex);  <br>&nbsp;&nbsp;&nbsp;&nbsp;
	 gST-&gt;ConIn->ReadKeyStroke (gST-&gt;ConIn, &Key);  <br>&nbsp;&nbsp;&nbsp;&nbsp;
	 Print(L"%c", Key.UnicodeChar);  <br>&nbsp;&nbsp;&nbsp;&nbsp;
	 if (Key.UnicodeChar == CHAR_DOT){  <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
		ExitLoop = TRUE;  <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
    	 }  <br>&nbsp;&nbsp;&nbsp;&nbsp;
    } while (!(Key.UnicodeChar == CHAR_LINEFEED || <br>&nbsp;&nbsp;&nbsp;&nbsp;
	    Key.UnicodeChar == CHAR_CARRIAGE_RETURN) ||   <br>&nbsp;&nbsp;&nbsp;&nbsp;
       !(ExitLoop) );  <br>&nbsp;&nbsp;
  <br>&nbsp;&nbsp;
 Print(L"\n");  <br>&nbsp;&nbsp;
 return EFI_SUCCESS;  <br>
}
</font>
</span></p>
@snapend

Note:

Copy and paste from the following sub slide

+++
@title[Lab 5 :Typewriter Function Solution]
<p align="right"><span class="gold" ><b>Lab 5 :  Solution</b></span></p>
<span style="font-size:0.8em" >SampleApp.c Should have the following for Lab 5: </span>

```C++
#include <Uefi.h>
#include <Library/UefiApplicationEntryPoint.h>
#include <Library/UefiLib.h>
#include <Library/UefiBootServicesTableLib.h>
// Lab 5
#include <Library/BaseMemoryLib.h>
#define CHAR_DOT  0x002E    // '.' in Unicode

EFI_STATUS
EFIAPI
UefiMain (
  IN EFI_HANDLE        ImageHandle,
  IN EFI_SYSTEM_TABLE  *SystemTable
  )
{
  UINTN          EventIndex;
// Lab 5
  BOOLEAN        ExitLoop;
  EFI_INPUT_KEY  Key;
  
// Lab 3
 Print(L"System Table: 0x%p \n",SystemTable); 

//Lab 4
 Print( L"\nPress any Key to continue : \n\n");
 gBS->WaitForEvent (1, &gST->ConIn->WaitForKey,    	&EventIndex);

// Lab 5
 Print(L"Enter text. Include a dot ('.') in a sentence then <Enter> to exit:\n "); //
 ZeroMem (&Key, sizeof (EFI_INPUT_KEY));
 gST->ConIn->ReadKeyStroke (gST->ConIn, &Key);
 ExitLoop = FALSE;
 do {    // Do loop until "DOT" and "enter" 
	 gBS->WaitForEvent (1, &gST->ConIn->WaitForKey,&EventIndex);
	 gST->ConIn->ReadKeyStroke (gST->ConIn, &Key);
	 Print(L"%c", Key.UnicodeChar);
	 if (Key.UnicodeChar == CHAR_DOT){
		ExitLoop = TRUE;
    	 }
    } while (!(Key.UnicodeChar == CHAR_LINEFEED  ||  
	    Key.UnicodeChar == CHAR_CARRIAGE_RETURN) || 
       !(ExitLoop) );

 Print(L"\n");
 return EFI_SUCCESS;
}

// End of lab
```

Note:

Same as slide


---?image=/assets/images/slides/Slide52.JPG
@title[Lab 5 :Build and Test SampleApp ]
<p align="right"><span class="gold" ><b>Lab 5 :Build and Test SampleApp</b></span></p>
<span style="font-size:0.8em" >At the VS Command Prompt</span>
```shell
  C:/FW/edk2-ws/edk2> Build
  C:/FW/edk2-ws/edk2> RunEmulator.bat
```

<span style="font-size:0.8em" >Run the application from the shell</span>
<br>
<br>
<br>
<br>
<br>
<br>

<span style="font-size:0.8em" >Exit</span>

```shell
 Shell> Reset
```


Note:

End of Lab 5


 
---
@title[Bonus Lab :Open Protocol example]
<p align="right"><span class="gold" ><b>Bonus Exercise: Open Protocol Example</b></span></p>
<span style="font-size:0.9em" >Write an Application using <font face="Consolas">argv, argc</font> parameters</span>
<ul style="line-height:0.7;">
 <li><span style="font-size:0.8em" >Captures command line parameters using Open Protocol</span></li>
 <li><span style="font-size:0.8em" >Will need to open </span><span style="font-size:0.5em" ><font face="Consolas">SHELL_INTERFACE_PROTOCOL</font></span></li>
 <li><span style="font-size:0.8em" >Note  : Requires ShellPkg</span></li>
</ul>
<br>
<span style="font-size:0.9em" > Build SampleApp</span>
```
  C:/FW/edk2-ws/edk2> Build
  C:/FW/edk2-ws/edk2> RunEmulator.bat
```
<span style="font-size:0.9em" >Run the application from the shell </span>
```
 Shell> SampleApp  test1 test2
```

<span style="font-size:0.8em" >(hint: <font face="Consolas">..FW/LabSampleCode/ShellAppSample</font> has the solution)</span>


Note:

- Write an Application using argv, argc parameters
  - Captures command line parameters using Open Protocol
  - Need to open SHELL_INTERFACE_PROTOCOL
  - Note  : Requires ShellPkg

- Build SampleApp – Cd to ~/src/edk2     	bash$ build

- Copy  SampleApp.efi  to hda-contents	`bash$ cp Build/OvmfX64/DEBUG_GCC5/X64/SampleApp.efi \`
										`~/run-ovmf/hda-contents`

- Test by Invoking Qemu 		`bash$ cd ~/run-ovmf`
 							 	`bash$ . RunQemu.sh`
- Run the application from the shell

- `Shell> SampleApp  test1 test2`

- (hint: ~FW/LabSampleCode/ShellAppSample has the solution)



---?image=assets/images/binary-strings-black2.jpg
@title[Write a EADK Application  Section]
<br><br><br><br><br><br><br>
### <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Using EADK </span>
<span style="font-size:0.9em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Using EADK with UEFI Application</span>



---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 6: Writing UEFI Applications with EADK]
<br>
<br>
<p align="Left"><span class="gold" >Lab 6:  Writing UEFI Applications with EADK</span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab, you’ll write an application with the same functionality as SampleApp.c using LibC from the EDK II Application Development Kit (EADK)</span>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:

 
---?image=/assets/images/slides/Slide56.JPG
@title[Lab 6: With EDK II EADK]
<p align="right"><span class="gold" ><b>Lab 6: With EDK II EADK</b></span></p>
<br>

<p style="line-height:60%" align="left" ><span style="font-size:0.7em;" >
Write the same application with the same functionality as SampleApp.c using the LibC from the EADK
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
What libraries are needed <br>
What differences are there using the LibC
</span></p>



<br>

Note:

- Write the same application with the same functionality as SampleApp.c using the LibC from the EADK
- What libraries are needed 
- What differences are there using the LibC

---?image=/assets/images/slides/Slide57.JPG
@title[Lab 6: EDK II using EADK]
<p align="right"><span class="gold" ><b>Lab 6: EDK II using EADK</b></span></p>
<p style="line-height:60%" align="left" ><span style="font-size:0.7em;" >
Start with the packages for EADK from edk2-libc<br>
</span></p>
<p style="line-height:45%" align="left" ><span style="font-size:0.57em; font-family:Consolas;" >
&bull; /edk2-libc - AppPkg	- <font face="Arial">has directory Applications</font><br>
&bull; /edk2-libc - StdLib - <font face="Arial">contains the LibC libraries</font><br>
<br>
&bull; <font face="Arial"><b>Copy</b> and <b>paste </b> directory</font> ../FW/LabSampleCode/SampleCApp to <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
       C:/FW/edk2-ws/edk2-libc/AppPkg/Applications/SampleCApp 

<br><br><br>&nbsp;
</span></p>

<br>

Note:
- Start with the packages for EADK 
- /edk2-libc  - AppPkg	- has directory Applications
- /edk2-libc -  StdLib 	- contains the LibC libraries


- Copy and paste directory ~/FW/LabSampleCode/SampleCApp to C:/FW/edk2-ws/edk2-libc/AppPkg/Applications/SampleCApp 

---?image=/assets/images/slides/Slide58.JPG
@title[Lab 6: EDK II using EADK 02]
<p align="right"><span class="gold" ><b>Lab 6: EDK II using EADK</b></span></p>
<p style="line-height:60%" align="left" ><span style="font-size:0.7em;" >
Check out <font face="Consolas">AppPkg/Applications/SampleCApp<br>
SampleCApp.c &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;       </font> and  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font face="Consolas">SampleCApp.inf</font>
</span></p>

@snap[north-west span-50 ]
<br>
<br>
<br>
<br>
<p style="line-height:35%" align="left" ><span style="font-size:0.42em; font-family:Consolas;" ><font color="black"><br><br>
&num;include &lt;stdio.h&gt; <br>
   // . . . <br>
   int <br>
   main ( <br>&nbsp;&nbsp;
     IN int Argc, <br>&nbsp;&nbsp;
     IN char **Argv <br>
   ) <br>
   { <br>&nbsp;&nbsp;
      return 0; <br>
   } <br>
</font>   
</span></p>
@snapend


@snap[north-east span-46 ]
<br>
<br>
<br>
<br>
<p style="line-height:35%" align="left" ><span style="font-size:0.42em; font-family:Consolas;" ><font color="black"><br><br>
[Defines] <br>&nbsp;&nbsp;
  INF_VERSION &nbsp;&nbsp;       = 1.25 <br>&nbsp;&nbsp;
  BASE_NAME  &nbsp;&nbsp;&nbsp;&nbsp;        = SampleCApp <br>&nbsp;&nbsp;
  FILE_GUID  &nbsp;&nbsp;&nbsp;&nbsp;        = 4ea9… <br>&nbsp;&nbsp;
  MODULE_TYPE &nbsp;&nbsp;       = UEFI_APPLICATION <br>&nbsp;&nbsp;
  VERSION_STRING     = 0.1 <br>&nbsp;&nbsp;
  ENTRY_POINT &nbsp;&nbsp;       = ShellCEntryLib <br>
 <br>
[Sources] <br>&nbsp;&nbsp;
  SampleCApp.c <br>
 <br>
[Packages] <br>&nbsp;&nbsp;
  StdLib/StdLib.dec <br>&nbsp;&nbsp;
  MdePkg/MdePkg.dec <br>&nbsp;&nbsp;
  ShellPkg/ShellPkg.dec <br>
<br>
[LibraryClasses] <br>&nbsp;&nbsp;
  LibC <br>&nbsp;&nbsp;
  LibStdio <br>

</font>   
</span></p>
@snapend

Note:

- EDK II using EADK
- Check out AppPkg/Applications/SampleCApp

<pre>
```
#include <stdio.h>
   // . . .
   int
   main (
     IN int Argc,
     IN char **Argv
   )
   {
      return 0;
   }

[Defines]
  INF_VERSION    = 1.25
  BASE_NAME      = SampleCApp
  FILE_GUID      = 54321…
  MODULE_TYPE    = UEFI_APPLICATION
  VERSION_STRING = 0.1
  ENTRY_POINT    = ShellCEntryLib

[Sources]
  SampleCApp.c

[Packages]
  StdLib/StdLib.dec
  MdePkg/MdePkg.dec
  ShellPkg/ShellPkg.dec

[LibraryClasses]
  LibC
  LibStdio
```
</pre>





---
@title[Lab 6 : Update AppPkg.dsc ]
<p align="right"><span class="gold" ><b>Lab 6 : Update AppPkg.dsc </b></span></p>
<p style="line-height:70%"><span style="font-size:0.8em" >Edit the AppPkg/AppPkg.dsc and add <font face="Consolas">SampleCApp.inf</font> at the end of the components section</span></p>
- <span style="font-size:0.6em" > (hint: search for "#### Sample Applications")</span>
- <span style="font-size:0.6em" ><font face="Consolas">AppPkg/Applications/SampleCApp/SampleCApp.inf</font> </span>
<br>

```php
[Components]

  #### Sample Applications.
  AppPkg/Applications/Hello/Hello.inf        # No LibC includes or functions.
  AppPkg/Applications/Main/Main.inf          # Simple invocation. No other LibC functions.
  AppPkg/Applications/Enquire/Enquire.inf    #
  AppPkg/Applications/ArithChk/ArithChk.inf  #
  
  AppPkg/Applications/SampleCApp/SampleCApp.inf #  LAB 6 
  
```
@[9](Add to the Components section)

Note:


---
@title[Lab 6 :Build and Test SampleCApp ]
<p align="right"><span class="gold" ><b>Lab 6 :Build and Test SampleCApp</b></span></p>
<span style="font-size:0.8em" >Build AppPkg at the VS Command prompt</span>
```shell
  C:/FW/edk2-ws/edk2> build -p AppPkg\AppPkg.dsc –m AppPkg\Applications\SampleCApp\SampleCApp.inf
```
<span style="font-size:0.8em" >Copy the built application to the EmulatorPkg runtime directory</span>
```shell
  C:/FW/edk2-ws/edk2> copy ..\Build\AppPkg\DEBUG_VS2015x86\X64\SampleCApp.efi  ..\Build\EmulatorX64\DEBUG_VS2015x86\X64
```
<span style="font-size:0.8em" >Run the Emulation</span>
```shell
  C:/FW/edk2-ws/edk2> RunEmulator.bat
```
<span style="font-size:0.8em" >Run the application SampleCapp from the shell</span>
```shell
 Shell> SampleCApp
```
<p style="line-height:60%" align="left" ><span style="font-size:0.7em;" >Notice that the program will immediately unload because the main function is empty</span></p>

Note:

---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 7: Adding Functionality to SampleCApp]
<br>
<br>
<p align="Left"><span class="gold" ><b>Lab 7:  Adding Functionality to SampleCApp</b></span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab, you’ll add functionality to SampleCApp the same as in Lab 5. This lab will use EADK libraries so the coding style is similar to standard C.</span>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:

 
---?image=/assets/images/slides/Slide62.JPG
<!-- .slide: data-transition="none" -->
@title[Lab 7: With EDK II EADK]
<p align="right"><span class="gold" ><b>Lab 7: Add the same functionally from Lab 5</b></span></p>
<br>

Note:

+++?image=/assets/images/slides/Slide63.JPG
<!-- .slide: data-background-transition="none" -->
<!-- .slide: data-transition="none" -->
@title[Lab 7: With EDK II EADK 02]
<p align="right"><span class="gold" ><b>Lab 7: Add the same functionally from Lab 5</b></span></p>

Note:


+++?image=/assets/images/slides/Slide64.JPG
<!-- .slide: data-background-transition="none" -->
<!-- .slide: data-transition="none" -->
@title[Lab 7: With EDK II EADK 03]
<p align="right"><span class="gold" ><b>Lab 7: Add the same functionally from Lab 5</b></span></p>

Note:


+++?image=/assets/images/slides/Slide65.JPG
<!-- .slide: data-background-transition="none" -->
<!-- .slide: data-transition="none" -->
@title[Lab 7: With EDK II EADK 04]
<p align="right"><span class="gold" ><b>Lab 7: Add the same functionally from Lab 5</b></span></p>

Note:

---
@title[Lab 7: With EDK II EADK solution]
<p align="right"><span class="gold" >@size[1.0em](<b>Lab 7: Solution  </b>)</span><br>
<span style="font-size:0.75em;" > SampleCApp.c and SampleCApp.inf &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></p>


@snap[north-west span-53 ]
<br>
<br>
<br>

@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.9em;" ><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>&nbsp;</span></p>)
@snapend

@snap[north-east span-46 ]
<br>
<br>
<br>

@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.9em;" ><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>&nbsp;</span></p>)
@snapend

@snap[north-east span-98 ]
<br>
<br>
 
<p style="line-height:32%" align="left" ><span style="font-size:0.42em; font-family:Consolas;" >
<font color="cyan">@size[1.3em]("C" file)</font>
<br>
&num;include &lt;stdio.h&gt; <br>
   // . . . <br>
   int <br>
   main ( <br>&nbsp;&nbsp;
     IN int Argc, <br>&nbsp;&nbsp;
     IN char **Argv <br>
   ) <br>
   { <br>&nbsp;&nbsp;
   char c; <br>
// Lab 3 <br>&nbsp;&nbsp;
   printf("System Table: %p \n", gST) ;  <br>
// Lab 4 <br>&nbsp;&nbsp;
   puts("Press any Key and then <Enter> to continue :  "); <br>&nbsp;&nbsp;
   c=(char)getchar(); <br>
  <br>
// Lab 5 <br>&nbsp;&nbsp;
   puts ("Enter text. Include a dot ('.') <br>&nbsp;&nbsp;&nbsp;&nbsp;in a sentence then <Enter> to exit:"); <br>&nbsp;&nbsp;
   do { <br>&nbsp;&nbsp;&nbsp;&nbsp;
      c=(char)getchar(); <br>&nbsp;&nbsp;
     } while (c != '.'); <br>&nbsp;&nbsp;
   puts ("\n"); <br>&nbsp;&nbsp;
   return 0; <br>
} <br>

</span></p>
@snapend


@snap[north-east span-44 ]
<br>
<br>
<p style="line-height:32%" align="left" ><span style="font-size:0.42em; font-family:Consolas;" >
<font color="yellow">@size[1.3em](.inf file)</font>
<br>
[Defines] <br>&nbsp;&nbsp;
  INF_VERSION &nbsp;&nbsp;       = 1.25 <br>&nbsp;&nbsp;
  BASE_NAME  &nbsp;&nbsp;&nbsp;&nbsp;        = SampleCApp <br>&nbsp;&nbsp;
  FILE_GUID  &nbsp;&nbsp;&nbsp;&nbsp;        = 4ea9… <br>&nbsp;&nbsp;
  MODULE_TYPE &nbsp;&nbsp;       = UEFI_APPLICATION <br>&nbsp;&nbsp;
  VERSION_STRING     = 0.1 <br>&nbsp;&nbsp;
  ENTRY_POINT &nbsp;&nbsp;       = ShellCEntryLib <br>
 <br>
[Sources] <br>&nbsp;&nbsp;
  SampleCApp.c <br>
 <br>
[Packages] <br>&nbsp;&nbsp;
  StdLib/StdLib.dec <br>&nbsp;&nbsp;
  MdePkg/MdePkg.dec <br>&nbsp;&nbsp;
  ShellPkg/ShellPkg.dec <br>
 <br>
[LibraryClasses] <br>&nbsp;&nbsp;
  LibC <br>&nbsp;&nbsp;
  LibStdio <br>&nbsp;&nbsp;
  UefiBootServicesTableLib
</span></p>
@snapend


Note:


---
@title[Lab 7 :Build and Test SampleCApp ]
<p align="right"><span class="gold" ><b>Lab 7 :Build and Test SampleCApp</b></span></p>
<span style="font-size:0.8em" >Build AppPkg at the VS Command prompt</span>
```shell
  C:/FW/edk2-ws/edk2> build -p AppPkg\AppPkg.dsc –m AppPkg\Applications\SampleCApp\SampleCApp.inf
```
<span style="font-size:0.8em" >Copy the built application to the EmulatorPkg runtime directory</span>
```shell
  C:/FW/edk2-ws/edk2> copy ..\Build\AppPkg\DEBUG_VS2015x86\X64\SampleCApp.efi  ..\Build\EmulatorX64\DEBUG_VS2015x86\X64
```
<span style="font-size:0.8em" >Run the Emulation</span>
```shell
  C:/FW/edk2-ws/edk2> RunEmulator.bat
```
<span style="font-size:0.8em" >Run the application SampleCapp from the shell</span>
```shell
 Shell> SampleCApp
 Press any Key and then <Enter> to Continue :

 Enter text. Include a dot (‘.’) in a sentence then <Enter> to exit:
 Some text here, then a DOT. 
 Shell>
```

Note:



---  
@title[Summary]
<BR>
### <p align="center"><span class="gold"   >Summary </span></p><br>
<br>
<ul style="list-style-type:none">
 <li>@fa[certificate gp-bullet-green]<span style="font-size:0.9em">&nbsp;&nbsp;UEFI Application with PCDs</span> </li>
 <li>@fa[certificate gp-bullet-cyan]<span style="font-size:0.9em">&nbsp;&nbsp;Simple UEFI Application</span></li>
 <li>@fa[certificate gp-bullet-yellow]<span style="font-size:0.9em">&nbsp;&nbsp;Add functionality to UEFI Application</span> </li>
 <li>@fa[certificate gp-bullet-magenta]<span style="font-size:0.9em">&nbsp;&nbsp;Using EADK with UEFI Application</span></li>
</ul>

 

---?image=assets/images/gitpitch-audience.jpg
@title[Questions]
<br>
![Questions](/assets/images/questions.JPG) 


---?image=assets/images/gitpitch-audience.jpg
@title[Logo Slide]
<br><br><br>
![Logo Slide](/assets/images/TianocoreLogo.png =10x)


---
@title[Acknowledgements]
#### <p align="center"><span class="gold"   >Acknowledgements</span></p>

```c++
/**
Redistribution and use in source (original document form) and 'compiled' forms (converted
to PDF, epub, HTML and other formats) with or without modification, are permitted provided
that the following conditions are met:

Redistributions of source code (original document form) must retain the above copyright 
notice, this list of conditions and the following disclaimer as the first lines of this 
file unmodified.

Redistributions in compiled form (transformed to other DTDs, converted to PDF, epub, HTML
and other formats) must reproduce the above copyright notice, this list of conditions and 
the following disclaimer in the documentation and/or other materials provided with the 
distribution.

THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR IMPLIED 
WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND 
FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL TIANOCORE PROJECT BE 
LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES 
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, 
DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, 
WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) 
ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF ADVISED OF THE POSSIBILITY 
OF SUCH DAMAGE.

Copyright (c) 2020, Intel Corporation. All rights reserved.
**/

```
