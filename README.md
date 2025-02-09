# IBController Setup Guide

## Initial Environment Setup

1. Activate trading environment:
```bash
source /home/yudaa0110/miniconda3/etc/profile.d/conda.sh
conda activate trading_env
```

## Download and Install IBController

1. Create directory and download IBController:
```bash
cd ~/ibkr
wget https://github.com/ib-controller/ib-controller/releases/download/3.4.0/IBController-3.4.0.zip
```

2. Install unzip if not already installed:
```bash
sudo apt-get install unzip
```

3. Extract IBController:
```bash
unzip IBController-3.4.0.zip -d IBController
```

## Configure IBController

1. Create configuration file:
```bash
nano ~/ibkr/IBController/IBController.ini
```

2. Add basic configuration:
```ini
# IBController Configuration
LogToConsole=yes
IbLoginId=your_username
IbPassword=your_password
FIX=no
StoreSettingsOnServer=no
MinimizeMainWindow=no
ExistingSessionDetectedAction=primary
AcceptIncomingConnectionAction=accept
ShowAllTrades=no
ForceTWSApiPort=7497
IbAutoClosedown=no
AcceptNonBrokerageAccountWarning=yes
```

## Create Startup Script

1. Create script:
```bash
nano ~/ibkr/start-ibgateway.sh
```

2. Add content:
```bash
#!/bin/bash
export TWS_MAJOR_VRSN=1030
export TWS_PATH="/home/yudaa0110/Jts"
export IBC_PATH="/home/yudaa0110/ibkr/IBController"
export IBC_INI="/home/yudaa0110/ibkr/IBController/IBController.ini"
export DISPLAY=:1

cd "$IBC_PATH"
./Scripts/DisplayBannerAndLaunch.sh --gateway --mode=paper
```

3. Make script executable:
```bash
chmod +x ~/ibkr/start-ibgateway.sh
```

## Testing IBController

1. Ensure VNC server is running:
```bash
# Check VNC status
ps aux | grep vnc
```

2. Test IBController startup:
```bash
~/ibkr/start-ibgateway.sh
```

## Directory Structure
```
~/ibkr/
├── IBController/
│   ├── IBController.ini
│   └── Scripts/
├── Jts/
│   └── ibgateway/
└── start-ibgateway.sh
```

## Important Notes

1. Verify paths:
   - TWS_PATH should point to your IB Gateway installation
   - IBC_PATH should point to IBController directory

2. Common issues:
   - Make sure DISPLAY is set correctly (:1 for VNC)
   - Check file permissions
   - Verify VNC server is running

3. Next steps (to be configured later):
   - System service setup
   - Automatic startup
   - Monitoring integration
   - Error handling

## Troubleshooting

1. If IBController doesn't start:
```bash
# Check logs
tail -f ~/ibkr/IBController/Logs/*.txt
```

2. If display issues:
```bash
# Verify DISPLAY variable
echo $DISPLAY
```

3. If permission issues:
```bash
# Fix permissions
chmod -R u+x ~/ibkr/IBController/Scripts
```

## Configuration Parameters

Key IBController.ini parameters:
- `LogToConsole`: Enable console logging
- `IbLoginId`: Your IB account username
- `IbPassword`: Your IB account password
- `ForceTWSApiPort`: API port (default 7497)
- `ExistingSessionDetectedAction`: How to handle existing sessions

## Command Reference

1. Start IBController:
```bash
~/ibkr/start-ibgateway.sh
```

2. Kill IBController process:
```bash
pkill -f IBController
```

3. View logs:
```bash
tail -f ~/ibkr/IBController/Logs/*.txt
```

## Environment Variables

Important environment variables:
- `TWS_MAJOR_VRSN`: IB Gateway version
- `TWS_PATH`: Path to IB Gateway installation
- `IBC_PATH`: Path to IBController installation
- `IBC_INI`: Path to IBController configuration
- `DISPLAY`: X display for GUI (usually :1 for VNC)
