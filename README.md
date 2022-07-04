# Tutorial Run Aptos Testnet
Tutorial simpel RUN Testnet Aptos di baca baik-baik biar gak ada error. 

# Update packages

```
sudo apt update && sudo apt upgrade -y
```
```
sudo apt-get install jq unzip -y
```
# Install screen
```
sudo apt install screen
```
# install git
```
apt install git
```
# Install cargo
```
apt install cargo
```
# Download Aptos Core
```
git clone https://github.com/aptos-labs/aptos-core.git
```
# Masuk ke folder
```
cd aptos-core
```
# Lalu install
```
./scripts/dev_setup.sh
```
```
source ~/.cargo/env
```
# Setup validator
```
git checkout --track origin/testnet
```
```
export WORKSPACE=testnet
mkdir ~/$WORKSPACE
```
# Generate key pairs
```
cargo run --release -p aptos -- genesis generate-keys --output-dir ~/$WORKSPACE
```
Nanti ada 3 file `private-keys.yaml` `validator-identity.yaml` `validator-full-node-identity.yaml` kalian simpan semua isinya buat isi form di https://community.aptoslabs.com/
Untuk menyimpan gunakan perintah di bawah
```
nano /root/testnet/private-keys.yaml
```
```
nano /root/testnet/validator-identity.yaml
```
```
nano /root/testnet/validator-full-node-identity.yaml
```
# Konfigurasi validator
```
cargo run --release -p aptos -- genesis set-validator-configuration \
    --keys-dir ~/$WORKSPACE --local-repository-dir ~/$WORKSPACE \
    --username xxxxxxxxxxx\
    --validator-host IPVPS:6180 \
    --full-node-host IPVPS:6182
```
ganti `xxxxxxxxxxx` dan `IPVPS` dengan nama node dan ip vps kalian
Contoh di bawah
```
cargo run --release -p aptos -- genesis set-validator-configuration \
    --keys-dir ~/$WORKSPACE --local-repository-dir ~/$WORKSPACE \
    --username furi\
    --validator-host 35.232.235.205:6180 \
    --full-node-host 34.135.169.144:6182
```
# buat yaml file dengan username kalian
```
sudo nano /root/testnet/usernamekalian.yaml
```
**setelah itu buat file* `layout.yaml`
```
nano ~/$WORKSPACE/layout.yaml
```
# Paste ini di dalam file `layout.yaml`
```
---
root_key: "F22409A93D1CD12D2FC92B5F8EB84CDCD24C348E32B3E7A720F3D2E288E63394"
users:
  - "xxxxxx"
chain_id: 40
min_stake: 0
max_stake: 100000
min_lockup_duration_secs: 0
max_lockup_duration_secs: 2592000
epoch_duration_secs: 86400
initial_lockup_timestamp: 1656615600
min_price_per_gas_unit: 1
allow_new_validators: true
```
# ganti `xxxxxx` dengan nama node/username kalian
contoh di bawah
```
---
root_key: "F22409A93D1CD12D2FC92B5F8EB84CDCD24C348E32B3E7A720F3D2E288E63394"
users:
  - "furi"
chain_id: 40
min_stake: 0
max_stake: 100000
min_lockup_duration_secs: 0
max_lockup_duration_secs: 2592000
epoch_duration_secs: 86400
initial_lockup_timestamp: 1656615600
min_price_per_gas_unit: 1
allow_new_validators: true
```
# Selanjutnya copy paste perintah di bawah satu-satu
```
cargo run --release --package framework -- --package aptos-framework --output current
mkdir ~/$WORKSPACE/framework
```
```
mv aptos-framework/releases/artifacts/current/build/**/bytecode_modules/*.mv ~/$WORKSPACE/framework/
```
```
mv aptos-framework/releases/artifacts/current/build/**/bytecode_modules/dependencies/**/*.mv ~/$WORKSPACE/framework/
```

# Kompilasi genesis blob dan waypoint dengan perintah ini
```
cargo run --release -p aptos -- genesis generate-genesis --local-repository-dir ~/$WORKSPACE --output-dir ~/$WORKSPACE
```
# Ini akan membuat dua file, `genesis.blob` dan `waypoint.txt` di direktori kalian.

Dan sekarang salin file validator.yaml, fullnode.yaml ke direktori ini dengan perintah di bawah:
```
mkdir ~/$WORKSPACE/config
cp docker/compose/aptos-node/validator.yaml ~/$WORKSPACE/validator.yaml
cp docker/compose/aptos-node/fullnode.yaml ~/$WORKSPACE/fullnode.yaml
```
# buka file config kalian
```
nano /root/testnet/validator.yaml
```
sekarang ganti folder dimana kalian menyimpan file `key`, `genesis file`, `waypoint`
![Screenshot_33](https://user-images.githubusercontent.com/63885192/177060340-7de70ba9-a995-40ef-8984-575000156e0e.png)

```
/root/testnet/
```
# Setelah selesai Lanjut buat screen session dengan nama bebas contoh `aptos-core`
```
screen -S aptos-core
```
```
cargo run -p aptos-node --release -- -f ~/testnet/validator.yaml
```
kalau muncul Logs artinya node sudah berjalan, close tekan Ctrl+A then D close putty

Chcek IP VPS kalian disini https://aptos-node.info/
![Screenshot_34](https://user-images.githubusercontent.com/63885192/177060451-f35eb797-8d54-4291-a9be-b7e9158cee3d.png)
jika sudah seperti di screenshoot atas silahkan daftarkan node kalian disini https://community.aptoslabs.com/

Terimakasih.

