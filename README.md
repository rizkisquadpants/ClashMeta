
<h1 align="center">
  <img src="https://raw.githubusercontent.com/MetaCubeX/Clash.Meta/Meta/Meta.png" alt="Clash" width="200">
  <br>Config Premium OpenClash Meta OpenWrt<br>

</h1>

  <p align="center">
  <a target="_blank" href="https://github.com/rizkisquadpants/OpenClash/archive/refs/heads/main.zip">
    <img src="https://img.shields.io/github/v/release/rizkisquadpants/ClashMeta?style=for-the-badge&label=Download">
  </a>
  </p>
  

<p align="center">
Config OpenClash OpenWrt Meta Spesial Edition
</p>
<p align="center">
Support Multi-WAN, Pisah Traffic, Bypass Traffic, Support Game, Support Streaming
</p>
<p align="center">
- Terimakasih <a href="https://github.com/vernesong/OpenClash" target="_blank"> Vernesong </a> For Clash OpenWrt ，<a href="https://github.com/malikshi" target="_blank"> Maliksih </a> Untuk Rule Provider ，<a href="https://www.facebook.com/r3yr3" target="_blank"> Reyre </a>Best Firmware  ，Dan <a href="https://howdy.id/" target="_blank"> Howdy </a>The Best Server -
</p>

**Konten Tabel**
  
  - [Openclash](#openclash)
    - [Setting OpenClash](#global-setting)
    - [Edit Files Proxy Provider](#edit-files-proxy-provider)
    - [Multi-WAN ISP](#multi-wan-isp)
    - [Edit Files Rule Provider](#edit-files-rule-provider)
    - [Setting Yacd](#setting-yacd)


## Openclash

Plugin ini adalah klien Clash yang bisa dijalankan di OpenWrt. Kompatibel dengan Shadowsocks ShadowsocksR, Vmess, Trojan, Snell dan protokol lainnya, dan mengimplementasikan proxy kebijakan sesuai dengan konfigurasi aturan yang fleksibel.

 <img src="https://raw.githubusercontent.com/rizkisquadpants/ClashMeta/main/assets/Main.png">

### Setting OpenClash

## Global Setting

Hasil settingan pada global setting akan meng-overide settingal awal pada file main.yaml.

### Operation Mode

* Ceklist/centang opsi sesuai gambar berikut:

<img src="https://raw.githubusercontent.com/rizkisquadpants/ClashMeta/main/assets/Opration.PNG" border="0">


### DNS Setting

* Ceklist/centang opsi sesuai gambar berikut:

<img src="https://raw.githubusercontent.com/rizkisquadpants/ClashMeta/main/assets/DNS.PNG" border="0">

* Isi Fallback-filter

Perlu diedit isi fallback-filternya supaya mendapatkan hostname di Yacd Connection log. isi fallback-filter dengan ini.

```yaml
fallback-filter:
  geoip: true
  geoip-code: ID
  ipcidr:
    - 0.0.0.0/8
    - 10.0.0.0/8
    - 100.64.0.0/10
    - 127.0.0.0/8
    - 169.254.0.0/16
    - 172.16.0.0/12
    - 192.0.0.0/24
    - 192.0.2.0/24
    - 192.88.99.0/24
    - 192.168.0.0/16
    - 198.18.0.0/15
    - 198.51.100.0/24
    - 203.0.113.0/24
    - 224.0.0.0/4
    - 240.0.0.0/4
    - 255.255.255.255/32
  domain:
    - "+.google.com"
    - "+.facebook.com"
    - "+.youtube.com"
    - "+.githubusercontent.com"
    - "+.googlevideo.com"
    - "+.msftconnecttest.com"
    - "+.msftncsi.com"
    - msftconnecttest.com
    - msftncsi.com
    - "+.*"
```
* Custom DNS

Perlu diedit isi Name Server DNS Sesua Seperti Dibawah Ini
<img src="https://raw.githubusercontent.com/rizkisquadpants/ClashMeta/main/assets/CustomDNS.png" border="0">


### Meta Setting

Jangan Lupa Aktifkan Meta Pada Meta Settings Dan Sesuaikan Seperti Pada Gambar
<img src="https://raw.githubusercontent.com/rizkisquadpants/ClashMeta/main/assets/Meta.png" border="0">

### Edit Files Proxy Provider


Mengisi akun tunnel pada 3 files pada folder proxy_provider yang terdiri dari `pp-gaming.yaml`, `pp-indo.yaml`, dan `pp-browsing.yaml`.
Fungsi dari proxy_provider diatas:
* pp-gaming.yaml, Gunakan akun yang memiliki ping atau latency rendah seperti trojan,shadowsocks,grPC,Snell.
* pp-indo.yaml, Untuk Akun berlokasi INDONESIA untuk keperluan akses websites/marketplace/live stream apps/Video on Demand yang mengharuskan memakai IP Address Publik Indonesia.
* pp-browsing.yaml, Untuk Akun Browsing Rekomendasi SGGS


### Multi-WAN ISP

Default Confing Menggunakan 2 ISP, Untung Pengguna 1 ISP Silahkan Edit Config `CONFIG-RTA-SERVER.yaml`,Kemudian Edit Sedikit Config Tersebut Untuk Merubah `interface-name` Sesuai Interface yg Dipakai, Default yg Dipakai `eth1` dan `eth2` . Mengecek nama interface bisa dari LuCI > Network > Interface atau bisa jalankan commandline `ifconfig`.
Setelah memperoleh nama interface dari tiap WAN suudah di tentukan maka tinggal cara settingnya sebagai berikut:
* Edit file CONFIG-RTA-SERVER.yaml.

    Edit `interface-name: ` pada list proxy-groups dengan mengisikan nama interface WAN dari modem kalian. Dan perlu diperhatikan jika hanya menggunakan 1 WAN saja silahkan edit seperti berikut.
    
    ```
    proxy-groups dari DIRECT ISP1.
    ```yaml
      - name: DIRECT ISP1
        type: url-test
        disable-udp: false
        proxies:
        - DIRECT
        url: http://www.gstatic.com/generate_204
        interval: '30'
        interface-name: eth1
    ```
    proxy-groups dari DIRECT ISP2.
    ```yaml
      - name: DIRECT ISP2
        type: url-test
        disable-udp: false
        proxies:
        - DIRECT
        url: http://www.gstatic.com/generate_204
        interval: '30'
        interface-name: eth2
    ```
	
	proxy-groups dari DIRECT ISP2. Jika Menggunakan 1 ISP Dan Hapus Juga Pada Bagian Di Bawah Ini
	
	```yaml
      - name: ✔️ Direct Mode
		type: select
		disable-udp: false
		proxies:
		- DIRECT ISP1
		- DIRECT ISP2 #Hapus Baris Ini
		- 🔰 Browsing 🔰
    ```
	```yaml
	  - name: 🎮 Gaming UDP
		type: select
		disable-udp: false
		proxies:
		- DIRECT ISP1
		- DIRECT ISP2 #Hapus Baris Ini
		use:
		- PP-Gaming
		url: http://www.gstatic.com/generate_204
		interval: '300'
	```

* Edit Setiap file pada folder Proxy_Provider

    Sebagai Contoh Akun Trojan Untuk LB 2 ISP
    
    ISP1
    ```yaml
    - name: "ID BLABLA ISP1"
      type: trojan
      server: aaa.bbb.ccc.ddd
      port: 443
      password: PASSWORD
      udp: true
      sni: BUGSNI.COM
      alpn:
        - h2
        - http/1.1
      skip-cert-verify: true
      interface-name: eth1
    
	ISP2
    - name: "ID BLABLA ISP2"
      type: trojan
      server: aaa.bbb.ccc.ddd
      port: 443
      password: PASSWORD
      udp: true
      sni: BUGSNI.COM
      alpn:
        - h2
         - http/1.1
      skip-cert-verify: true
      interface-name: eth2
    ```

### Edit Files Rule Provider

Untuk `rp-direct.yaml` bersifat offline Bisa Di Edit Sebagai Contoh Direct Whatsapp


### Setting Yacd

Yacd adalah yet another clash dashboard, yakni dashboard clash yang dapat digunakan untuk mengatur proxy dan memantau services clash yang dijalankan oleh Openclash.

#### Proxies

Untuk pertama kali start openclash maka harus setting proxies `GLOBAL` ke proxy-groups `🔰 Browsing 🔰`.
 <img src="https://raw.githubusercontent.com/rizkisquadpants/ClashMeta/main/assets/Yacd.png">
