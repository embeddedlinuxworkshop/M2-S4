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

<font color="#729FCF"><b>bin</b></font>
 <font color="#729FCF"><b>dev</b></font>
 <font color="#729FCF"><b>etc</b></font>
 <font color="#729FCF"><b>home</b></font>
 <font color="#729FCF"><b>lib</b></font>
 <font color="#729FCF"><b>lib64</b></font>
 <font color="#729FCF"><b>proc</b></font>
 <font color="#729FCF"><b>sbin</b></font>
 <font color="#729FCF"><b>sys</b></font>
 <font color="#729FCF"><b>tmp</b></font>
 <font color="#729FCF"><b>usr</b></font>
  <font color="#729FCF"><b>bin</b></font>
  <font color="#729FCF"><b>include</b></font>
   <font color="#729FCF"><b>arpa</b></font>
   <font color="#729FCF"><b>asm</b></font>
   <font color="#729FCF"><b>asm-generic</b></font>
   <font color="#729FCF"><b>bits</b></font>
    <font color="#729FCF"><b>types</b></font>
   <font color="#729FCF"><b>drm</b></font>
   <font color="#729FCF"><b>finclude</b></font>
   <font color="#729FCF"><b>gdb</b></font>
   <font color="#729FCF"><b>gnu</b></font>
   <font color="#729FCF"><b>linux</b></font>
    <font color="#729FCF"><b>android</b></font>
    <font color="#729FCF"><b>byteorder</b></font>
    <font color="#729FCF"><b>caif</b></font>
    <font color="#729FCF"><b>can</b></font>
    <font color="#729FCF"><b>cifs</b></font>
    <font color="#729FCF"><b>dvb</b></font>
    <font color="#729FCF"><b>genwqe</b></font>
    <font color="#729FCF"><b>hdlc</b></font>
    <font color="#729FCF"><b>hsi</b></font>
    <font color="#729FCF"><b>iio</b></font>
    <font color="#729FCF"><b>isdn</b></font>
    <font color="#729FCF"><b>misc</b></font>
    <font color="#729FCF"><b>mmc</b></font>
    <font color="#729FCF"><b>netfilter</b></font>
     <font color="#729FCF"><b>ipset</b></font>
    <font color="#729FCF"><b>netfilter_arp</b></font>
    <font color="#729FCF"><b>netfilter_bridge</b></font>
    <font color="#729FCF"><b>netfilter_ipv4</b></font>
    <font color="#729FCF"><b>netfilter_ipv6</b></font>
    <font color="#729FCF"><b>nfsd</b></font>
    <font color="#729FCF"><b>raid</b></font>
    <font color="#729FCF"><b>sched</b></font>
    <font color="#729FCF"><b>spi</b></font>
    <font color="#729FCF"><b>sunrpc</b></font>
    <font color="#729FCF"><b>surface_aggregator</b></font>
    <font color="#729FCF"><b>tc_act</b></font>
    <font color="#729FCF"><b>tc_ematch</b></font>
    <font color="#729FCF"><b>usb</b></font>
   <font color="#729FCF"><b>misc</b></font>
    <font color="#729FCF"><b>uacce</b></font>
   <font color="#729FCF"><b>mtd</b></font>
   <font color="#729FCF"><b>net</b></font>
   <font color="#729FCF"><b>netash</b></font>
   <font color="#729FCF"><b>netatalk</b></font>
   <font color="#729FCF"><b>netax25</b></font>
   <font color="#729FCF"><b>neteconet</b></font>
   <font color="#729FCF"><b>netinet</b></font>
   <font color="#729FCF"><b>netipx</b></font>
   <font color="#729FCF"><b>netiucv</b></font>
   <font color="#729FCF"><b>netpacket</b></font>
   <font color="#729FCF"><b>netrom</b></font>
   <font color="#729FCF"><b>netrose</b></font>
   <font color="#729FCF"><b>nfs</b></font>
   <font color="#729FCF"><b>protocols</b></font>
   <font color="#729FCF"><b>rdma</b></font>
    <font color="#729FCF"><b>hfi</b></font>
   <font color="#729FCF"><b>rpc</b></font>
   <font color="#729FCF"><b>scsi</b></font>
    <font color="#729FCF"><b>fc</b></font>
   <font color="#729FCF"><b>sound</b></font>
    <font color="#729FCF"><b>intel</b></font>
     <font color="#729FCF"><b>avs</b></font>
    <font color="#729FCF"><b>sof</b></font>
   <font color="#729FCF"><b>sys</b></font>
   <font color="#729FCF"><b>video</b></font>
   <font color="#729FCF"><b>xen</b></font>
  <font color="#729FCF"><b>lib</b></font>
  <font color="#729FCF"><b>lib64</b></font>
   <font color="#729FCF"><b>audit</b></font>
   <font color="#729FCF"><b>gconv</b></font>
       <font color="#729FCF"><b>gconv-modules.d</b></font>
  <font color="#729FCF"><b>libexec</b></font>
   <font color="#729FCF"><b>getconf</b></font>
  <font color="#729FCF"><b>sbin</b></font>
  <font color="#729FCF"><b>share</b></font>
      <font color="#729FCF"><b>i18n</b></font>
       <font color="#729FCF"><b>charmaps</b></font>
       <font color="#729FCF"><b>locales</b></font>
      <font color="#729FCF"><b>locale</b></font>
          <font color="#729FCF"><b>be</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>bg</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>ca</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>cs</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>da</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>de</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>el</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>en_GB</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>eo</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>es</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>fi</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>fr</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>gl</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>hr</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>hu</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>ia</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>id</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>it</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>ja</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>ka</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>ko</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>lt</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>nb</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>nl</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>pl</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>pt</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>pt_BR</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>ro</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>ru</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>rw</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>sk</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>sl</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>sr</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>sv</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>tr</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>uk</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>vi</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>zh_CN</b></font>
           <font color="#729FCF"><b>LC_MESSAGES</b></font>
          <font color="#729FCF"><b>zh_TW</b></font>
              <font color="#729FCF"><b>LC_MESSAGES</b></font>
 <font color="#729FCF"><b>var</b></font>
     <font color="#729FCF"><b>db</b></font>
     <font color="#729FCF"><b>log</b></font>
</pre>
a. Init process.
b. Shell.
c. Commands.
d. Applications.

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


