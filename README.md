```
$mb_sn = Get-WmiObject Win32_BaseBoard | Select-Object -ExpandProperty SerialNumber
$mach_guid = Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Cryptography" -Name MachineGuid | Select-Object -ExpandProperty MachineGuid
$bios_sn = Get-WmiObject Win32_BIOS | Select-Object -ExpandProperty SerialNumber
$csp_uuid = Get-WmiObject Win32_ComputerSystemProduct | Select-Object -ExpandProperty UUID
$dm_pnpdid = Get-WmiObject Win32_DesktopMonitor | Select-Object -ExpandProperty PNPDeviceID
$dd_sn = Get-WmiObject Win32_DiskDrive | Select-Object -ExpandProperty SerialNumber
$ld_did_vsn = (Get-WmiObject Win32_LogicalDisk | Select-Object DeviceID, VolumeSerialNumber | ForEach-Object { ($_.DeviceID -replace ':', '') + '_' + $_.VolumeSerialNumber }) -join ','
$mc_dl_pn = (Get-WmiObject Win32_PhysicalMemory | Select-Object PartNumber, SerialNumber | ForEach-Object { $_.PartNumber.Trim() + '_' + $_.SerialNumber.Trim() }) -join ','
$os_sn_id_ru = (Get-WmiObject Win32_OperatingSystem | Select-Object SerialNumber, InstallDate, RegisteredUser |  ForEach-Object { $_.RegisteredUser.Trim() + '_' + $_.SerialNumber.Trim() + '_' + $_.InstallDate }) -join ','
$vol_dl_l_sn_did =  (Get-WmiObject Win32_Volume | Select-Object DriveLetter, Label, SerialNumber, DeviceID |  ForEach-Object { ($_.DriveLetter -replace ':', '') + '_' + $_.Label + '_' + $_.SerialNumber + '_' + ($_.DeviceID | Select-String -Pattern "\{([a-z0-9-]{36})\}").Matches.Groups[1].Value }) -join ','
$sid_base = (Get-WmiObject Win32_UserAccount -Filter "Name='$env:USERNAME'").SID -replace '-\d+$', ''

Write-Output @{
mb_sn = $mb_sn
mach_guid = $mach_guid
bios_sn = $bios_sn
csp_uuid = $csp_uuid
dm_pnpdid = $dm_pnpdid
dd_sn = $dd_sn
ld_did_vsn = $ld_did_vsn
mc_dl_pn = $mc_dl_pn
os_sn_id_ru = $os_sn_id_ru
vol_dl_l_sn_did = $vol_dl_l_sn_did
sid_base = $sid_base
} | ConvertTo-Json

```
