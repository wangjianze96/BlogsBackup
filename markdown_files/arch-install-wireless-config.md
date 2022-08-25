# Wireless Condiguration in Arch Linux

Prerequested packages:
- `wpa_supplicant`
- `dhcpcd`

Referecen:
[arch linux 开启自动连接wifi](https://www.codeleading.com/article/97032965127/)

## In installation
1. Check the wireless port name:
   ```
   ip link show
   ```
2. Turn on wlan port (replace `wlan0` with your own):
   ```
   ip link set wlan0 up
   ```
3. generate connection configuration (wifi's name & password):
   `TPLINKXXX` is the Wi-Fi's name that you want to connect and `TheStrongestPasswd` is your password.
   ```
   wpa_passphrase TPLINKXXX TheStrongestPasswd > wpa_supplicant-wlan0.conf
   ```
4. connect Wi-Fi use the generated configuration:
   Replace `wlan0` with your own wlan port.
   ```
   wpa_supplicant -c wpa_suppplicant-wlan0.conf -i wlan0
   ```

## Auto start-up connection after installation

1. Check your port name:
   ```
   ip link show
   ```
   the result should be like:
   ```
   1. lo: ....
   2. enp30s0: ....
   3. wlp50s0: ....
   ```
   `lo` is the loop; `enp40s0` is for ethernet; `wlp50s0` is for wireless connection.
2. Generate configuration file (root privilege required):
   ```
   wpa_passphrase TPLINKXXX TheStrongestPasswd > /etc/wpa_supplicant/wpa_supplicant-wlp50s0.conf
   ```
   **Noted**: the file should be named as such `wpa_supplicant-yourPortName.conf`. This is because `systemd` will use the file's name to transfer the port name.
3. Enable `wpa_supplicant` service
   ```
   systemctl enable wpa_supplicant@wlp50s0
   ```
4. Configure `systemd-network`:
   Create `/etc/systemd/network/00-wireless-dhcp.network`, according to Archbbs, the name can be varied.
   ```
   [Match]
   Name=wlp50s0
   
   [Network]
   DHCP=yes
   ```
5. Enable `systemd-networkd` service
   ```
   systemctl enable systemd-networkd.service
   ```
