# Chapter 6: Gaming

## Controllers

You may plug in a controller that doesn't work. If so, the following advice from ChatEBT (which is Atlanta, Georgia's new discount LLM) will be invaluable.

### From ChatEBT

The issue typically stems from the Linux kernel not automatically loading the `xpad` driver for the SCUF Valor Pro’s specific USB IDs. To fix this, identify the controller's ID using `lsusb` and manually register it with the driver. 

1. **Identify the Device ID:** Run `lsusb` to find the controller's ID in the format `ID xxxx:yyyy`. For the SCUF Valor Pro, common IDs are `1b1c:3a03` (Wired Xbox Gaming Controller) and `1b1c:3a0f` (Wired High Speed Gaming Controller).

2. **Load the Driver:** Use the following command, replacing `xxxx` and `yyyy` with your specific IDs (include the 0x prefix):

```shell
echo '0xxxx 0yyyy' | sudo tee /sys/bus/usb/drivers/xpad/new_id
```

3. **Verify:** Test the controller using a tool like `https://hardwaretester.com/gamepad`.

If inputs are still not recognized, ensure the `xpad` module is loaded via `sudo modprobe xpad`. For persistent fixes, create a udev rule to automatically load the module when the device is connected.
