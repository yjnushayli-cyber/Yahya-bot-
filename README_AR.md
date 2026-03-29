# مشروع البوت الجاهز للرفع على GitHub

هذا المشروع مقسوم إلى جزأين:

- `apps/web` → واجهة Next.js للنشر على Vercel
- `apps/worker` → محرك Python FastAPI للنشر على Railway

## ما الذي يفعله الآن
- worker يحلل ملف capture JSON
- يبني شموع 1 دقيقة
- يخرج إشارة: CALL / PUT / SKIP
- يحدد وقت بداية الصفقة ونهايتها لمدة دقيقة
- web تعرض آخر إشارة وترفع ملفات capture إلى worker

## النشر على GitHub بدون تيرمنال
1. فك ضغط الملف.
2. ارفع كل المحتويات إلى Repository جديد في GitHub.
3. لا ترفع ملف zip نفسه؛ ارفع الملفات بعد فك الضغط.

## تشغيل `apps/worker` على Railway
1. أنشئ مشروع جديد في Railway.
2. اختر Deploy from GitHub repo.
3. عند اختيار Root Directory اجعله: `apps/worker`
4. سيقرأ Railway ملف `railway.json` ويشغل:
   `uvicorn src.main:app --host 0.0.0.0 --port $PORT`
5. بعد النشر انسخ رابط الخدمة. مثال:
   `https://my-worker.up.railway.app`
6. افتح هذا الرابط وأضف `/replay-sample` كطلب POST من صفحة docs أو Postman إن رغبت.

## تشغيل `apps/web` على Vercel
1. أنشئ مشروع جديد في Vercel من نفس GitHub repo.
2. اجعل Root Directory = `apps/web`
3. أضف متغيرات البيئة التالية:
   - `WORKER_URL` = رابط Railway
   - `NEXT_PUBLIC_WORKER_URL` = نفس الرابط
4. اضغط Deploy.
5. افتح موقعك وستشاهد الواجهة.

## كيف تستخدمه
- في الواجهة اضغط على اختيار ملف JSON
- اختر capture من الإضافة
- اضغط "ارفع الملف وحلل"
- ستتحدث الإشارة تلقائيًا

## ملاحظات مهمة
- هذه النسخة تبني الإشارة من ملف capture، وليست بعد collector live دائم.
- مناسبة جدًا للبداية بدون تيرمنال.
- لاحقًا نبدل upload بربط حي مستمر.
