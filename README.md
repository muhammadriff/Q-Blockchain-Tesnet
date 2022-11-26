# Tutorial Q-Blockchain-Tesnet

## Referensi

[Faucet](https://faucet.qtestnet.org/)

[Leaderboard validator](https://stats.qtestnet.org/)

[docs qtestnet](https://docs.qtestnet.org/how-to-setup-validator/)

[Explorer](https://explorer.qtestnet.org/)

[discord](https://discord.gg/UMJJqE2mwx)

## Spesifikasi Minimum
| Komponen | Spesifikasi minimal |
|----------|---------------------|
|CPU|Intel Core i3 or i5|
|RAM|4 GB DDR4 RAM|
|Penyimpanan|500 GB HDD|
|Koneksi|100 Mbit/s port|

| Komponen | Spesifikasi rekomendasi |
|----------|---------------------|
|CPU|Intel Core i7-8700 Hexa-Core|
|RAM|64 GB DDR4 RAM|
|Penyimpanan|2 x 1 TB NVMe SSD|
|Koneksi|1 Gbit/s port|

### Persyaratan perangkat lunak

| Komponen | Spesifikasi minimal |
|----------|---------------------|
|Sistem Operasi|Ubuntu 16.04|

| Komponen | Spesifikasi rekomendasi |
|----------|---------------------|
|Sistem Operasi|Ubuntu 18.04 atau lebih tinggi|

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

Public address of the key:   0xb3FF24F818b0ff6Cc50de951bcB8f86b52287dac
Path of the secret key file: /data/keystore/UTC--2021-01-18T11-36-28.705754426Z--b3ff24f818b0ff6cc50de951bcb8f86b52287dac

- You can share your public address with anyone. Others need it to interact with you.
- You must NEVER share the secret key with anyone! The key controls access to your funds!
- You must BACKUP your key file! Without the key, it's impossible to access account funds!
- You must REMEMBER your password! Without the password, it's impossible to decrypt the key!
```

**Noted** NotedBackup wallet anda!

### CLaim faucet

Pergi ke [sini](https://faucet.qtestnet.org/) lalu masukan alamat dompet yang tadi anda buat dan klaim Q Token

> jika terjadi error maka ulangi terus sampai dapat, anda juga bisa klaim token yang lain

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


### Perintah-Perintah Berguna!
gunakan di direktori `testnet-public-tools/testnet-validator` | `testnet-public-tools/Root-Node` | `testnet-public-tools/testnet-fullnode`

Restart node
```
docker-compose restart
```

stop node
```
docker-compose stop
```


### Art-Team INFO
noted: ***art team*** here does not have any specific goals or intentions, they only collect data and share it with everyone.

untuk INFO Testnet lainya Silahkan join Discord 👇

[![twitter](https://img.shields.io/badge/twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white)](https://twitter.com/ArtSy5team)
[![Discord](https://img.shields.io/badge/discord-7289d9?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/EAKEdZU6c8)
[![Github](https://img.shields.io/badge/GitHub-171515?style=for-the-badge&logo=GitHub&logoColor=white)](https://github.com/Art-Sy5team)
[![trakteer](https://img.shields.io/badge/trakteer.id-e31e1e?style=for-the-badge&logo=ko-fi&logoColor=white)](https://trakteer.id/Art-Sy5team/tip)

