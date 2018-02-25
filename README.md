# OpenBTS-<img src="https://raw.githubusercontent.com/mgp25/OpenBTS-UMTS/master/assets/umts.png" width=90>


### Contents

* [Introduction](#introduction)
* [Release Features/Capabilities](#release-featurescapabilities)
* [Supported Hardware](#supported-hardware)
	- [Ettus Research USRP](#ettus-research-usrp)
	- [CPU requirements](cpu-requirements)
* [Phones/Modems tested](#phonesmodems-tested)
* [SIMS and Authentication](#sims-and-authentication)
* [Build, Install, Setup, and Run Instructions](#build-install-setup-and-run-instructions)
	- [Prerequisites](#prerequisites)
	- [Obtaining the Source](#obtaining-the-source)
	- [Configure and Build](#configure-and-build)
	- [Ettus Research USRP](#etthus-research-usrp)
	- [Running OpenBTS-UMTS](#running-openbts-umts)
	
- [Programming your own USIM card](#programming-your-own-usim-card)
	- [Prerequisites](#prerequisites)
	- [Providers](#providers)
	- [Get the SIM programmer](#get-the-sim-programmer)
	- [Get the software (PySIM, PCSCd, Pyscard)](#get-the-software-pysim-pcscd-pyscard)
	- [Programming the SIM card](#programming-the-sim-card)
	- [Adding subscribers](#adding-subscribers)


	

## Introduction

OpenBTS-UMTS is a Linux-based application that uses a software radio to
present a UMTS network to any standard 3G UMTS handset or modem. It
builds upon the OpenBTS framework, where the MS or UE is treated as an
IP endpoint at the edge of the network.

## Release Features/Capabilities

  - supports original UMTS Release 99 (or Release 3)
  - supports packet-switched services only (i.e. data)
  - supports a single U-ARFCN
  - supports one or two high-speed active data sessions
  - spreading factors of 4-256
  - rate-1/2 convolutional coding
  - rate-1/3 turbo coding
  - maximum downlink data speed of 106 Kbytes/s
  - maximum uplink data speed of 52 Kbytes/s
  - Integrity Protection of GSM SIMs
  - Features not supported in this release:
      - circuit-switched services (e.g. voice, text)
      - handover
      - Inter-RAT mobility (moving b/w a 2G and 3G network)
      - paging
      - ciphering
      - USIM-based authentication

## Supported Hardware

Initial integration of the OpenBTS-UMTS public release was accomplished
with the Range Networks SDR1 (a.k.a. RAD1). Support for recent Ettus
Research USRP devices is now available. Integration with other
software-defined radios is ongoing, see [Radio
Integration](http://openbts.org/w/index.php?title=RadioIntegration) for more information about
connecting OpenBTS and OpenBTS-UMTS to different radio interfaces.


### Ettus Research USRP

**Important note:** Requires USB 3.0!

Supported Ettus Research products include third generation USRP devices
(B200 series and X-series) and second generation models with capable
bandwidth for UMTS (N-series). Older Ettus Research **USB 2.0 based
products are not supported** by OpenBTS-UMTS due to transport bus
limitations. Supported USRP devices include Intel SSE optimization for
UMTS pulse shaping and host resampling operations.

For full USRP details, see [Ettus Research
USRP](http://openbts.org/w/index.php?title=Ettus_Research_USRP).

<table class="wikitable">
<tr>
<th colspan="7">Ettus Research UMTS Capable Devices
</th></tr>
<tr>
<td>
</td>
<td>Transport
</td>
<td>Recommended RF
</td>
<td colspan="2">Frequency Accuracy
</td></tr>
<tr>
<td>B200
</td>
<td rowspan="2">USB 3.0
</td>
<td rowspan="2">Integrated
</td>
<td rowspan="2">TCXO 2.0 ppm
</td>
<td rowspan="6">GPSDO &lt;1 ppb
</td></tr>
<tr>
<td>B210
</td></tr>
<tr>
<td>X300
</td>
<td rowspan="2">1 or 10 Gigabit Ethernet
</td>
<td rowspan="5">SBX, WBX, CBX
</td>
<td rowspan="4">TCXO 2.5 ppm
</td></tr>
<tr>
<td>X310
</td></tr>
<tr>
<td>N200
</td>
<td rowspan="3">1 Gigabit Ethernet
</td></tr>
<tr>
<td>N210
</td></tr>
<tr>
<td>USRP2
</td>
<td>VCXO 20 ppm
</td>
<td>NA
</td></tr></table>
<table class="wikitable">
<tr>
<th colspan="4">UMTS Band Support
</th></tr>
<tr>
<td>
</td>
<td>Frequency Range
</td>
<td>UMTS Bands
</td>
<td>Output Power
</td></tr>
<tr>
<td>B200/B210
</td>
<td>70 MHz - 6 GHz
</td>
<td>1-14, 19-21, 22, 25, 26, 32
</td>
<td rowspan="4">up to 100 mW
</td></tr>
<tr>
<td>WBX
</td>
<td>50 Mhz - 2.2 GHz
</td>
<td>1-14, 19-21, 25, 26, 32
</td></tr>
<tr>
<td>SBX
</td>
<td>400 MHz - 4.4 GHz
</td>
<td>1-14, 19-21, 22, 25, 26, 32
</td></tr>
<tr>
<td>CBX
</td>
<td>1.2 GHz - 6 GHz
</td>
<td>1-4, 7, 9-11, 21, 22, 25
</td></tr></table>

### CPU requirements

OpenBTS-UMTS is a more computationally intensive application than
OpenBTS, since the UMTS channel bandwidth is roughly 13x larger than a
GSM channel. Generally, a multi-core high performance CPU is required,
such as Intel Core i3, i5, or i7 running at more than 1.6Ghz. Intel Atom
processors are too weak to support the current implementation


## Phones/Modems tested

  - Works
      - iPhones (3, 4, and 5)
      - HTC Velocity
      - Samsung Galaxy
      - Palm Pre
      - a variety of Multitech modems
  - Doesn't work
      - iPhone 7

## SIMS and Authentication

UMTS mandates mutual authentication between the UE and the NodeB. This
is a major change from 2G/2.5G authentication, where only the BTS
authenticates the MS.

A good presentation of UMTS security is available at:
[http://www.netlab.tkk.fi/opetus/s38153/k2003/Lectures/g42UMTS_security.pdf](http://www.netlab.tkk.fi/opetus/s38153/k2003/Lectures/g42UMTS_security.pdf)

Another detailed description is available at: [http://www.3g4g.co.uk/Tutorial/ZG/zg_security.html](http://www.3g4g.co.uk/Tutorial/ZG/zg_security.html)

So what does this mean? The subscriber registry will need to know the
SIM's Ki value to:

  - perform authentication and
  - enable integrity protection.

Without proper authentication and integrity protection, the UE will not
attach (or register) with OpenBTS-UMTS. For most users, this means you
must provide the SIMs for the UEs on the network. The only way to use
SIMs from another provider is to obtain the Ki through a roaming
interface to the provider's HLR/HSS.

This also means that some of the features that circumvented
authentication in OpenBTS, like open registration, are not possible with
OpenBTS-UMTS.

USIMs (e.g. 3G SIMs) are not currently supported by the OpenBTS-UMTS
implementation. They require different authentication algorithms than
GSM SIMs; these algorithms are not supported in the public release of
OpenBTS-UMTS.

## Build, Install, Setup, and Run Instructions

The following instructions support the Range Networks RAD1 and Ettus
Research USRP devices.

### Prerequisites

The following prerequisites are required to build and run OpenBTS-UMTS:

  - Development and build packages for your Linux distribution. These
    are mostly unchanged from OpenBTS.
    

`sudo apt-get install build-essential debhelper pkg-config autoconf libtool libtool-bin libortp-dev libsqlite3-dev libusb-1.0-0-dev libreadline-dev libzmq3-dev libosip2-dev sqlite3 libusb-1.0-0 libortp-dev libc6 pkg-config libreadline6 libosip2-11 git`
    


#### [ASN1C compiler](http://lionet.info/asn1c/). **Version 0.9.23 is required.**
 
 We also need ASN.1 C compiler that turns the formal ASN.1 specifications into the C code.
 
 Use [asn1c-0.9.23.tar.gz](https://github.com/mgp25/OpenBTS-UMTS/blob/master/asn1c-0.9.23.tar.gz) which is in the root folder of this repository.
    
```
test -f configure || autoreconf -iv
./configure
make
sudo make install
```

#### CoreDumper

OpenBTS uses the coredumper shared library to produce meaningful debugging information if OpenBTS crashes.

```
git clone https://github.com/RangeNetworks/libcoredumper.git
cd libcoredumper
sudo ./build.sh
sudo dpkg -i libcoredumper1_1.2.1-1_amd64.deb libcoredumper-dev_1.2.1-1_amd64.deb
```

#### Sipauthserve


Subscriber Registry API and SIP Authentication Server.

```
git clone https://github.com/RangeNetworks/subscriberRegistry.git
cd subscriberRegistry
git submodule init
git submodule update
sudo NodeManager/install_libzmq.sh
autoreconf -i
./configure
make
sudo make install
```

#### SIM

  - Valid SIM with a known IMSI and Ki values. These values must be
    added to the subscriber registry.

### Obtaining the Source

OpenBTS-UMTS is available on GitHub, ensure that you have Git version
1.8.2+ installed on your system.

    git clone https://github.com/mgp25/OpenBTS-UMTS

The `NodeManager` component is setup as a git submodule and can be added
with the following commands. Note that the current configuration
requires a GitHub account to check out the submodule.

    git submodule init
    git submodule update

### Configure and Build

You are now ready to build OpenBTS-UMTS, from your checked-out
OpenBTS-UMTS directory:

```
sudo NodeManager/install_libzmq.sh
./autogen.sh
./configure
make
sudo make install
```

**Note:** When running `sudo NodeManager/install_libzmq.sh` it might throw an error (source not found 404), you can ignote it.

#### Ettus Research USRP

USRP Driver:

`sudo apt-get install libuhd-dev libuhd003 uhd-host`

Make sure that the Ettus UHD driver was found in the output when running
configure. When found, Intel SSE support will also be tested and
automatically enabled. If UHD is not found, verify that the UHD driver
is properly installed.

    checking for UHD... yes
    checking whether mmx is supported... yes
    checking whether sse is supported... yes
    checking whether sse2 is supported... yes
    checking whether sse3 is supported... yes
    checking whether ssse3 is supported... yes
    checking whether sse4.1 is supported... yes
    checking whether sse4.2 is supported... yes

The UHD device should be detectable before running OpenBTS. Device
presence can be checked with the `uhd_usrp_probe` command installed with
the UHD driver.

```
$ uhd_usrp_probe
linux; GNU C++ version 4.8.3 20140911 (Red Hat 4.8.3-7); Boost_105400; UHD_003.007.003-0-ge10df19c

  _____________________________________________________
 /
|       Device: B-Series Device
|     _____________________________________________________
|    /
|   |       Mboard: B210
|   |   revision: 4
|   |   product: 2
|   |   serial: E0R05Z8BT
|   |   FW Version: 4.0
|   |   FPGA Version: 3.0

```

#### Range Networks RAD1

You will need to go into the TransceiverRAD1 directory and run the
following script right before running the OpenBTS-UMTS binary in the
apps directory (OpenBTS-UMTS/apps/):

`  sudo ./clkit_61_44mhz.sh`

#### Setup

To setup OpenBTS-UMTS, it may be needed to modify the settings such as ARFCN, DNS, Firewall. Navigate to `~/OpenBTS-UMTS/app` folder and open `OpenBTS-UMTS.example.sql` file with the preferable text editor. You may need to edit following options:

- 'GGSN.DNS' set to '8.8.8.8' to enable Google DNS.
- 'GGSN.Firewall.Enable' set to '0' to disable Firewall.
- 'UMTS.Radio.Band' - set the band you are going to use. Select from 850, 900, 1700, 1800, 1900 or 2100.
- 'UMTS.Radio.C0' - set the UARFCN. Range of valid values depend upon the selected operating band. Please, ensure that you are using free operating band.

To create folders and working database:

```
sudo mkdir /var/log/OpenBTS-UMTS
cd /etc/OpenBTS
sudo sqlite3 /etc/OpenBTS/OpenBTS-UMTS.db “.read OpenBTS-UMTS.example.sql”
sudo cp TransceiverUHD/transceiver ~/OpenBTS-UMTS/
sudo cp TransceiverUHD/transceiver apps
```

It's necessary to setup forwarding in iptables to properly forward data between devices, host machine, and the Internet:

```
sudo su
iptables -t nat -A POSTROUTING -j MASQUERADE -o eth0
echo 1 > /proc/sys/net/ipv4/ip_forward
exit
``` 

If you have Internet connection through the another interface (for example, Wi-Fi), you need to change eth0 to the applicable one (i.e., wlan0). Please note, that you need to setup forwarding in iptables every time after you computer rebooted.

**For SipAuthServe:**

Subscriber Registry controls database of subscriber information and in fact works as HLR (Home Location Registry):

```
sudo mkdir /var/lib/asterisk/
sudo mkdir /var/lib/asterisk/sqlite3dir/
sudo cp apps/comp128 ~/OpenBTS-UMTS/
sudo cp apps/comp128 ~/OpenBTS-UMTS/apps/
sudo cp apps/comp128 /OpenBTS
```


#### Running OpenBTS-UMTS

After successfully building and configuring, you are ready to launch
OpenBTS-UMTS:

```
cd OpenBTS-UMTS/apps
sudo ./OpenBTS-UMTS
cd subscriberRegistry/apps
sudo ./sipauthserve
```

---
**UMTSCLI part isn't working fine yet**

Several useful commands are available for debugging the packet-switched
OpenBTS-UMTS application. Launch the OpenBTS-UMTS CLI to manipulate and
configure your UMTS installation.

`sudo /OpenBTS-UMTS/OpenBTS-UMTSCLI`

In the CLI, type `help` and press ENTER for a list of available
commands. `help command` gives you a detailed info on a particular
command. Type `quit` to exit the CLI. THis does not stop the
OpenBTS-UMTS.


## Programming your own USIM card

### Prerequisites

`sudo apt-get install python-pip`

```
sudo python -m pip install serial pycrypto
```

### Providers

**sysmoUSIM-SJS1 4FF/nano SIM + USIM Card (10-pack):**

[http://shop.sysmocom.de/products/sysmousim-sjs1-4ff](http://shop.sysmocom.de/products/sysmousim-sjs1-4ff)

### Get the SIM programmer

You need a SIM card programmer which is compatible with the PCSC application on Linux. To have a more or less complete list of the compatible devices, please visit this page:

http://pcsclite.alioth.debian.org/ccid/supported.html

Don't forget that you need a programmer with APDU support. Personally we use **SCM Microsystems Inc. SCR 3310**, you can find it and many of the above list on Ebay.

### Get the software (PySIM, PCSCd, Pyscard)

**First install dependencies:**

`sudo apt-get install pcscd pcsc-tools libccid libpcsclite-dev`

Connect your SIM card reader, plug thhe programmable SIM card in, and check connectivity by running the following command:

`sudo pcsc_scan`

If your reader and card got recognized, you will see something similar:

```
PC/SC device scanner
V 1.4.22 (c) 2001-2011, Ludovic Rousseau <ludovic.rousseau@free.fr>
Compiled with PC/SC lite version: 1.8.10
Using reader plug'n play mechanism
Scanning present readers...
0: OMNIKEY AG CardMan 3121 01 00

Wed Dec 24 14:56:32 2014
Reader 0: OMNIKEY AG CardMan 3121 01 00
  Card state: Card inserted,
  ATR: 3B 9F 95 80 1F C7 80 31 E0 73 FE 21 13 57 12 29 11 02 01 00 00 C2

ATR: 3B 9F 95 80 1F C7 80 31 E0 73 FE 21 13 57 12 29 11 02 01 00 00 C2
+ TS = 3B --> Direct Convention
+ T0 = 9F, Y(1): 1001, K: 15 (historical bytes)
  TA(1) = 95 --> Fi=512, Di=16, 32 cycles/ETU
    125000 bits/s at 4 MHz, fMax for Fi = 5 MHz => 156250 bits/s
  TD(1) = 80 --> Y(i+1) = 1000, Protocol T = 0
-----
  TD(2) = 1F --> Y(i+1) = 0001, Protocol T = 15 - Global interface bytes following
-----
  TA(3) = C7 --> Clock stop: no preference - Class accepted by the card: (3G) A 5V B 3V C 1.8V
+ Historical bytes: 80 31 E0 73 FE 21 13 57 12 29 11 02 01 00 00
  Category indicator byte: 80 (compact TLV data object)
    Tag: 3, len: 1 (card service data byte)
      Card service data byte: E0
        - Application selection: by full DF name
        - Application selection: by partial DF name
        - BER-TLV data objects available in EF.DIR
        - EF.DIR and EF.ATR access services: by GET RECORD(s) command
        - Card with MF
    Tag: 7, len: 3 (card capabilities)
      Selection methods: FE
        - DF selection by full DF name
        - DF selection by partial DF name
        - DF selection by path
        - DF selection by file identifier
        - Implicit DF selection
        - Short EF identifier supported
        - Record number supported
      Data coding byte: 21
        - Behaviour of write functions: proprietary
        - Value 'FF' for the first byte of BER-TLV tag fields: invalid
        - Data unit in quartets: 2
      Command chaining, length fields and logical channels: 13
        - Logical channel number assignment: by the card
        - Maximum number of logical channels: 4
    Tag: 5, len: 7 (card issuer's data)
      Card issuer data: 12 29 11 02 01 00 00
+ TCK = C2 (correct checksum)

Possibly identified card (using /usr/share/pcsc/smartcard_list.txt):
3B 9F 95 80 1F C7 80 31 E0 73 FE 21 13 57 12 29 11 02 01 00 00 C2
        sysmocom sysmoUSIM-GR1
        http://sysmocom.de/
```

Hit `Ctrl+C` to exit `pcsc_scan`.

### Pyscard

Now you need to download and install Pyscard:

[http://pyscard.sourceforge.net/](http://pyscard.sourceforge.net/)

Download and extract the latest Pyscard version:

[https://sourceforge.net/projects/pyscard/files/pyscard/](https://sourceforge.net/projects/pyscard/files/pyscard/)

Go to the extracted Pyscard folder (where the setup.py file is located) and run the following command:

`sudo /usr/bin/python setup.py build_ext install`

### PySIM

Now get the code of PySIM:

```
git clone git://git.osmocom.org/pysim pysim
cd pysim
```

and run the `/pySim-read.py` to read your card:

`./pySim-read.py`

if you done everything allright, you will see something similar:

```
Reading ...
ICCID: 8901901550000123456
IMSI: 901550000123456
SMSP: fffffffffffffffffffffffffdffffffffffffffffffffffff069186770700f9ffffffffffffffff
ACC: ffff
MSISDN: Not available
Done !
```

Sometimes it is necessary to give the program the number of the card programmer:

`./pySim-read.py -p 0`      or       `./pySim-read.py -p 1`

Now we are ready to program the USIM finally! :-)

### Sysmo USIM Tool

Get Sysmo USIM tool:

`git clone git://git.sysmocom.de/sysmo-usim-tool`

We will need: `sysmo-usim-tool.sjs1.py`


### Programming the SIM card

**Important:**


In order to program the USIM cards, you must use the `zecke/tmp2` branch of Pysim. Please note that with the `zecke/tmp2` branch you can program but cannot read the cards. If you want to read the cards you will need to swtich back to the `master` branch. **If you are not using the** `zecke/tmp2` **branch or you are not giving the ADM1 pin correctly, you can permanently damage your card!!!**

To change branch to `zecke/tmp2`, use this command:

`git checkout zecke/tmp2`

Example to program a SysmoUSIM-SJS1 card:

```
./pySim-prog.py -p 0 --mcc 101 --mnc 02 -t sysmoUSIM-SJS1 --imsi 101020000000003 --iccid 8988211000000012345 --ki 8BAF473F2F8FD09487CCCBD7097C6862 --pin-adm 53770832
```

**IMPORTANT:** Where `-a` is the part where you need to give the `ADM1` for this specific SIM card. Again, if you are not using the `zecke/tmp2` branch or not giving the proper `ADM1` pin when you try to program the Sysmo-USIm S1J1 SIMs, you will likely end up with a permamnently damaged card!

Now lets set COMP128v1 algorithm and disable USIM application in order to make it work with OpenBTS or OpenBTS-UMTS:

`./sysmo-usim-tool.sjs1.py --adm1 ADM1_KEY -T COMP128v1:COMP128v1 --classic`


**Reference:** [Sysmo USIM Manual](https://www.sysmocom.de/manuals/sysmousim-manual.pdf)

### Adding subscribers

`sudo ./nmcli.py sipauthserve subscribers create "name" imsi msisdn ki`


Values imsi, msisdn and ki should be taken from your SIM-card.

You can use the following command to show all subscribers:

`./nmcli.py sipauthserve subscribers read`

Now, connect your phone to the network and test 3G. That's it!