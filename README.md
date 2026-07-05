# gitlab-docker-compose
# GitLab CE Self-Hosted with Docker Compose

A production-style, self-hosted GitLab Community Edition setup using Docker Compose. This project demonstrates deploying and configuring a full-featured GitLab instance — including web (HTTP), SSH, and persistent storage — using a single, reproducible Compose file.

## 🚀 Features

- **GitLab CE 17.11.0** running in a single Docker container
- **Persistent data** via bind-mounted volumes (config, logs, application data survive container restarts/rebuilds)
- **Custom networking** using a dedicated Docker bridge network
- **SSH access** for Git operations (port 22) alongside HTTP web access (port 80/443)
- **Timezone configured** for Tehran (`Asia/Tehran`)
- **Omnibus configuration** injected directly through environment variables — no manual `gitlab.rb` editing required at first boot

## 🧱 Architecture

```
                ┌─────────────────────────────┐
                │        Host Machine          │
                │                               │
  Port 80  ───► │  ┌─────────────────────────┐ │
  Port 443 ───► │  │   gitlab_container       │ │
  Port 22  ───► │  │   (gitlab/gitlab-ce)      │ │
                │  │                           │ │
                │  │  /etc/gitlab   (config)   │ │
                │  │  /var/log/gitlab (logs)   │ │
                │  │  /var/opt/gitlab (data)   │ │
                │  └───────────┬───────────────┘ │
                │              │                 │
                │      gitlab-network (bridge)   │
                └─────────────────────────────────┘
```

## 📋 Prerequisites

- Docker Engine (20.10+)
- Docker Compose (v2 or the `docker-compose` v1.29+ binary)
- At least **4 GB RAM** available (GitLab recommends 4 GB minimum, 8 GB for smoother performance)
- At least **10 GB free disk space** for the persistent volumes
- Open ports `80`, `443`, and `22` on the host (make sure `22` doesn't conflict with your host's own SSH daemon)

## 📁 Project Structure

```
.
├── docker-compose.yml
└── README.md
```

Persistent data is stored on the host at:

```
/data/gitlab/config   -> /etc/gitlab
/data/gitlab/logs     -> /var/log/gitlab
/data/gitlab/data     -> /var/opt/gitlab
```

## ⚙️ Configuration

The GitLab instance is configured via the `GITLAB_OMNIBUS_CONFIG` environment variable, which is passed directly into GitLab's Omnibus configuration (`gitlab.rb`) on container startup:

| Setting | Value | Description |
|---|---|---|
| `external_url` | `http://192.168.1.5` | The URL used to access GitLab (update this to your own host IP or domain) |
| `gitlab_shell_ssh_port` | `22` | Port used for Git operations over SSH |
| `time_zone` | `Tehran` | Server timezone |
| `nginx['enable']` | `true` | Enables GitLab's bundled NGINX |
| `nginx['listen_port']` | `80` | Port NGINX listens on internally |
| `nginx['redirect_http_to_https']` | `false` | Disables forced HTTPS redirect (suitable for local/internal use) |

> ⚠️ **Important:** Before deploying, update `external_url` in `docker-compose.yml` to match your actual server's IP address or domain name. If it doesn't match how you access GitLab, you may run into redirect issues, broken clone URLs, or asset loading problems.

## ▶️ Getting Started

1. **Clone this repository**
   ```bash
   git clone <your-repo-url>
   cd <your-repo-folder>
   ```

2. **Create the data directories on the host** (or update the volume paths to your preference)
   ```bash
   sudo mkdir -p /data/gitlab/config /data/gitlab/logs /data/gitlab/data
   ```

3. **Edit `external_url`** in `docker-compose.yml` to match your server's IP/domain.

4. **Start the service**
   ```bash
   docker compose up -d
   ```

5. **Watch the logs** while GitLab initializes (first boot can take a few minutes)
   ```bash
   docker compose logs -f gitlab
   ```

6. **Access GitLab** in your browser at the `external_url` you configured (e.g. `http://192.168.1.5`).

## 🔑 Initial Login

On first boot, GitLab generates a random password for the `root` user. Retrieve it with:

```bash
docker exec -it gitlab_container grep 'Password:' /etc/gitlab/initial_root_password
```

> Note: This file is automatically deleted 24 hours after the first startup, so grab the password early.

## 🔐 Using SSH for Git Operations

Since port `22` is mapped directly to the container, you can clone/push over SSH as usual:

```bash
git clone git@192.168.1.5:group/project.git
```

Make sure to add your SSH public key under **GitLab → User Settings → SSH Keys**.

## 🛠️ Useful Commands

| Action | Command |
|---|---|
| Start the stack | `docker compose up -d` |
| Stop the stack | `docker compose down` |
| View logs | `docker compose logs -f gitlab` |
| Restart GitLab (apply config changes) | `docker exec -it gitlab_container gitlab-ctl reconfigure` |
| Check service status | `docker exec -it gitlab_container gitlab-ctl status` |
| Open a shell in the container | `docker exec -it gitlab_container bash` |

## 🧯 Troubleshooting

- **GitLab is slow / unresponsive right after startup:** This is normal — GitLab CE runs many internal services (Sidekiq, PostgreSQL, Redis, Puma, etc.) and can take 3–5 minutes to fully initialize on first boot.
- **Can't reach GitLab in the browser:** Double-check that `external_url` matches the address you're using, and that ports `80`/`443` aren't blocked by a firewall.
- **SSH clone fails:** Confirm port `22` isn't already used by the host's own SSH server, and that your SSH key is added to your GitLab profile.

## 📌 Notes

- This configuration uses **HTTP only** (`redirect_http_to_https: false`) and is intended for local/internal/lab environments. For production deployments exposed to the internet, configure TLS certificates and enable HTTPS.
- All persistent data lives outside the container under `/data/gitlab/`, so the container can be safely recreated (`docker compose up -d --force-recreate`) without data loss.

## 📄 License

This project is provided as-is for educational and portfolio purposes.

---

# راه‌اندازی GitLab CE با Docker Compose (فارسی)

این پروژه یک نمونه از راه‌اندازی سرویس **GitLab Community Edition** به‌صورت خودمیزبان (Self-Hosted) با استفاده از Docker Compose است. هدف این پروژه نمایش توانایی پیکربندی و مدیریت یک سرویس واقعی و پیچیده مانند GitLab با استفاده از یک فایل Compose ساده و قابل بازتولید است.

## ویژگی‌ها

- اجرای **GitLab CE نسخه 17.11.0** در یک کانتینر داکر
- ذخیره‌سازی دائمی داده‌ها (Config، Logs، Data) با استفاده از Volume های Bind Mount
- شبکه اختصاصی داکر برای ایزوله‌سازی سرویس
- دسترسی همزمان از طریق HTTP (پورت ۸۰) و SSH (پورت ۲۲) برای عملیات Git
- تنظیم منطقه زمانی روی تهران
- پیکربندی مستقیم GitLab از طریق متغیر محیطی، بدون نیاز به ویرایش دستی فایل `gitlab.rb`

## پیش‌نیازها

- نصب Docker (نسخه ۲۰.۱۰ به بالا)
- نصب Docker Compose
- حداقل ۴ گیگابایت RAM آزاد
- حداقل ۱۰ گیگابایت فضای دیسک آزاد
- باز بودن پورت‌های ۸۰، ۴۴۳ و ۲۲ روی سرور (دقت کنید پورت ۲۲ با SSH خود سرور تداخل نداشته باشد)

## نحوه اجرا

۱. مخزن را کلون کنید:
```bash
git clone <https://github.com/mamad0111/gitlab-docker-compose.git>
cd </gitlab-docker-compose>
```

۲. پوشه‌های داده روی هاست بسازید:
```bash
sudo mkdir -p /data/gitlab/config /data/gitlab/logs /data/gitlab/data
```

۳. مقدار `external_url` را در فایل `docker-compose.yml` متناسب با آی‌پی یا دامنه واقعی سرورتان تغییر دهید.

۴. سرویس را بالا بیاورید:
```bash
docker compose up -d
```

۵. لاگ‌ها را بررسی کنید (راه‌اندازی اولیه چند دقیقه طول می‌کشد):
```bash
docker compose logs -f gitlab
```

۶. از طریق مرورگر به آدرس تنظیم‌شده در `external_url` وارد شوید.

## دریافت رمز عبور اولیه کاربر root

```bash
docker exec -it gitlab_container grep 'Password:' /etc/gitlab/initial_root_password
```

> توجه: این فایل پس از ۲۴ ساعت از اولین اجرا به‌طور خودکار حذف می‌شود.

## دستورات کاربردی

| عملیات | دستور |
|---|---|
| بالا آوردن سرویس | `docker compose up -d` |
| متوقف کردن سرویس | `docker compose down` |
| مشاهده لاگ‌ها | `docker compose logs -f gitlab` |
| اعمال تغییرات پیکربندی | `docker exec -it gitlab_container gitlab-ctl reconfigure` |
| بررسی وضعیت سرویس‌ها | `docker exec -it gitlab_container gitlab-ctl status` |

## نکات مهم

- این تنظیمات فقط از HTTP استفاده می‌کند و برای محیط داخلی/آزمایشی مناسب است. برای استفاده در محیط تولید (Production) و در معرض اینترنت، حتماً گواهی TLS تنظیم و HTTPS فعال شود.
- تمام داده‌های دائمی خارج از کانتینر و در مسیر `/data/gitlab/` ذخیره می‌شوند، بنابراین کانتینر را می‌توان بدون از دست رفتن داده بازسازی کرد.
