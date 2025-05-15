# lpac

lpac is a cross-platform local profile agent program, compatible with [SGP.22 version 2.2.2](https://www.gsma.com/solutions-and-impact/technologies/esim/wp-content/uploads/2020/06/SGP.22-v2.2.2.pdf).

Features:

- Support Activation Code and Confirmation Code
- Support Custom IMEI sent to server
- Support Profile Discovery (SM-DS)
- Profile management: list, enable, disable, delete and nickname
- Notification management: list, send and delete
- Lookup eUICC chip info
- etc

## Usage

You can download lpac from [GitHub Release][latest], and read [USAGE](docs/USAGE.md) to use it.
If you can't run it you need to compile by yourself, see also [DEVELOPERS](docs/DEVELOPERS.md).
If you want to known which Linux distributions include lpac, see also [LINUX-DIST](docs/LINUX-DIST.md).
If you have any issue, please read [FAQ](docs/FAQ.md) first.

[latest]: https://github.com/estkme-group/lpac/releases/latest

## Software Ecosystem

- [EasyLPAC] (Windows, Linux and macOS)
- [{Open,Easy}EUICC][openeuicc] ([Mirror][openeuicc-mirror], Android)
- [eSIM Manager (lpa-gtk)](https://codeberg.org/lucaweiss/lpa-gtk) (Linux Mobile)
- [rlpa-server](https://github.com/estkme-group/rlpa-server) for eSTK.me Cloud Enhance function

[easylpac]: https://github.com/creamlike1024/EasyLPAC/releases/latest
[openeuicc]: https://gitea.angry.im/PeterCxy/OpenEUICC
[openeuicc-mirror]: https://github.com/estkme-group/openeuicc

## Thanks

[![Contributors][contrib]][contributors]

[contrib]: https://contrib.rocks/image?repo=estkme-group/lpac
[contributors]: https://github.com/estkme-group/lpac/graphs/contributors

---

## License

- lpac ([/src](src), [/driver](driver)): AGPL-v3.0-only
- libeuicc ([/euicc](euicc)): LGPL-v2.1-only or Commercial License Agreement with ESTKME TECHNOLOGY LIMITED, Hong Kong
- cjson ([/cjson](cjson)): MIT
- dlfcn-win32 ([/dlfcn-win32](dlfcn-win32)): MIT

Copyright &copy; 2023-2025 ESTKME TECHNOLOGY LIMITED, Hong Kong

---
---
---

## Lenovo Modifications (Quectel WWAN Modules Support)
Changes are mostly minor and related to changing APDU and default AT device.
- In src/main.c, line 135: Changed apdu_driver from 'pcsc' to 'at'
- In driver/apdu/at.c, added modifications to add support for the following devices: EM05G,RM520N-GL,EM160R-GL

## Dependencies
Install dependencies:

$sudo apt install pkg-config\
$sudo apt install libpcsclite-dev\
$sudo apt install cmake\
ssudo apt install libmbim-glib-dev\
$sudo apt install libcurl4-openssl-dev

## Build
Standard Cmake instructions are applicable:

$cd lpac_modified\
$mkdir build\
$cd build\
$cmake ..\
$make 

## Usage
Go to /build/output/ first.

Usage instructions are in /docs/USAGE.md\
But below are some commands that can be quickly referenced for profile operations:

### Profile Operations

#### > Base command:
>$sudo ./lpac profile <list|enable|disable|nickname|delete|download|>

Below are some commands that are useful for profile management:

#### > Profile Download
Base command:
>$sudo ./lpac profile download -a '<LPA_Authentication_code>'

Example:\
sudo ./lpac profile download -a 'LPA:1$rsp.truphone.com$QR-G-5C-1LS-1W1Z9P7'

#### > Profile list
>$sudo ./lpac profile list

Sample output:\
{"type":"lpa","payload":{"code":0,"message":"success","data":[{"iccid":"89000000000000000012","isdpAid":"a0000005591010ffffffff8900002000","profileState":"disabled","profileNickname":"test22","serviceProviderName":"Rohde & Schwarz","profileName":"R&S CMW500","iconType":null,"icon":null,"profileClass":"test"}]}}


#### > Profile Enable/disable/delete

The 'isdpAid' is used as reference for profile

Example:\
$sudo ./lpac profile enable a0000005591010ffffffff8900002000\
$sudo ./lpac profile disable a0000005591010ffffffff8900002000\
$sudo ./lpac profile delete a0000005591010ffffffff8900002000
