<div align="right">

🌐 [English](#-english) | [فارسی](#-فارسی)

</div>

---

## 🇬🇧 English

# GitLab CE Self-Hosted Deployment with Docker Compose

A production-style **Docker Compose** setup for self-hosting **GitLab Community Edition (CE)**. This project demonstrates containerized deployment, persistent data management, and Omnibus configuration of a real-world DevOps platform.

**Repository:** [github.com/mamad0111/gitlab-docker-compose](https://github.com/mamad0111/gitlab-docker-compose)

### 📌 Overview

This repository provides a ready-to-use `docker-compose.yml` file to deploy a fully functional GitLab CE instance using the official `gitlab/gitlab-ce` image. It includes persistent storage for configuration, logs, and application data, along with custom Omnibus GitLab configuration (timezone, SSH port, NGINX settings, etc.).

### 🛠️ Tech Stack

- **Docker & Docker Compose** — container orchestration
- **GitLab CE 17.11.0** — self-hosted Git repository management, CI/CD, and DevOps platform
- **NGINX** (bundled via Omnibus) — reverse proxy / web server layer
- **Bridge Networking** — isolated custom Docker network

### ✨ Features

- One-command deployment of a complete GitLab instance
- Persistent volumes for config, logs, and data (survives container restarts/rebuilds)
- Custom Omnibus configuration via `GITLAB_OMNIBUS_CONFIG`
- Configurable external URL, timezone, and SSH port
- Isolated Docker network for the service
- Auto-restart policy (`restart: always`) for high availability

### 📂 Project Structure

```
.
├── docker-compose.yml
└── README.md
```

### ⚙️ Configuration Details

| Setting | Value | Description |
|---|---|---|
| `external_url` | `http://gitlab.example.com` | Public URL used to access GitLab |
| `gitlab_shell_ssh_port` | `22` | Port used for Git operations over SSH |
| `time_zone` | `Tehran` | Server timezone |
| `nginx['listen_port']` | `80` | HTTP port for the bundled NGINX |
| `nginx['redirect_http_to_https']` | `false` | HTTPS redirect disabled (HTTP only, by default) |

#### Ports

| Host Port | Container Port | Purpose |
|---|---|---|
| `80` | `80` | HTTP (Web UI) |
| `443` | `443` | HTTPS (Web UI, if enabled) |
| `22` | `22` | SSH (Git clone/push/pull) |

#### Volumes

| Host Path | Container Path | Purpose |
|---|---|---|
| `/data/gitlab/config` | `/etc/gitlab` | GitLab configuration files |
| `/data/gitlab/logs` | `/var/log/gitlab` | Application & service logs |
| `/data/gitlab/data` | `/var/opt/gitlab` | Repositories, database, artifacts, etc. |

### 🚀 Getting Started

#### Prerequisites

- Docker Engine installed
- Docker Compose v2 (or v1.28+)
- At least **4 GB of RAM** (8 GB+ recommended) and **2 CPU cores**
- Ports `80`, `443`, and `22` available on the host

#### Installation

1. Clone this repository:
   ```bash
   git clone git@github.com:mamad0111/gitlab-docker-compose.git
   cd gitlab-docker-compose
   ```

2. (Optional) Update the `external_url` in `docker-compose.yml` to match your domain or server IP:
   ```yaml
   external_url 'http://your-domain-or-ip'
   ```

3. Create the required host directories (or let Docker create them automatically):
   ```bash
   sudo mkdir -p /data/gitlab/{config,logs,data}
   ```

4. Start the service:
   ```bash
   docker compose up -d
   ```

5. Monitor the startup logs (GitLab can take a few minutes to become fully ready):
   ```bash
   docker compose logs -f gitlab
   ```

6. Once ready, access GitLab in your browser at:
   ```
   http://gitlab.example.com
   ```
   or `http://<your-server-ip>` if you didn't change the `external_url`.

#### Initial Login

On first boot, GitLab generates a random password for the `root` user. Retrieve it with:

```bash
docker exec -it gitlab_container grep 'Password:' /etc/gitlab/initial_root_password
```

> ⚠️ This file is automatically deleted 24 hours after the first startup, so retrieve the password promptly.

### 🔧 Useful Commands

| Command | Description |
|---|---|
| `docker compose up -d` | Start the container in detached mode |
| `docker compose down` | Stop and remove the container |
| `docker compose logs -f gitlab` | Follow container logs |
| `docker exec -it gitlab_container gitlab-ctl status` | Check status of internal GitLab services |
| `docker exec -it gitlab_container gitlab-ctl reconfigure` | Apply configuration changes |
| `docker exec -it gitlab_container gitlab-ctl restart` | Restart internal GitLab services |

### 📝 Notes

- This configuration serves GitLab over **HTTP only**. For production environments, it is strongly recommended to enable HTTPS (e.g., via `letsencrypt['enable'] = true` in the Omnibus config, or by placing a reverse proxy like Traefik/NGINX in front of it).
- All persistent data lives outside the container under `/data/gitlab`, so the container can be safely removed/recreated without data loss.
- Resource usage can be high; consider tuning `puma['worker_processes']` and `postgresql['shared_buffers']` in `GITLAB_OMNIBUS_CONFIG` for smaller servers.

### 📄 License

This project is open source and available for learning and reference purposes.

### 👤 Author

**mamad0111**
GitHub: [@mamad0111](https://github.com/mamad0111)

Feel free to connect or reach out with questions/suggestions about this setup.

<div align="right">

[⬆ Back to top](#-english) | [برو به فارسی ⬇](#-فارسی)

</div>

---

## 🇮🇷 فارسی

# استقرار GitLab CE با Docker Compose (Self-Hosted)

یک پیکربندی **Docker Compose** به سبک محیط تولید (production-style) برای راه‌اندازی **GitLab Community Edition (CE)** به‌صورت خودمیزبان (Self-Hosted). این پروژه نمونه‌ای از استقرار کانتینری، مدیریت داده‌های پایدار (Persistent Data) و پیکربندی Omnibus یک پلتفرم واقعی DevOps را نشان می‌دهد.

**ریپازیتوری:** [github.com/mamad0111/gitlab-docker-compose](https://github.com/mamad0111/gitlab-docker-compose)

### 📌 معرفی

این ریپازیتوری یک فایل آماده `docker-compose.yml` برای راه‌اندازی یک نمونه کاملاً فعال از GitLab CE با استفاده از ایمیج رسمی `gitlab/gitlab-ce` ارائه می‌دهد. این پیکربندی شامل ذخیره‌سازی پایدار برای کانفیگ، لاگ‌ها و داده‌های برنامه، همراه با تنظیمات سفارشی Omnibus GitLab (منطقه زمانی، پورت SSH، تنظیمات NGINX و...) است.

### 🛠️ تکنولوژی‌های استفاده‌شده

- **Docker و Docker Compose** — مدیریت و ارکستراسیون کانتینرها
- **GitLab CE 17.11.0** — مدیریت مخازن گیت، CI/CD و پلتفرم DevOps به‌صورت خودمیزبان
- **NGINX** (همراه با Omnibus) — لایه وب‌سرور / پروکسی معکوس
- **شبکه Bridge اختصاصی** — شبکه ایزوله و مجزای داکر

### ✨ ویژگی‌ها

- استقرار کامل GitLab تنها با یک دستور
- Volume های پایدار برای کانفیگ، لاگ و داده (بدون از دست رفتن داده هنگام ری‌استارت یا بازسازی کانتینر)
- پیکربندی سفارشی Omnibus از طریق متغیر `GITLAB_OMNIBUS_CONFIG`
- قابلیت تنظیم آدرس خارجی (external URL)، منطقه زمانی و پورت SSH
- شبکه اختصاصی و ایزوله برای سرویس
- سیاست ری‌استارت خودکار (`restart: always`) برای پایداری بیشتر سرویس

### 📂 ساختار پروژه

```
.
├── docker-compose.yml
└── README.md
```

### ⚙️ جزئیات پیکربندی

| تنظیم | مقدار | توضیح |
|---|---|---|
| `external_url` | `http://gitlab.example.com` | آدرس عمومی دسترسی به GitLab |
| `gitlab_shell_ssh_port` | `22` | پورت استفاده‌شده برای عملیات گیت از طریق SSH |
| `time_zone` | `Tehran` | منطقه زمانی سرور |
| `nginx['listen_port']` | `80` | پورت HTTP برای NGINX داخلی |
| `nginx['redirect_http_to_https']` | `false` | ریدایرکت HTTPS غیرفعال است (به‌صورت پیش‌فرض فقط HTTP) |

#### پورت‌ها

| پورت میزبان | پورت کانتینر | کاربرد |
|---|---|---|
| `80` | `80` | رابط وب (HTTP) |
| `443` | `443` | رابط وب (HTTPS، در صورت فعال‌سازی) |
| `22` | `22` | SSH (کلون، پوش و پول گیت) |

#### Volume ها

| مسیر میزبان | مسیر داخل کانتینر | کاربرد |
|---|---|---|
| `/data/gitlab/config` | `/etc/gitlab` | فایل‌های پیکربندی GitLab |
| `/data/gitlab/logs` | `/var/log/gitlab` | لاگ‌های سرویس و برنامه |
| `/data/gitlab/data` | `/var/opt/gitlab` | مخازن، دیتابیس، آرتیفکت‌ها و غیره |

### 🚀 شروع به کار

#### پیش‌نیازها

- نصب بودن Docker Engine
- Docker Compose نسخه v2 (یا v1.28 به بالا)
- حداقل **۴ گیگابایت رم** (پیشنهاد: ۸ گیگابایت یا بیشتر) و **۲ هسته پردازنده**
- در دسترس بودن پورت‌های `80`، `443` و `22` روی سرور

#### نصب و راه‌اندازی

۱. کلون کردن ریپازیتوری:
   ```bash
   git clone git@github.com:mamad0111/gitlab-docker-compose.git
   cd gitlab-docker-compose
   ```

۲. (اختیاری) ویرایش مقدار `external_url` در فایل `docker-compose.yml` متناسب با دامنه یا آی‌پی سرور خودتان:
   ```yaml
   external_url 'http://your-domain-or-ip'
   ```

۳. ساخت پوشه‌های موردنیاز روی سرور میزبان (یا اجازه دهید داکر خودش بسازد):
   ```bash
   sudo mkdir -p /data/gitlab/{config,logs,data}
   ```

۴. اجرای سرویس:
   ```bash
   docker compose up -d
   ```

۵. مشاهده لاگ‌های راه‌اندازی (بالا آمدن کامل GitLab ممکن است چند دقیقه طول بکشد):
   ```bash
   docker compose logs -f gitlab
   ```

۶. پس از آماده شدن، از طریق مرورگر به آدرس زیر دسترسی پیدا کنید:
   ```
   http://gitlab.example.com
   ```
   یا در صورت عدم تغییر `external_url`، از آدرس `http://<آی‌پی-سرور-شما>` استفاده کنید.

#### ورود اولیه (Initial Login)

در اولین اجرا، GitLab یک رمز عبور تصادفی برای کاربر `root` تولید می‌کند. برای دریافت آن دستور زیر را اجرا کنید:

```bash
docker exec -it gitlab_container grep 'Password:' /etc/gitlab/initial_root_password
```

> ⚠️ این فایل ۲۴ ساعت پس از اولین اجرا به‌صورت خودکار حذف می‌شود، پس حتماً رمز را در همان ابتدا دریافت و ذخیره کنید.

### 🔧 دستورات کاربردی

| دستور | توضیح |
|---|---|
| `docker compose up -d` | اجرای کانتینر در حالت پس‌زمینه (detached) |
| `docker compose down` | توقف و حذف کانتینر |
| `docker compose logs -f gitlab` | مشاهده زنده لاگ‌های کانتینر |
| `docker exec -it gitlab_container gitlab-ctl status` | بررسی وضعیت سرویس‌های داخلی GitLab |
| `docker exec -it gitlab_container gitlab-ctl reconfigure` | اعمال تغییرات پیکربندی |
| `docker exec -it gitlab_container gitlab-ctl restart` | ری‌استارت سرویس‌های داخلی GitLab |

### 📝 نکات مهم

- این پیکربندی GitLab را فقط از طریق **HTTP** سرو می‌کند. برای محیط‌های تولید (Production) قویاً توصیه می‌شود HTTPS فعال شود (مثلاً با تنظیم `letsencrypt['enable'] = true` در پیکربندی Omnibus، یا با قرار دادن یک ریورس‌پروکسی مانند Traefik یا NGINX جلوی سرویس).
- تمام داده‌های پایدار خارج از کانتینر و در مسیر `/data/gitlab` نگهداری می‌شوند، بنابراین می‌توانید کانتینر را بدون نگرانی از بین رفتن داده حذف یا بازسازی کنید.
- مصرف منابع سیستمی GitLab می‌تواند بالا باشد؛ برای سرورهای کوچک‌تر، تنظیم پارامترهایی مانند `puma['worker_processes']` و `postgresql['shared_buffers']` در `GITLAB_OMNIBUS_CONFIG` توصیه می‌شود.

### 📄 لایسنس

این پروژه متن‌باز است و برای اهداف یادگیری و مرجع در دسترس قرار دارد.

### 👤 نویسنده

**mamad0111**
گیت‌هاب: [@mamad0111](https://github.com/mamad0111)

خوشحال می‌شوم برای سوال یا پیشنهاد درباره این پروژه با من در ارتباط باشید.

<div align="right">

[⬆ برو به بالا](#-فارسی) | [Back to English ⬆](#-english)

</div>
