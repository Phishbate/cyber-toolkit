# Setup GoPhish Server

Setup Kali LINUX

[https://www.youtube.com/watch?v=9zdjQ9w_v_4](https://www.youtube.com/watch?v=9zdjQ9w_v_4)

Setup on LightSale

- Setup on either EC2 or Lightsail

Lightsail instructions

[https://www.youtube.com/watch?v=6n3kM5M1hOo&t=229s](https://www.youtube.com/watch?v=6n3kM5M1hOo&t=229s)

EC2 Instructions

[Phishing with GoPhish (a-z story)](https://medium.com/@a.izraeli/phishing-with-gophish-a-z-story-53e25d49a01)

**Setup GoPhish Server**

```bash
wget https://github.com/gophish/gophish/releases/download/v0.12.1/gophish-v0.12.1-linux-64bit.zip
```

Install Unzip

```bash
apt install unzip
```

Unzip File

```bash
unzip gophish
```

Configure the JSON File

```bash
VIM config.json
```

Click ‘i’ to insert

then click ‘esc’ to exit

 then enter ‘:wq!’ to exit

Update the permissions

```bash
chmod 777 gophish
```

Start her up

```bash
./gophish
```

Password

```bash
48187762e3cbe918
```

Navigate to the IP Address for LightSail instance

```bash
https://13.211.200.252:3333/login?next=%2F
```

[Page title...](Page%20title%20f3cf3f10fba649deb949e9aa3efca1ab.md)

[Page title...](Page%20title%20d450bc3abf5646b093ff3e76db7241cd.md)

[Handling Database Migrations in Go · Gophish - Blog](Handling%20Database%20Migrations%20in%20Go%20·%20Gophish%20-%20Blo%201290626fba7e465aa5b8a4cb4b477d89.md)

[Logging - Gophish User Guide](Logging%20-%20Gophish%20User%20Guide%20647ed309d9494cbebd3605f0b22c395c.md)

[Official Elasticsearch Pricing: Elastic Cloud, Managed Elasticsearch](Official%20Elasticsearch%20Pricing%20Elastic%20Cloud,%20Mana%204c422cdbd31f474b86a6044759d0985d.md)