
# آموزش ساخت گواهی‌نامه (Certificate) در میکروتیک

## مرحله 1: ایجاد الگوهای گواهی‌نامه (Templates)

/certificate
```bash
add name=ca-template common-name=<ip or domain> days-valid=3650 key-size=1024 key-usage=crl-sign,key-cert-sign
add name=server-template common-name=*.<ip or domain> days-valid=3650 key-size=1024 key-usage=digital-signature,key-encipherment,tls-server
add name=client-template common-name=client.<ip or domain> days-valid=3650 key-size=1024 key-usage=tls-client
```

## مرحله 2: امضای گواهی‌نامه‌ها (Signing)

/certificate
```bash
sign ca-template name=ca-certificate
sign server-template name=server-certificate ca=ca-certificate
sign client-template name=client-certificate ca=ca-certificate
```

## مرحله 3: خروجی گرفتن از گواهی‌نامه‌ها (Export)

/certificate
```bash
export-certificate ca-certificate export-passphrase=""
export-certificate client-certificate export-passphrase=12345678
```

## مرحله 4: انتقال فایل‌ها به کلاینت

از طریق Winbox وارد منوی **Files** شوید و فایل‌های زیر را به سیستم کلاینت منتقل کنید:

- `cert_export_ca-certificate.crt`
- `cert_export_client-certificate.crt`
- `cert_export_client-certificate.key`

> 🔐 رمز عبور فایل client key همان `12345678` است که در مرحله قبل تعیین شد.

---

**نکته:** مقادیر `<ip or domain>` را با آدرس دامنه یا IP واقعی سرور جایگزین نمایید.


## برای خارج کردن مقدار فایل `cert_export_client-certificate.key` میتونید فایل رو یک سرور لینوکسی یا برنامه های ssl	reader انتقال بدین 

openssl rsa -in ~/*.key -check