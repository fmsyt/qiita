---
title: JavaScriptのDateクラスにおける、初期タイムゾーンについて調べた
tags:
  - JavaScript
  - timezone
  - ブラウザ
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
---

## 環境

```bash
$ localectl
System Locale: LANG=ja_JP.UTF-8
```

```bash
$ google-chrome --version
Google Chrome 116.0.5845.179
```


`tz.html`

```html
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Check Timezone</title>
</head>
<body>
    <p>Current timezone is <code id="tz"></code></p>
    <p>Current time is <code id="time"></code></p>
    <script>
        function displayTZ() {
            // 2023/9/7 UTC
            const date = new Date("2023-09-07T00:00:00Z");

            document.getElementById('tz').innerHTML = Intl.DateTimeFormat().resolvedOptions().timeZone;
            document.getElementById('time').innerHTML = date.toLocaleString();
        }

        window.onload = function() {
            displayTZ();
        }
    </script>
</body>
</html>
```


## 環境変数TZを変更したときの挙動

```bash
$ google-chrome tz.html
```

Current timezone is Asia/Tokyo
Current time is 2023/9/7 9:00:00

```bash
$ TZ=UTC google-chrome tz.html
```

Current timezone is UTC
Current time is 2023/9/7 0:00:00

```bash
$ TZ= google-chrome tz.html
```

Current timezone is Etc/Unknown
Current time is 2023/9/7 0:00:00

```bash
$ TZ=Asia/Tokyo google-chrome tz.html
```

Current timezone is Asia/Tokyo
Current time is 2023/9/7 9:00:00

```bash
$ TZ=Pacific/Honolulu google-chrome tz.html
```

Current timezone is Pacific/Honolulu
Current time is 2023/9/6 14:00:00


## localectlで変更したときの挙動

```bash
$ localectl set-locale LANG=C.UTF-8
$ google-chrome tz.html
```

Current timezone is Asia/Tokyo
Current time is 2023/9/7 9:00:00

```bash
$ localectl set-locale LANG=en_US.UTF-8
$ google-chrome tz.html
```

Current timezone is Asia/Tokyo
Current time is 2023/9/7 9:00:00

```bash
$ localectl set-locale LANG=ja_JP.UTF-8
$ google-chrome tz.html
```

Current timezone is Asia/Tokyo
Current time is 2023/9/7 9:00:00
