// WiFi 정보
const char* ssid = "TP-Link_C6A8_JU";
const char* password = "Javierju12";
const char* host = "35.213.49.212";
const int httpPort = 80;

CREATE USER 'javier'@'localhost' IDENTIFIED BY '@Javierju12';
GRANT ALL PRIVILEGES ON test.* TO 'javier'@'localhost';
FLUSH PRIVILEGES;

CREATE USER 'remote_user'@'%' IDENTIFIED BY '@Javierju12';
GRANT ALL PRIVILEGES ON *.* TO 'remote_user'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;


// SSH명령어
cd /var/www/html/
sudo nano process.php
sudo nano /var/www/html/index.html

//  MySQL명령어
sudo mysql -u root -p     p: @Javierju12
use test
SELECT * FROM tempnhumi;
SELECT * FROM sensor_readings_R4;

USE test;
DELETE FROM tempnhumi;
ALTER TABLE tempnhumi AUTO_INCREMENT = 1;

USE test;
DELETE FROM sensor_readings_R4;
ALTER TABLE sensor_readings_R4 AUTO_INCREMENT = 1;

ALTER TABLE tempnhumi
ADD COLUMN light INT,
ADD COLUMN soil_moisture INT;


//  GCP Shell
gcloud compute ssh test1-20240628-012038 --zone=asia-northeast1-b

The key fingerprint is:
SHA256:pMJfdqBMZd3FAV0iSOjNUFEydghhZfyTUbJzYUkaD9Y boswkd@cs-614166293691-default
The key's randomart image is:
+---[RSA 3072]----+
|        *XX*XOO+.|
|       +oo==oXEo |
|      ..o+ .+oo  |
|   . o +..o +o   |
|    o + S .  .   |
|     o o .       |
|      .          |
|                 |
|                 |
+----[SHA256]-----+

//  VS CODE SSH
PS C:\Users\khju> ssh-keygen -t rsa -f C:\Users\khju\.ssh\google_compute_engine -C boswkd@gmail.com -b 2048
Generating public/private rsa key pair.
C:\Users\khju\.ssh\google_compute_engine already exists.
Overwrite (y/n)? y
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\khju\.ssh\google_compute_engine
Your public key has been saved in C:\Users\khju\.ssh\google_compute_engine.pub
The key fingerprint is:
SHA256:V0QdirgtKP/ocdL0iVMUlqtiauZeJkIeVq+bq5Cqju0 boswkd@gmail.com
The key's randomart image is:
+---[RSA 2048]----+
|          ooo... |
|         o.+ ..  |
|    .   . o.o    |
|   . . . +..     |
|  + . o S.+      |
| = . +oo.* .     |
|o o oo*.= o      |
|oo .+* * .       |
|*oE**oo .        |
+----[SHA256]-----+
PS C:\Users\khju> type C:\Users\khju\.ssh\google_compute_engine.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDNDp+chYUOgEtZcQovGS1iI4i10H0zQ+agK4NC1FwFL/TafONANu853FIM3y92Cg26MHgrNs3vW+gH9fI2ZHZgrjUYysgOByP49W214UwbnuxBFoxwk+aEPW931WwTatUYLZeBmJsq1+MigF7aFCSZbdXO3sRlxI8snh6d1LNkshzdvMQqcIdRj34IIsw4J6SbI1bdh07NbmD4vIpaM1Wbs3Y7DpQveToG1l2RHwX5bgVVZwHwTmd3MM4q902A4tfCqXwG0KoFJQVHijYAJrzwch7/L12gA1oynBozJN2KS19SwjirFYH/CBQO1YFepyshfrnytXx5gK0rjToWwXu5 boswkd@gmail.com

// GCP VM sftp.json
// {
//     "name": "GCP VM - SmartFarm_test",
//     "host": "35.213.49.212",
//     "protocol": "sftp",
//     "port": 22,
//     "username": "boswkd",
//     "privateKeyPath": "C:/Users/khju/.ssh/google_compute_engine",
//     "remotePath": "/var/www/html/SmartFarm_test",
//     "uploadOnSave": false
// }