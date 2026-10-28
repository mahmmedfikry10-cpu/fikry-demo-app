# Fikry Demo App — تطبيق فكري التجريبي

## إيه هو المشروع ده؟
تطبيق أندرويد بسيط (Java + WebView) بيفتح صفحة HTML عربية RTL من جوه التطبيق نفسه، وشغال **بدون إنترنت**.
فيه:
- عنوان ترحيبي: "أهلاً بيك في تطبيق فكري"
- عداد تفاعلي (زيادة / نقصان / تصفير)
- قائمة مهام (إضافة / إنجاز / حذف)
- حفظ العداد والمهام على الجهاز بـ `localStorage`

مفيش Firebase ولا مفاتيح API ولا أي خدمات مدفوعة، ومفيش أي صلاحيات (Permissions) مطلوبة.

## أهم الملفات
| الملف | وظيفته |
|---|---|
| `app/src/main/assets/index.html` | واجهة التطبيق (HTML + CSS + JS) |
| `app/src/main/java/com/fikry/demo/MainActivity.java` | الـ Activity اللي فيها الـ WebView وزرار الرجوع |
| `app/src/main/AndroidManifest.xml` | تعريف التطبيق |
| `app/build.gradle` | إعدادات بناء التطبيق (minSdk 23، compileSdk 35، Java 17) |
| `build.gradle` / `settings.gradle` | إعدادات Gradle الرئيسية (Android Gradle Plugin 8.7.3) |
| `.github/workflows/build-apk.yml` | بناء الـ APK أوتوماتيك على GitHub Actions |

> **ملحوظة:** المشروع **مفيهوش Gradle Wrapper** (`gradlew`). الـ workflow بيركّب Gradle 8.9 بنفسه عن طريق `gradle/actions/setup-gradle`، فمش محتاج تعمل حاجة.

## رفع المشروع على GitHub
1. فك ضغط الملف `Fikry_Demo_Android_Project.zip`. هتلاقي فولدر `fikry-demo-app`.
2. اعمل Repository جديد على GitHub (مثلاً اسمه `fikry-demo-app`).
3. ارفع **محتويات** الفولدر (مش الفولدر نفسه) في جذر الـ Repository، بحيث `app/` و `build.gradle` و `.github/` تبقى في أول الـ Repo.
   - من الموبايل أو المتصفح: Add file ← Upload files، وبعدين اسحب الملفات.
   - ⚠️ الفولدرات المخفية (اللي بتبدأ بنقطة) ساعات مبتظهرش في الرفع. لو `.github` ماترفعتش، اعمل ملف جديد من GitHub (Add file ← Create new file) واكتب في الاسم `.github/workflows/build-apk.yml` وانسخ محتواه.
4. تأكد إن الفرع اسمه `main`.

## التأكد إن الـ workflow اتحمّل صح
- افتح تبويب **Actions** في الـ Repository.
- لازم تلاقي workflow اسمه **Build Debug APK**. لو مش موجود يبقى `.github/workflows/build-apk.yml` مترفعش صح.

## تشغيل البناء
- أوتوماتيك: أي Push على الفرع `main`.
- يدوي: Actions ← **Build Debug APK** ← **Run workflow**.
- استنى حوالي 3–8 دقايق في أول مرة.

## تحميل الـ APK
1. افتح الـ run اللي خلص بنجاح (علامة ✅).
2. انزل لتحت لحد **Artifacts**.
3. نزّل **fikry-demo-debug-apk** (هينزل كملف ZIP، فك ضغطه تلاقي `app-debug.apk`).

## تثبيت الـ APK على الموبايل
1. انقل `app-debug.apk` للموبايل.
2. افتحه، ولو طلب السماح بالتثبيت من مصادر غير معروفة فعّلها للتطبيق اللي بتفتح منه (Files أو Chrome).
3. اضغط Install. الحد الأدنى: Android 6.0.

## حل المشاكل الشائعة
- **مفيش Workflow في Actions:** الملف `.github/workflows/build-apk.yml` مترفعش أو في مكان غلط.
- **فشل بسبب SDK / licenses:** اعمل Re-run للـ job، وتأكد إن خطوة "Install required SDK components" خلصت.
- **APK not found:** اقرأ أخطاء خطوة "Build debug APK" فوقها، السبب غالباً هناك.
- **فشل Gradle بعد تعديل الإصدارات:** ماتغيرش إصدار Android Gradle Plugin من غير ما تتأكد إنه متوافق مع Gradle 8.9 (AGP 8.7.3 محتاج Gradle 8.9 أو أحدث).
- **التطبيق مش بيتثبت:** اتأكد إن إصدار أندرويد 6.0 أو أعلى، وإنك فعّلت التثبيت من مصادر غير معروفة.

## تنبيه مهم
الـ APK ده **Debug** للتجربة بس، وموقّع بمفتاح الـ debug الافتراضي. مش نسخة Release مُوقّعة وينفع ترفعها على Google Play.
