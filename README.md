# Senkronize Web Sunucuları 

Bu proje, iki web sunucusunun senkronize bir şekilde çalışmasını ve her iki sunucuda da Apache web sunucusunun yapılandırılmasını sağlar. Ayrıca, dosya senkronizasyonu için `rsync` kullanarak içeriklerin otomatik olarak güncellenmesini sağlar.


## Adım 1: Apache Web Sunucusunu Kurma

### 1.1. Apache'yi Kurma

Her iki sunucuda da Apache web sunucusunu kurun:

```bash
sudo apt update
sudo apt install apache2
sudo systemctl start apache2
sudo systemctl enable apache2
```

## Adım 2: Web Sunucuları Arasında Dosya Senkronizasyonu İçin rsync Kurulumu

### 2.1. rsync'yi Kurma

```
sudo apt update
sudo apt install rsync
```

### 2.2. SSH Anahtarları ile Yetkilendirme

Anahtar Oluşturma (ilk sunucuda):

```
ssh-keygen -t rsa
```

Anahtar Kopyalama:

```
ssh-copy-id username@remote_server_ip
```

## Adım 3: Web İçeriklerinin Senkronizasyonu

### 3.1. rsync Komutunu Kullanma

Web içeriklerini (örneğin /var/www/html dizinini) senkronize etmek için rsync komutunu kullanın:

İlk Sunucuda (ana sunucu) rsync ile Senkronizasyon:

```
rsync -avz --delete /var/www/html/ username@remote_server_ip:/var/www/html/
```

Diğer Sunucuda Senkronizasyon:

```
rsync -avz --delete /var/www/html/ username@other_server_ip:/var/www/html/
```

### 3.2. .pem İle Bağlanmak

ilk Sunucu rsync ile senkronizasyon:

```
rsync -avzi -e "ssh -i /root/private.pem" --delete /var/www/html/ username@other_server:/var/www/html/
```

Diğer Sunucuda Senkronizasyon:

```
rsync -avzi -e "ssh -i /root/private.pem" --delete /var/www/html/ username@other_server:/var/www/html/
```


## Adım 4: Senkronizasyonu Otomatikleştirme

### 4.1. Cron Job ile Otomatik Senkronizasyon

Her iki sunucuda da cronjob ekleyerek rsync işlemini belirli aralıklarla otomatik olarak çalıştırabilirsiniz:


Cron Job Ekleme:

```
crontab -e
```


Cronjob'u ekleyin:

```
*/5 * * * * rsync -avz --delete /var/www/html/ username@remote_server_ip:/var/www/html/
```

### 4.2. Diğer Sunucu İçin Cronjob

Diğer sunucuda da aynı cronjob'u ekleyin.



## Adım 5: Apache Konfigürasyonu

Her iki sunucuda da Apache konfigürasyon dosyalarınızı düzenleyin:

```
sudo nano /etc/apache2/sites-available/000-default.conf
```

### 5.2. Apache'yi Yeniden Başlatma

Konfigürasyon dosyalarını düzenledikten sonra Apache'yi yeniden başlatın:

```
systemctl restart apache2
```

## TEST



### MAİN WEB SUNUCUSU

Sunucu içerinden belirtmiş olunan dosya yoluna değişiklik yapıldığında 1 dakika içerisinde backup web sunucusunda da aynı sayfa güncellenmiş oluyor.

![image](https://github.com/user-attachments/assets/e6f34373-99e9-426f-9cab-00b3b309d413)


### BACKUP WEB SUNUCUSU

![image](https://github.com/user-attachments/assets/d6235319-343b-4a96-a3bd-bba172860d14)









--------------------------------------------------------

Bu rehber, iki senkronize web sunucusu oluşturmanız ve yapılandırmanız için gereken adımları kapsamaktadır.

Okuduğunuz için teşekkürler.



