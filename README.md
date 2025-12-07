# airflow-docker-ubuntu-vm

**Bu proses Airflow-un rəsmi (production) imicinə əsaslanır və verilənlər bazası (PostgreSQL) ilə mesaj brokerini (Redis) də avtomatik quraşdırır.**

Aşağıdakı addımları izləyərək bu sistemi qura bilərsiniz.

---

1. **Hazırlıq (Qovluqların yaradılması)**
Airflow-un işləməsi üçün bəzi qovluqların əvvəlcədən mövcud olması və düzgün icazələrə malik olması vacibdir. Ubuntu terminalında bu əmrləri icra edin:

```bash
mkdir -p ./dags ./logs ./plugins ./config
echo -e "AIRFLOW_UID=$(id -u)" > .env
```
**Qeyd: .env faylı sadəcə istifadəçi ID-sini təyin etmək üçündür, bütün konfiqurasiya docker-compose.yml daxilində olacaq.**


---

2. **docker-compose.yml Faylı**
İndi həmin qovluqda docker-compose.yml adlı fayl yaradın və repodakı kodu olduğu kimi daxil edin.

Bu fayl aşağıdakı servisləri ehtiva edir:

- Postgres: Airflow-un metadata bazası.
- Redis: Tapşırıqların növbəyə düzülməsi üçün.
- Airflow Webserver: İnterfeys (UI).
- Airflow Scheduler: Tapşırıqları planlayan.
- Airflow Worker: Tapşırıqları icra edən.
- Airflow Init: Bazanı ilkin olaraq yaradan və "airflow" istifadəçisini əlavə edən köməkçi servis.

---

3. **İşə Salınma (Run)**
Terminalda docker-compose.yml faylının olduğu qovluqda aşağıdakı əmri icra edin:

```bash
docker compose up -d
```
---

4. **Yoxlama və Giriş**
Bütün konteynerlərin işlədiyini yoxlamaq üçün:

```bash
docker compose ps
```

- Hər şeyin statusu "Up" və ya "healthy" olmalıdır.
- Brauzerinizi açın və aşağıdakı ünvana daxil olun (əgər VM-in daxilindəsinizsə):

- URL: http://localhost:8080
- İstifadəçi adı: airflow
- Şifrə: airflow

**Əgər host maşından (məsələn Windows-dan) VMware-dəki Ubuntu-ya qoşulursunuzsa, localhost əvəzinə Ubuntu-nun IP adresini yazmalısınız (məsələn: http://192.168.x.x:8080).**

---

**Vacib Qeyd**
- Apache Airflow resurs tələb edən bir sistemdir. VMware parametrlərində Ubuntu-ya ən azı 4GB RAM (tövsiyə olunan 6-8GB) və 2 vCPU verdiyinizdən əmin olun, əks halda konteynerlər "Exit Code 137" (Out of Memory) xətası ilə dayana bilər.
