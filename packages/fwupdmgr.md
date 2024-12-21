```
# Display supported devices 
fwupdmgr get-devices

# Downloading the latest metadata from LVFS
fwupdmgr refresh

# Checking for available firmware updates
fwupdmgr get-updates

# Update the device firmware
# Updates that can be applied live will be done immediately.
# Updates that run at bootup will be staged for the next reboot.
fwupdmgr update

# Optional: Send Telemetry
# fwupd project encourages users to report both successful and failed updates back to LVFS.
# You can send the report using:
fwupdmgr report-history

```