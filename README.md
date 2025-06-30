# Modified version of bip39scan v2.1.7 - For sale $250
To purchase, write to telegram ```@phrutis``` or buy from a bot in a group [t.me/cuda8](https://t.me/cuda8)

Updates:<br>
1. Added a patch for the found address to the window and file.<br>
2. Output of the find to the file is now CSV.<br>
3. Added Status.txt (every 5 minutes the last position of the brute is written).<br>
4. When reading a dictionary from a file, a processbar with the output of % progress was added.<br>
5. Minor changes.
<hr>
A new vulnerability has been discovered - thousands of mnemonic phrases are at risk.<br>
All applications and generators of mnemonic phrases accept only 2048 standard words.<br>
Phrases of length 3,6,9,12,15,18,21,24 words and not all of them, only valid phrases.<br>
What clumsy programmer in the application or generator made a critical error.<br>
When a user adds a password to a phrase, it replaces the entire phrase with itself.<br>
Usually, those who need additional protection and security put passwords on phrases.<br>
Most often, these are users who have large coin balances.<br>
It is not known whether the developer did this accidentally or intentionally?<br>
The fact remains that thousands of phrases are at risk.<br>

## Correct generation of phrase and password
![Image](https://github.com/user-attachments/assets/c4a45a7a-08e9-491d-be49-b21739787424)

## Vulnerable Phrase and Password Generation
![Image](https://github.com/user-attachments/assets/335d0cc1-e37e-4906-a07c-2836111d6486)

## The second version of the origin of the finds.
In 2015-2016, there was an online service live.ether where everyone could generate addresses using passwords.<br>
At first https://live.ether.camp they generated from camp 2031 iteration of SHA-3 (Keccak), then switched to a more secure generation of pbkdf2_hmac_sha512 2048 iterations.<br>
The service worked for about a year and closed, the wallets remained.<br>

![Image](https://github.com/user-attachments/assets/5bc500a2-07a8-4612-b591-e3fd0bf71c52)

A modification bip39scan v2 was developed for this vulnerability<br>
Only CUDA cards support GTX, RTX, CMP<br>
Brute speed:<br>
RTX 4090 = 520k/s<br>
RTX 5090 = 800k/s<br>
In Ubuntu (linux, hiveos) the speed can be higher by 10-20%<br>
The program is sold with the source code (cmake VS2022) + ready-made programs (multigpu) for win, lin, hive, ubuntu<br>
It is intended for research purposes and analysis only.<br>
I do not recommend touching active accounts, transferring other people's coins, etc.<br>
In some countries this is illegal and punishable.<br>
Don't ask about the balances of the finds. <br>
If you are interested, search the transaction history by date.<br>

List of found passwords 7165 pcs.t.<br>
You can see the [**list of FOUNDS passwords**](https://github.com/phrutis/bip39scan2/blob/main/FOUNDS.md) or download [**CSV**](https://github.com/phrutis/bip39scan2/releases/download/2.0.1/Founds_CSV.txt) for Emeditor<br>
According to the analysis of the findings, almost all accounts are ether (a little Bitcoin)<br>
You can check the findings in the modification of the popular phrase generator [bip39scan.html](https://github.com/phrutis/bip39scan2/releases/download/2.0.1/bip39scan.html)<br>
Open the page using a browser (works autonomously + offline)<br>
In this generator, you can also create a test password to test the program


## Mode 1 (Generator)
 
Sequential generation of passwords from a given alphabet.<br>
The increment function (automatic increase of password length) has been added to the generation<br>
The alphabet is in a text file, it is easy to change.<br>

```bip39scan.exe -a eth.txt --save Found.txt -p m/44'/60'/0'/0/0-9 --alphabet alpha.txt --start A```

https://github.com/user-attachments/assets/9b000438-5aae-42f0-a560-0fe109114151

You can start (continue) the search from the desired position<br>
```bip39scan.exe -a eth.txt --save Found.txt -p m/44'/60'/0'/0/0-9 --alphabet alpha.txt --start FromHire12345```

If you need a gap in the starting combination<br>
```bip39scan.exe -a eth.txt --save Found.txt -p m/44'/60'/0'/0/0-9 --alphabet alpha.txt --start "Hello 2@@5!"```

https://github.com/user-attachments/assets/580c0bdf-8f8b-490f-8ea4-87018dd8cd73

Important! The symbols from the starting position must be present in the alphabet.

## Mode 2 (professional brute)<br>
Using an external password generator with advanced features.

Generation by mask [info](https://hashcat.net/wiki/doku.php?id=mask_attack) <br>
```hashcat.exe --stdout -a 3 -1 ?u?l ?1?l?l?l?d?d?d?d | bip39scan.exe -a eth.txt -m stdin --save Found.txt -p m/44'/60'/0'/0/0-9```<br>
Use your own masks and character sets

Generation by mask with increment<br>
```hashcat.exe --stdout -a 3 --increment --increment-min=1 --increment-max=9 -1 ?u?l ?1?l?l?l?l?l?l?l | bip39scan.exe -a eth.txt -m stdin --save Found.txt -p m/44'/60'/0'/0/0-9```

https://github.com/user-attachments/assets/4c5c33c5-7c16-4674-982e-603860b0f5fa

Dictionary multiplied by dictionary to create phrases from several words [info](https://hashcat.net/wiki/doku.php?id=combinator_attack)<br>
```hashcat.exe --stdout -a 1 dict1.txt dict2.txt | bip39scan.exe -a eth.txt -m stdin --save Found.txt -p m/44'/60'/0'/0/0-9```

Dictionary + rule. Passwords are taken from a file, the specified variations are created by the rule. [info](https://hashcat.net/wiki/doku.php?id=rule_based_attack)<br>
```hashcat.exe --stdout -a 0 dict1.txt -r TOP.rule | bip39scan.exe -a eth.txt -m stdin --save Found.txt -p m/44'/60'/0'/0/0-9```

## Mode 3
Reading passwords and phrases from a text file.<br>
```bip39scan.exe -a eth.txt -m Dict.txt --save Found.txt -p m/44'/60'/0'/0/0-9```

https://github.com/user-attachments/assets/e57c34c1-bdde-40af-b8e1-16c5e03e4298

The program reads passwords from a new line files up to 32 TB

## Linux (ubuntu, hiveos)
The commands are the same as for Windows.<br>
You need to add in patch \ before '

```chmod +x bip39scan```<br>
``` ./bip39scan -a eth.txt --save Found.txt -p m/44\'/60\'/0\'/0/0-9 --alphabet alpha.txt --start A```

## Database .bin
To avoid waiting for a long time for the addresses to be loaded into the program.<br>
Create and use binary databases.<br>
The program will start in seconds<br>
Create databases:<br>
BTC<br>
```bip39scan.exe --save Found.txt -a btc1.txt -t P2PKH --save-bin btc1.bin -p m/44'/0'/0'/0/0-9 --alphabet alpha.txt --start A```<br>
```bip39scan.exe --save Found.txt -a btc3.txt -t P2SH --save-bin btc3.bin -p m/49'/0'/0'/0/0-9 --alphabet alpha.txt --start A```<br>
```bip39scan.exe --save Found.txt -a btc-bc.txt -t bech32 --save-bin btc-bc.bin-p m/84'/0'/0'/0/0-9 --alphabet alpha.txt --start A```

ETH and tokens<br>
```bip39scan.exe --save Found.txt -a eth_addresses.txt --save-bin eth.bin -t ethereum -p m/44'/60'/0'/0/0-9 --alphabet alpha.txt --start A```

Next launches run like this<br>
```bip39scan.exe --save Found.txt -a btc1.bin -t P2PKH -p m/44'/0'/0'/0/0-9 --alphabet alpha.txt --start A```<br>
```bip39scan.exe --save Found.txt -a btc3.bin -t P2SH -p m/49'/0'/0'/0/0-9 --alphabet alpha.txt --start A```<br>
```bip39scan.exe --save Found.txt -a btc-bc.bin -t bech32 -p m/84'/0'/0'/0/0-9 --alphabet alpha.txt --start A```<br>
```bip39scan.exe --save Found.txt -a eth.bin -t ethereum -p m/44'/60'/0'/0/0-9 --alphabet alpha.txt --start A```


Where can I download a fresh database of addresses?<br>
BTC and other coins https://blockchair.com/dumps<br>
ETH https://privatekeyfinder.io/dump/ethereum.tsv.gz<br>
Each address must be on a new line.<br>
Ethereum addresses must be 0x...<br>
Bitcoin addresses 1.., 3.., bc.. (New long addresses bc.. does not accept)

# Download ready-made address databases for bip39scan

**ALL ETH 1145590543 addresses** with balance 20/05/2025 + empty + ALL ETH TOKENS with balance + empty history:<br>
ARBITRUM, AVALANCHE, BASE, BNB, BSC, BTT, CRONOS, CELO, ETC, Ethereumnie, ERA, ERC20, ETH, Ethered, FANTOM, <br>
GETH (Goerli), GNOSIS, IOTX, LINEA, MOONBEAM, MOONRIVER, OPBNB, OPTIMISM, POLYGON, VET, ZKEVM-POLYGON...<br>
Add these arguments to run ```--bloom 4096M -t ethereum```<br>
(The database fits on a 12 GB card or more)<br>
Download http://89.23.98.83/up/alleth.bin  **22.5 GB**

**ALL BTC addresses 1...** P2PKH with balance + empty (history)<br>
Add these arguments to run ```--bloom 2048M -t P2PKH```<br>
Download http://89.23.98.83/up/allbtc1.bin  **11.9 GB**

**ALL BTC addresses 3...** P2SH with balance + empty (history)<br>
Add these arguments to run ```--bloom 2048M -t P2SH```<br>
Download http://89.23.98.83/up/allbtc3.bin  **7.6 GB**

**ALL BTC addresses bc1q...** bech32 with balance + empty (history)<br>
Add these arguments to run ```--bloom 2048M -t bech32```<br>
Download http://89.23.98.83/up/allbc.bin  **6.5 GB**

## Addresses only with positive balance

Download ETH addresses 0x ```-t ethereum``` 20/05/2025<br>
http://89.23.98.83/up/eth.bin  **3.1 GB**

Download BTC addresses 1... 30/06/2025<br>
http://89.23.98.83/up/btc1.txt  **787 MB**

Download BTC addresses 3... 30/06/2025<br>
http://89.23.98.83/up/btc3.txt **240 MB**

Download BTC addresses bc... 30/06/2025<br>
http://89.23.98.83/up/bc.txt **842 MB**

# BONUS
Generator based on the vulnerable libbitcoin library v3.2 for Linux and Windows.<br>
For the convenience of generating vulnerable phrases, ```bx.py``` has been added.<br>
Script with the required selection of values ​​(multicpu).<br>
Run: ```py bx.py```<br> (python3 bxlin.py for linux)<br>
The first version of the [**bip39scan**](https://github.com/phrutis/bip39scan) program with the source code is included as a gift

![Image](https://github.com/user-attachments/assets/43daffcb-1438-4435-b52e-fc9508c8dbed)


## FAQ
Made from a 25 GB address base a 11 GB bin base with an error when starting:<br>
```Kernel memory: 8128MNo enough memory in dev #0 for bloom filter allocation```

Allocate more memory for the bloom filter.<br>
Add an argument to the startup line ```--bloom 2048M```<br>
base.bin - 22 GB+ USE ```--bloom 4096M```
<hr>



## Building on Windows VS-2022

Install cmake 3.30+ from this link: https://github.com/Kitware/CMake/releases/download/v3.31.8/cmake-3.31.8-windows-x86_64.msi<br>
Or find another version on this page: https://cmake.org/download/<br>

Install Visual Studio 2022 community: https://learn.microsoft.com/en-us/visualstudio/install/install-visual-studio?view=vs-2022<br>
click the big Download button
 
Install Nvidia CUDA 12.9: https://developer.nvidia.com/cuda-downloads?target_os=Windows&target_arch=x86_64&target_version=11&target_type=exe_local<br>
choose Windows version
 
Install OpenSSL from https://slproweb.com/download/Win64OpenSSL-3_0_16.msi

Go to the "Sources" folder<br>
In cmd run: 

```cmake bip39scan```

The project files will appear in the folder:<br>
bip39scan.sln (run this file the project will open)<br>
bip39scan.vcxproj<br>
bip39scan.vcxproj.filters<br>
..

![brute bip39 phrases](https://github.com/user-attachments/assets/16e2197f-a01e-43ee-a02e-5b6f9e515e15)

OpenSSL should be found. If now, rename c:\program files\OpenSSL-Win64 to OpenSSL and re-run. Note that the libcrypto dll should<br>
be in PATH or in the current directory when running bip39scan.

 
Visual Studio 2022 opens. In the top toolbar choose: Release/x64.<br>
In the "Solution explorer" to the right, right-click bip39scan, choose "build".<br>
The executable builds in the your-build-directory\Release
 
![gpu brute bip39 mnemonic](https://github.com/user-attachments/assets/34d6574c-20a3-462c-a857-117ae9ae664f)

If necessary run precomp.exe file precomp.bin will be generated

## Building on Ubuntu:

Below is detailed instruction with bash commands required to build bip39scan.<br>
The symbol '$' denotes command prompt.<br>
If your prompt is shown as '#' on your terminal, skip 'sudo'.<br>
For example, instead of

$ sudo sh cuda_12.0.1_525.85.12_linux.run

you should run

sh cuda_12.0.1_525.85.12_linux.run

Let's start.

install CUDA. Download the linux version from the NVIDIA website and run.<br>
Open https://developer.nvidia.com/cuda-12-0-1-download-archive?target_os=Linux<br>
in your browser and choose your system. The following is valid for Ubuntu 18.04.

$ wget https://developer.download.nvidia.com/compute/cuda/12.0.1/local_installers/cuda_12.0.1_525.85.12_linux.run<br>
$ sudo sh cuda_12.0.1_525.85.12_linux.run

Skip the driver installation (deselect the 'driver' checkbox) if you already have it.

To ensure the cuda is installed, run:<br>
$ nvcc --version<br>

It should print information and version of CUDA.<br>
If no nvcc is found, try adding the CUDA bin path to the PATH variable:<br>
$ export PATH=/usr/local/cuda/bin:$PATH

install build-essential:<br>
$ sudo apt-get install build-essential

Check the gcc version:<br>
$ gcc --version

if the version is less than 9, install gcc 9:

$ sudo apt-get install software-properties-common<br>
$ sudo add-apt-repository ppa:jonathonf/gcc<br>
$ sudo apt-get update<br>
$ sudo apt-get install gcc-9<br>
$ sudo apt-get install g++-9<br>
$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 10<br>
$ sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-9 10

install libssl:<br>
$ sudo apt-get install libssl-dev<br>
for the client-server version, install boost:<br>
$ sudo apt-get install libboost-all-dev

You will need Boost version at least 1.71. If apt-get does not intall at least 1.71, build Boost from source:
$ wget https://boostorg.jfrog.io/artifactory/main/release/1.71.0/source/boost_1_71_0.tar.gz<br>
$ tar -xzvf boost_1_71_0.tar.gz<br>
$ cd boost_1_71_0<br>
$ ./bootstrap.sh --prefix=/usr && ./b2 stage threading=multi link=static<br>
$ sudo ./b2 install threading=multi link=static<br>
$ sudo ln -svf detail/sha1.hpp /usr/include/boost/uuid/sha1.hpp

install cmake from here: https://cmake.org/download/ choose Binary distributions, if that does not work - build from source<br>
$ wget https://github.com/Kitware/CMake/releases/download/v3.28.0/cmake-3.28.0-linux-x86_64.tar.gz<br>
$ tar -xzvf cmake-3.28.0-linux-x86_64.tar.gz

unpack the bip39scan source, let's say bip39scan/<br>
make an empty build directory, and run cmake in it e.g.<br>
$ mkdir bip39scan-build<br>
$ cd bip39scan-build

On first make, it will generate precomp.bin file, which may take quite some time. <br>
If you already have the precomp.bin, copy it to the build directory and comment this line in the ../bip39scan/CMakeLists.txt: add_dependencies(bip39scan precomp-bin) like this:<br>
#add_dependencies(bip39scan precomp-bin)

Save CMakeLists.txt and run cmake:
$ ../cmake-3.28.0-linux-x86_64/bin/cmake ../bip39scan<br>

where ../bip39scan is the source code directory<br>
make the project<br>
$ make bip39scan
