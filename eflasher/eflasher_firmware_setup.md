
# Hướng dẫn cài đặt firmware eflasher

## 1. Cài đặt firmware eflasher vào thẻ nhớ
- Sử dụng phần mềm BalenaEtcher.
- Link tải firmware: [https://github.com/smatecvn/mgw-h6/tree/main/eflasher](https://github.com/smatecvn/mgw-h6/tree/main/eflasher)

## 2. Khởi động thiết bị với thẻ nhớ
- Cắm thẻ nhớ vào thiết bị để khởi động.
- Kiểm tra thông tin mount thẻ nhớ vào `/mnt/mmcblk0p1`.
  - `/dev/mmcblk0p4` -> `/mnt/mmcblk0p1`

### Các bước kiểm tra
**Bước 1:** Dùng lệnh `df -h`
```bash
root@MIRA:~# df -h
Filesystem                Size      Used Available Use% Mounted on
/dev/root                30.8M     30.8M         0 100% /rom
tmpfs                   997.9M      1.2M    996.7M   0% /tmp
/dev/loop0              171.3M     45.5M    125.7M  27% /overlay
overlayfs:/overlay      171.3M     45.5M    125.7M  27% /
tmpfs                   512.0K         0    512.0K   0% /dev
/dev/mmcblk0p4            6.8G     38.6M      6.4G   1% /mnt/mmcblk0p1
```

**Bước 2:** Nếu Bước 1 chưa đúng, tiếp tục:

- Thực hiện lệnh:
  ```bash
  block detect > /etc/config/fstab
  ```

- Kiểm tra file `/etc/config/fstab`:
```bash
root@MIRA:/# cat /etc/config/fstab

config global
        option anon_swap '0'
        option anon_mount '0'
        option auto_swap '1'
        option auto_mount '1'
        option delay_root '5'
        option check_fs '0'

config mount
        option target '/overlay'
        option uuid '0ab1b707-6028-4ba0-9ef3-d9726bf824ac'
        option enabled '0'

config mount
        option target '/mnt/mmcblk0p1'
        option uuid '66A6-EF81'
        option enabled '0'

config mount
        option target '/rom'
        option uuid '906ea478-0fa4b76d-50223213-5547e8a4'
        option enabled '0'

config mount
        option target '/mnt/mmcblk0p4'
        option uuid '66692a6a-115f-4902-8b5f-b2c789325148'
        option enabled '0'
```

- Mở file bằng lệnh `vi /etc/config/fstab` và chỉnh sửa:
  ```
  /mnt/mmcblk0p4 -> /mnt/mmcblk0p1
  option enabled '0' -> option enabled '1'
  ```

- File sau chỉnh sửa:
```bash
config global
        option anon_swap '0'
        option anon_mount '0'
        option auto_swap '1'
        option auto_mount '1'
        option delay_root '5'
        option check_fs '0'

config mount
        option target '/overlay'
        option uuid '0ab1b707-6028-4ba0-9ef3-d9726bf824ac'
        option enabled '0'

config mount
        option target '/mnt/mmcblk0p1'
        option uuid '66A6-EF81'
        option enabled '0'

config mount
        option target '/rom'
        option uuid '906ea478-0fa4b76d-50223213-5547e8a4'
        option enabled '0'

config mount
        option target '/mnt/mmcblk0p1'
        option uuid '66692a6a-115f-4902-8b5f-b2c789325148'
        option enabled '1'
```

- Thực hiện lệnh:
  ```bash
  mount /dev/mmcblk0p4 /mnt/mmcblk0p1
  ```

## 3. Copy firmware vào thư mục `/mnt/mmcblk0p1`
- Sử dụng phần mềm WinSCP.

**Kết quả sau khi copy:**
```bash
root@MIRA:/mnt/mmcblk0p1# ls
lost+found
mgwp
mpd
openwrt-sunxi-cortexa53-xunlong_orangepi-zero2w-squashfs-sdcard.img.gz
root@MIRA:/mnt/mmcblk0p1#
```
