# Tutorial Q-Blockchain-Tesnet

[Faucet](https://faucet.qtestnet.org/)

[Leaderboard validator](https://stats.qtestnet.org/)

[docs qtestnet](https://docs.qtestnet.org/how-to-setup-validator/)

[Explorer](https://explorer.qtestnet.org/)

[discord](https://discord.gg/UMJJqE2mwx)

| Komponen | Spesifikasi rekomendasi |
|----------|---------------------|
|Sistem Operasi|Ubuntu 16.04 atau lebih tinggi|
|CPU|Intel | Core i3 atau i5|
|  RAM   |  4 GB |
|Penyimpanan|2 x 1 TB NVMe SSD|

## Pasang dan Jalankan validator

### Pasang dependensi

```
apt install git docker docker-compose
```

### Unduh paket Q-Blockchain

```
git clone https://gitlab.com/q-dev/testnet-public-tools.git
```

### Masuk ke folder `testnet-validator`

```
cd testnet-public-tools/testnet-validator
```

### Membuat kata sandi wallet

Kata sandi harus disimpan di folder `keystore`, jadi anda perlu membuat foldernya terlebih dahulu dengan perintah

```
mkdir keystore
```

Lalu buat file `pwd.txt` dan isi dengan kata sandi anda (kata sandi bebas)

```
nano keystore/pwd.txt
```

Setelah memasukan kata sandi, tekan <kbd>CTRL</kbd> + <kbd>x</kbd> + <kbd>Y</kbd> lalu tekan <kbd>Enter</kbd>

Kemudian cek apakah kata sandi telah tersimpan

```
cat keystore/pwd.txt
```

> Jika kata sandi yang anda buat tadi tampil ke layar artinya kata sandi telah berhasil dibuat

### Buat dompet

```
docker run --entrypoint="" --rm -v $PWD:/data -it qblockchain/q-client:testnet geth account new --datadir=/data --password=/data/keystore/pwd.txt
```

Jika berhasil maka pesan seperti ini akan tampil di layar

```
Your new key was generated

Public address of the key:   0x6B746aBE88aee41aC5c8484576bB0f3339400744
Path of the secret key file: /data/keystore/UTC--2022-11-19T19-13-03.508785146Z--6b746abe88aee41ac5c8484576bb0f3339400744

- You can share your public address with anyone. Others need it to interact with you.
- You must NEVER share the secret key with anyone! The key controls access to your funds!
- You must BACKUP your key file! Without the key, it's impossible to access account funds!
- You must REMEMBER your password! Without the password, it's impossible to decrypt the key!
```

**Noted** Backup wallet anda!

### CLaim faucet

Pergi ke [sini](https://faucet.qtestnet.org/) lalu masukan alamat dompet yang tadi anda buat dan klaim Q Token

 jika terjadi error maka ulangi terus sampai dapat, anda juga bisa klaim token yang lain

### Konfigurasi `.env`

Jalankan perintah ini

```
nano .env
```

Lalu ganti variabel dibawah

| Variabel | Keterangan |
|----------|------------|
|ADDRESS|Ganti dengan alamat dompet tadi lalu hilangkan 0x)|
|IP|Ganti dengan alamat IP VPS anda (yang digunakan untuk login VPS)|

Setelah itu tekan <kbd>CTRL</kbd> + <kbd>x</kbd> + <kbd>Y</kbd> lalu tekan <kbd>Enter</kbd>

### Konfigurasi `config.json`

Jalankan perintah ini

```
nano config.json
```

Lalu ganti address dan password dengan alamat dompet dan kata sandi anda

Setelah itu tekan <kbd>CTRL</kbd> + <kbd>x</kbd> + <kbd>Y</kbd> lalu tekan <kbd>Enter</kbd>

### Taruh stake di validator contract

```
docker run --rm -v $PWD:/data -v $PWD/config.json:/build/config.json qblockchain/js-interface:testnet validators.js
```

### Daftarkan validator

Jalankan perintah ini

```
nano docker-compose.yaml
```

lalu dibagian `entrypoint` setelah `geth` tambahkan ini

```
"--ethstats=NAMA_VALIDATOR:qstats-testnet@stats.qtestnet.org",
```

Ganti nama validator anda bebas! `NAMA_VALIDATOR` 

Setelah itu tekan <kbd>CTRL</kbd> + <kbd>x</kbd> + <kbd>Y</kbd> lalu tekan <kbd>Enter</kbd>

### Jalankan validator

Jalankan perintah dibawah untuk menjalankan validator

```
docker-compose up -d
```

Lalu cek log

```
docker-compose logs -f --tail "100"
```

`CTRL + F` Cari nama validator anda [![disini](https://img.shields.io/static/v1?label=HERE&message=NODE-LIST&color=9cf)](https://stats.qtestnet.org/)

## Tambah Peer (jika node merah atau peers 0)
ke file directory `testnet-validator`
```
cd testnet-public-tools/testnet-validator
```
### add peers
command 1 
```
docker compose exec testnet-validator-node geth attach /data/geth.ipc
```
command 2 (gunakan ini jika command 1 eror)
```
docker-compose exec testnet-validator-node geth attach /data/geth.ipc
```
Jika sudah masuk di `data geth.ipc` seperi contoh di bawah, masukan satu-satu **list peers**

[![pers.png](https://i.postimg.cc/qRn4nrHL/pers.png)](https://postimg.cc/xJ0wDWCJ)

### list peers 

Team ART mengumpulkan ini dari discord Q-blockhain & peers team sendiri, jika punya peers yg lain silahkan tambahkan.

`
admin.addPeer('enode://c610793186e4f719c1ace0983459c6ec7984d676e4a323681a1cbc8a67f506d1eccc4e164e53c2929019ed0e5cfc1bc800662d6fb47c36e978ab94c417031ac8@79.125.97.227:30304')
admin.addPeer('enode://8eff01a7e5a66c5630cbd22149e069bbf8a8a22370cef61b232179e21ba8c7b74d40e8ee5aa62c54d145f7fc671b851e5ccbfe124fce75944cf1b06e29c55c80@79.125.97.227:30305')
admin.addPeer('enode://7a8ade64b79961a7752daedc4104ca4b79f1a67a10ea5c9721e7115d820dbe7599fe9e03c9c315081ccf6a2afb0b6652ee4965e38f066fe5bf129abd6d26df58@79.125.97.227:30306')
admin.addPeer('enode://780d33fb34efbcf97d0091f7d3b0337d7f27d8bf5d00358e47ffee0b7b27356f63128ac9dc7131c9bce155038eadcfac275e34158a35650cfd1fcb16fb92a405@134.122.71.55:30313')
admin.addPeer('enode://aa72b8309fc09728e9516c92c4a37944a29267cbc45a6e9fac807bb020808f8d61abaf86a510aa28fa7bc288f0ef1613a6785e74963203d6aeef01aaef1bfec1@5.75.140.251:30313')
admin.addPeer('enode://9efb0f552e9d9217843f5d143bb32dbd8cf7bf4073e601dce20510fc9a4a1e728fcd8658078db19f79ccef14cd45f54743d90fa1e64a2365fc5d83d307457018@65.108.74.190:61439')
admin.addPeer('enode://c610793186e4f719c1ace0983459c6ec7984d676e4a323681a1cbc8a67f506d1eccc4e164e53c2929019ed0e5cfc1bc800662d6fb47c36e978ab94c417031ac8@79.125.97.227:53820')
admin.addPeer('enode://5510070fd564e8930705f1a79a7d290efbc9d18743de9fb27872acdcd0775d1a65cad4851e7855ec455cfb48e2f0caa48a6d300d540dd08f42b462615c12e661@159.223.22.223:58658')
admin.addPeer('enode://a3862c64d5d7e43f8cffce810d4ad00d20c2b34524df20be5c3e9a91f157a7ca532068e358e690453e3f339d5bdeab6a922d7a2a6243d4c89d4b64304360de1e@18.158.211.67:30303')
admin.addPeer('enode://9960a6b7fcc6bb46f89575906d8ef68d7b3f924c9f9a206afd8e9576936f9f6176656296f530f19ca4a61e723a1515966fd6baa2391e7555a46855ac94059bd1@80.158.48.29:4452')
admin.addPeer('enode://81cdc63a97e7e687aa8aa28b3d4c5b72f94ddb856956e01ea5943f960a12fa0ea536a3366cefa7a522377ded79b243e9accc65d7f159762bc6971aa317850c5b@18.202.219.33:30303')
admin.addPeer('enode://7a8ade64b79961a7752daedc4104ca4b79f1a67a10ea5c9721e7115d820dbe7599fe9e03c9c315081ccf6a2afb0b6652ee4965e38f066fe5bf129abd6d26df58@79.125.97.227:49128')
admin.addPeer('enode://8eff01a7e5a66c5630cbd22149e069bbf8a8a22370cef61b232179e21ba8c7b74d40e8ee5aa62c54d145f7fc671b851e5ccbfe124fce75944cf1b06e29c55c80@79.125.97.227:54000')
admin.addPeer('enode://9127cf39043562e0600b8baa7e192fd92bdb3bff573288312aa7e0336c79a4000844e8e8580e8efd64fbd99ade341bae1ea21835e4d1a777e579304b66b5acaf@185.170.115.95:30313')
admin.addPeer('enode://37793ae0218500896ad2fb0a2aff634e29198b9729f5a83d52963b941f8f0af938a72cb4b5d48cb201a2c63e7903f1d35ea8827a38d4f7f7dc5b4100e8645f50@35.156.94.184:52350')
admin.addPeer('enode://9963c9a837719cd8c802b14eb510f060994f5d4f1bf36796ab6287f0a3886fcccb6c74cb90f7a63ff982d0c6adca112706632c17cdc8660f6089f9bf5300f5a9@80.158.48.29:61275')
admin.addPeer('enode://3b352d3349bfb0d1fac0fd703d0cc2e397fe9910d6a5fbe5077ff1ec46c50b261704d28de5e29c768e425b99f8acff605d00a0cf602425fac2a36a1505a26efb@36.69.120.50:53434')
admin.addPeer('enode://08cf0b8ce2194c327937a22b9706ce9dc8b061371a9744f5d5f0cdd5694f6e239f45bec218b9e9490f96253ccc921b5ee568c86558a2b56802ef4be3f6c9dc29@80.158.48.29:61278')
admin.addPeer('enode://e974d9354ababd356a6bfecbb03a59d14ab715ffa02d431c6accfc5de250e9c8c345817bd5687c119a04df78f1a4673e97877ea5775fa84270d311dac4a2eca7@128.199.213.70:30313')
admin.addPeer('enode://1032c556fbbfe37761951a20c2b98b4031234a8f871cc79dd8ff612a3e0436afe3458b325d2f25617b62134cfc8a8a4885e80c9760ecb4bb7c8deaee67a098ae@95.217.169.172:30303')
admin.addPeer('enode://b1d45b83e8ca1e1bb7ec71a59d3f02d67ab56175e068042e41c446f6445c8b0c677d0b34a454c1214081ea047a812156e55c35779c2c3528db84c91fde5688e5@149.102.137.123:30313')
`

Jika sudah selesai `ctrl + D` push atau restart node command di bawah


### Perintah-Perintah Berguna!
gunakan di direktori `testnet-public-tools/testnet-validator` | `testnet-public-tools/Root-Node` | `testnet-public-tools/testnet-fullnode`

```
docker-compose up -d
```
Restart node
```
docker-compose restart
```

stop node
```
docker-compose stop
```

### Backup Wallet 
Cara Pertama
- jika anda menggunakan Mobaxterms silahkan ke directory folder
<kbd>root/testnet-public-tools/testnet-validator/keystore/</kbd>
- folder dengan awalan UTC download dan simpan

### Art-Team INFO
noted: ***art team*** here does not have any specific goals or intentions, they only collect data and share it with everyone.

untuk INFO Testnet lainya Silahkan join Discord 👇

[![twitter](https://img.shields.io/badge/twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white)](https://twitter.com/ArtSy5team)
[![Discord](https://img.shields.io/badge/discord-7289d9?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/EAKEdZU6c8)
[![Github](https://img.shields.io/badge/GitHub-171515?style=for-the-badge&logo=GitHub&logoColor=white)](https://github.com/Art-Sy5team)
[![trakteer](https://img.shields.io/badge/trakteer.id-e31e1e?style=for-the-badge&logo=ko-fi&logoColor=white)](https://trakteer.id/Art-Sy5team/tip)

