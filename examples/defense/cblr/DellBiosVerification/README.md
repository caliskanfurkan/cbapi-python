# Dell BiosVerification.py Live Response API Script

## References

Dell Trusted Device Product Information: https://www.delltechnologies.com/endpointsecurity

Dell Trusted Device Installation Instructions: https://www.dell.com/support/manuals/us/en/04/trusted-device/trusted_device/installation?guid=guid-b9217d4f-6932-47d2-8db5-50633eb47691&lang=en-us

Troubleshooting: https://www.dell.com/support/manuals/us/en/04/trusted-device/trusted_device/results-troubleshooting-and-remediation?guid=guid-240f1964-167a-41b0-9fb3-687dddbdb71f&lang=en-us 



## Summary

This set of tools uses the VMware Carbon Black Security Cloud Live Response APIs to retrieve
artifacts generated by the Dell Trusted Device SafeBIOS verification service. The Dell Trusted
Device agent saves BIOS image files to the filesystem when a verification failure event is
detected.

Incident responders can use this set of scripts to retrieve the BIOS image files for forensic
analysis.


## Instructions

Usage:

To retrieve the BIOS image files from a device in a failed verification state via the Live Response API:


1. Copy the BiosVerification.py and dellbios.bat files to the same directory on the administrator system. 
2. Install the cbapi Python bindings: https://github.com/carbonblack/cbapi-python
3. Create a Live Response API key https://developer.carbonblack.com/reference/carbon-black-cloud/authentication/ 
4. Configure credentials on the administrator system: https://cbapi.readthedocs.io/en/latest/getting-started.html 
5. Run the provided BiosVerification.py utility with the following command line to target the failed system:
```
BiosVerification.py --get --machinename <MACHINE NAME>
```

If failed BIOS image files are found the script will retrieve the image files to the local administrator system in a compressed archive named 
```
<MACHINE NAME>-BiosImages.zip
```

## Example

```
$ ./BiosVerification.py --get --machinename "x\LT-7400"

[ * ] Establishing LiveResponse Session with Remote Host:
     - Hostname: x\LT-7400
     - OS Version: Windows 10 x64
     - Sensor Version: 3.6.0.1201
     - AntiVirus Status: ['AV_ACTIVE', 'ONDEMAND_SCAN_DISABLED']
     - Internal IP Address: 172.16.0.196
     - External IP Address: x.x.x.x

[ * ] Uploading scripts to the remote host
[ * ] Getting the images


c:\program files\confer\temp>mkdir c:\tmpbios 
A subdirectory or file c:\tmpbios already exists.

c:\program files\confer\temp>del BiosImages.zip 
Could Not Find c:\program files\confer\temp\BiosImages.zip

c:\program files\confer\temp>"C:\Program Files\Dell\BiosVerification\Dell.TrustedDevice.Service.Console.exe" -exportall -export c:\tmpbios 
Wrote image to c:\tmpbios\BIOSImageCaptureBVS06092020_120255.bv

c:\program files\confer\temp>powershell.exe -ExecutionPolicy Bypass Compress-Archive -Path c:\tmpbios\*.* -DestinationPath BiosImages.zip -Force 

[ * ] Removing scripts
[ * ] Downloading images
[ * ] Writing out x\LT-7400-BiosImages.zip

[ * ] Cleaning up

```


This script is compatible with the full VMware Carbon Black Cloud API and requires the python cbapi.