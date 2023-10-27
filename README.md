# M2-S4

# Building File system and Booting Kernel.


## Contents:
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
1. Kernel without File system.
2. Sending Filesystem to Kernel.
3. Defintion of Minimal rootfilesystem.
4. Content of Rootfilesystem.
5. `sudo` Permisions.
6. User space first process.
7. Programs and Utilities.
8. Libraries.
9. Merging `sysroot` with `rootfilesystem` using `rsync`
10. Virtual File systems [proc and sysfs].
11. Packaging rootfilesystem to target [initramfs - Disk Image - NFS].
12. Creating Uncompressed filesystem.
13. Compressing `initramfs` system using [mkinitfs - CPIO].
14. Booting Kernel with initramfs.
15. Init program [Process of reading configuration from /etc/inittab].
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 1. Kernel without filesystem 😱😱😱😱😱😱😱😱😱😱😱
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

```
qemu-system-aarch64 -M virt -cpu cortex-a53 -m 1G -kernel Image  -append "console=ttyAMA0"  -nographic
```
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
### 2. How Filesystem should look like
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
```
man hier
```

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
### 3. Defintion of Minimal rootfilesystem.
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
a. standard directories.
```
```

<pre>\u251c\u2500\u2500 <font color="#729FCF"><b>bin</b></font>
\u251c\u2500\u2500 <font color="#729FCF"><b>dev</b></font>
\u251c\u2500\u2500 <font color="#729FCF"><b>etc</b></font>
\u251c\u2500\u2500 <font color="#729FCF"><b>home</b></font>
\u251c\u2500\u2500 <font color="#729FCF"><b>lib</b></font>
\u251c\u2500\u2500 <font color="#729FCF"><b>lib64</b></font>
\u251c\u2500\u2500 <font color="#729FCF"><b>proc</b></font>
\u251c\u2500\u2500 <font color="#729FCF"><b>sbin</b></font>
\u251c\u2500\u2500 <font color="#729FCF"><b>sys</b></font>
\u251c\u2500\u2500 <font color="#729FCF"><b>tmp</b></font>
\u251c\u2500\u2500 <font color="#729FCF"><b>usr</b></font>
\u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>bin</b></font>
\u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>include</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>arpa</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>asm</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>asm-generic</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>bits</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>types</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>drm</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>finclude</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>gdb</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>gnu</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>linux</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>android</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>byteorder</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>caif</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>can</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>cifs</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>dvb</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>genwqe</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>hdlc</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>hsi</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>iio</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>isdn</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>misc</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>mmc</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>netfilter</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>ipset</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>netfilter_arp</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>netfilter_bridge</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>netfilter_ipv4</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>netfilter_ipv6</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>nfsd</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>raid</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>sched</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>spi</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>sunrpc</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>surface_aggregator</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>tc_act</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>tc_ematch</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>usb</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>misc</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>uacce</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>mtd</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>net</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>netash</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>netatalk</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>netax25</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>neteconet</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>netinet</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>netipx</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>netiucv</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>netpacket</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>netrom</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>netrose</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>nfs</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>protocols</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>rdma</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>hfi</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>rpc</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>scsi</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>fc</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>sound</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>intel</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>avs</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>sof</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>sys</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>video</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>xen</b></font>
\u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>lib</b></font>
\u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>lib64</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>audit</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>gconv</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0     \u2514\u2500\u2500 <font color="#729FCF"><b>gconv-modules.d</b></font>
\u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>libexec</b></font>
\u2502\u00a0\u00a0 \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>getconf</b></font>
\u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>sbin</b></font>
\u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>share</b></font>
\u2502\u00a0\u00a0     \u251c\u2500\u2500 <font color="#729FCF"><b>i18n</b></font>
\u2502\u00a0\u00a0     \u2502\u00a0\u00a0 \u251c\u2500\u2500 <font color="#729FCF"><b>charmaps</b></font>
\u2502\u00a0\u00a0     \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>locales</b></font>
\u2502\u00a0\u00a0     \u2514\u2500\u2500 <font color="#729FCF"><b>locale</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>be</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>bg</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>ca</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>cs</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>da</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>de</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>el</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>en_GB</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>eo</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>es</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>fi</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>fr</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>gl</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>hr</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>hu</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>ia</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>id</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>it</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>ja</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>ka</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>ko</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>lt</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>nb</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>nl</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>pl</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>pt</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>pt_BR</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>ro</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>ru</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>rw</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>sk</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>sl</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>sr</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>sv</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>tr</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>uk</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>vi</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u251c\u2500\u2500 <font color="#729FCF"><b>zh_CN</b></font>
\u2502\u00a0\u00a0         \u2502\u00a0\u00a0 \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2502\u00a0\u00a0         \u2514\u2500\u2500 <font color="#729FCF"><b>zh_TW</b></font>
\u2502\u00a0\u00a0             \u2514\u2500\u2500 <font color="#729FCF"><b>LC_MESSAGES</b></font>
\u2514\u2500\u2500 <font color="#729FCF"><b>var</b></font>
    \u251c\u2500\u2500 <font color="#729FCF"><b>db</b></font>
    \u2514\u2500\u2500 <font color="#729FCF"><b>log</b></font>
</pre>
a. Init process.
b. Shell.
c. Commands.
d. Applications.

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


