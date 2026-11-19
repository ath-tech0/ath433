# HYperRegedit / 3105

هذا هو المشروع الكامل المرسل في أرشيف MediaFire، ويحتوي على مشروع Xcode والشاشات والخدمات والأصول وملفات الـpatch ووحدات العمل الأصلية.

## البنية

- `ThreeOneOSFive/views`: واجهات التطبيق ونظام التصميم.
- `ThreeOneOSFive/helpers`: الخدمات الداخلية مثل إدارة الملفات والمشاريع والتنظيف والتخزين والأرشفة وإدارة التفعيل.
- `ThreeOneOSFive/Assets.xcassets`: الأيقونات والصور.
- `ThreeOneOSFive/Patches`: حزم patch المضمنة.
- `ThreeOneOSFive/exploit` و`ThreeOneOSFive/kexploit`: ملفات الدعم الأصلية كما وردت في المصدر.
- `ThreeOneOSFive.xcodeproj`: مشروع Xcode.
- `build_unsigned.sh`: بناء IPA غير موقع على macOS باستخدام Xcode.

## التفعيل وAPI

يتحقق التطبيق من الترخيص عبر `POST /api/license/mobile-verify` باستخدام مفتاح الترخيص ومعرّف تثبيت محفوظ في Keychain. لا يتم تضمين مفتاح HMAC داخل تطبيق iOS؛ يبقى `API_HMAC_SECRET` داخل بيئة Cloudflare Worker فقط. لا يمكن تطبيق patch إلا إذا أعاد الخادم `allow_inject: true`؛ وتظل الاستعادة متاحة. التفاصيل في [API_INTEGRATION.md](API_INTEGRATION.md).

## البناء

يتطلب البناء macOS وXcode. شغّل:

```bash
bash ./build_unsigned.sh
```

أو استخدم GitHub Actions مع المستودعات السرية المعرفة. يجب توقيع IPA بالطريقة المصرح بها قبل تثبيته.
