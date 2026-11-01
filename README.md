# Docker Compose Permission Denied Error (WSL Ubuntu)

## 1. প্রথমে যে Error দেখা গেছে

WSL Ubuntu terminal-এ নিচের command চালানোর পর:

``` bash
docker compose up -d
```

এই error দেখা যায়:

``` text
permission denied while trying to connect to the docker API
at unix:///var/run/docker.sock
```

### Error-এর সহজ অর্থ

আপনার Ubuntu (WSL) terminal Docker API-এর সঙ্গে যোগাযোগ করতে পারছে না।
কারণ Docker socket (`/var/run/docker.sock`) ব্যবহারের permission নেই,
অথবা Docker Desktop-এর WSL Integration সঠিকভাবে চালু নেই।

------------------------------------------------------------------------

## 2. সমাধান

আপনি যেহেতু Windows 11 + Docker Desktop + WSL2 ব্যবহার করছেন, নিচের ধাপগুলো
অনুসরণ করুন।

### Step 1: Docker Desktop চালু করুন

1.  Windows থেকে **Docker Desktop** খুলুন।
2.  Docker Desktop চালু (Running) আছে কি না নিশ্চিত করুন।
3.  Docker Desktop-এর **Settings** খুলুন।
4.  **Resources → WSL Integration**-এ যান।
5.  **Enable integration with my default WSL distro** চালু করুন।
6.  Ubuntu-এর পাশের Integration অপশনটি **Enabled** করুন।
7.  **Apply & Restart** চাপুন।

### Step 2: WSL restart করুন

Windows PowerShell খুলে চালান:

``` powershell
wsl --shutdown
```

এরপর Ubuntu আবার চালু করুন।

### Step 3: Docker কাজ করছে কি না পরীক্ষা করুন

Ubuntu terminal-এ চালান:

``` bash
docker version
```

এরপর:

``` bash
docker ps
```

দুটি command-ই ঠিকভাবে কাজ করলে Docker-এর সঙ্গে Ubuntu-এর connection ঠিক
আছে।

### Step 4: Project directory-তে যান

``` bash
cd /mnt/c/Users/User/Desktop/nest-kafka
```

### Step 5: Docker Compose চালান

``` bash
docker compose up -d
```

এবার Docker Compose আপনার containers চালু করার কথা।

------------------------------------------------------------------------

## 3. তারপরও একই Error হলে

প্রথমে Docker socket-এর permission দেখুন:

``` bash
ls -l /var/run/docker.sock
```

এরপর আপনার user কোন কোন group-এর সদস্য তা দেখুন:

``` bash
groups
```

এই দুই command-এর output সংরক্ষণ করুন। Docker Desktop-এর WSL Integration
ঠিক আছে কি না আবার পরীক্ষা করুন।

> **মনে রাখবেন:** Docker Desktop ব্যবহার করলে সাধারণত Ubuntu-তে আলাদা করে
> Docker Engine install করার প্রয়োজন নেই। Docker Desktop-এর WSL
> Integration ব্যবহার করুন। Permission error সমাধানের জন্য অন্ধভাবে `sudo`
> ব্যবহার করবেন না।

------------------------------------------------------------------------

## 4. Quick Checklist

-   [ ] Docker Desktop চালু আছে।
-   [ ] WSL Integration Enabled আছে।
-   [ ] Ubuntu Integration Enabled আছে।
-   [ ] `wsl --shutdown` চালিয়ে WSL restart করা হয়েছে।
-   [ ] `docker version` কাজ করছে।
-   [ ] `docker ps` কাজ করছে।
-   [ ] `docker compose up -d` চালানো হয়েছে।

## সংক্ষেপে

**সমস্যা:** WSL Ubuntu Docker socket access করতে পারছে না।

**সমাধান:** Docker Desktop চালু করুন → WSL Integration Enable করুন → WSL
restart করুন → Docker পরীক্ষা করুন → Docker Compose চালান।
