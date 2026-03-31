# 1. Basic
## 1.1 Device Node Tree
```
DeviceNode(usbdev1)  DeviceNode(usbdev2)      Upper
     \        /
       \    /
    DeviceNode(usbhub)                        Lower In Device Tree
```

### 1.1.1 Traverse Tree Node
1. GetParent:
   1. Get Top Device Object Of Current Device Stack: ref section 1.2.1 Traverse Stack
   2. Get Deviec Node of Current Device Stack: TopDeviceObject->DeviceObjectExtension->DeviceNode;
     > [Definition of _DEVOBJ_EXTENSION](https://www.vergiliusproject.com/kernels/x64/windows-11/25h2/_DEVOBJ_EXTENSION)
     > [Definition of _DEVICE_NODE](https://www.vergiliusproject.com/kernels/x64/windows-11/25h2/_DEVICE_NODE)
     > **warning**: Be careful. If you use the Non-Top-DeviceObject. DeviceObjectExtension->DeviceNode remains NULL if the DeviceObject (FiDO) has the DO_DEVICE_INITIALIZING flag set
   3. Get Parent Device Node: DeviceNode->Parent;

## 1.2 Device Stack
DeviceStack of DeviceNode(usbdev1)
```
Upper FiDO  (like UsbPcap)                    Upper In Device Stack
FDO         (usbhid)
Lower FiDO  (other)
PDO         (usbhub bus driver created)       Lower In Device Stack
```

### 1.2.1 Traverse Stack
1. Get Upper Device Object: DeviceObject->AttachedDevice;
2. Get Lower Device Object: IoGetLowerDeviceObject(DeviceObject); // api from ntifs.h. should being included before wdm.h
