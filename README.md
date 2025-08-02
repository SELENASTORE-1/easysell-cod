# EasySell COD - نظام الطلب السريع مع الدفع عند الاستلام

تطبيق ويب متكامل شبيه بـ EasySell COD، مصمم خصيصاً للتجارة الإلكترونية مع الدفع عند الاستلام، متكامل بالكامل مع Shopify.

## 🌟 المميزات الرئيسية

- **واجهة طلب بسيطة وسهلة الاستخدام**
- **تحقق من رقم الهاتف عبر SMS** باستخدام Twilio
- **تكامل كامل مع Shopify** لإدارة المنتجات والطلبات
- **تصدير أوتوماتيكي للطلبات** إلى Google Sheets
- **نظام عروض Upsell متقدم** لزيادة قيمة الطلبات
- **تصميم متجاوب** يعمل على جميع الأجهزة
- **نشر سهل** باستخدام Docker

## 🏗️ بنية المشروع

```
easysell-clone/
├── easysell-app/          # React Frontend
│   ├── src/
│   │   ├── components/    # مكونات React
│   │   ├── services/      # خدمات API
│   │   └── utils/         # أدوات مساعدة
│   ├── Dockerfile
│   └── nginx.conf
├── easysell-backend/      # Flask Backend
│   ├── src/
│   │   ├── models/        # نماذج قاعدة البيانات
│   │   ├── routes/        # مسارات API
│   │   ├── services/      # خدمات الأعمال
│   │   └── config/        # ملفات التكوين
│   ├── Dockerfile
│   └── requirements.txt
├── docker-compose.yml     # إعداد Docker Compose
└── .env.example          # مثال على متغيرات البيئة
```

## 🚀 التثبيت والتشغيل

### المتطلبات الأساسية

- Node.js 20+
- Python 3.11+
- Docker (اختياري للنشر)

### التثبيت المحلي

#### 1. استنساخ المشروع

```bash
git clone <repository-url>
cd easysell-clone
```

#### 2. إعداد Backend

```bash
cd easysell-backend
python -m venv venv
source venv/bin/activate  # على Windows: venv\Scripts\activate
pip install -r requirements.txt
```

#### 3. إعداد Frontend

```bash
cd ../easysell-app
npm install
```

#### 4. إعداد متغيرات البيئة

انسخ ملف `.env.example` إلى `.env` وقم بتعديل القيم:

```bash
cp .env.example .env
```

#### 5. تشغيل التطبيق

**تشغيل Backend:**
```bash
cd easysell-backend
source venv/bin/activate
python -m src.main
```

**تشغيل Frontend:**
```bash
cd easysell-app
npm run dev
```

الآن يمكنك الوصول إلى التطبيق على `http://localhost:5173`




### النشر باستخدام Docker

#### 1. إعداد متغيرات البيئة

```bash
cp .env.example .env
# قم بتعديل القيم في ملف .env
```

#### 2. بناء وتشغيل الحاويات

```bash
docker-compose up -d
```

#### 3. التحقق من حالة الخدمات

```bash
docker-compose ps
docker-compose logs
```

التطبيق سيكون متاحاً على:
- Frontend: `http://localhost`
- Backend API: `http://localhost:5000`

## ⚙️ التكوين

### متغيرات البيئة المطلوبة

#### Flask Backend

```env
# إعدادات Flask
FLASK_ENV=production
SECRET_KEY=your-secret-key-here

# إعدادات Shopify
SHOPIFY_STORE_URL=selenastor.myshopify.com
SHOPIFY_API_VERSION=2024-01
SHOPIFY_ACCESS_TOKEN=shpat_xxxxxxxxxxxxxxxx

# إعدادات Twilio
TWILIO_ACCOUNT_SID=ACxxxxxxxxxxxxxxxxx
TWILIO_AUTH_TOKEN=xxxxxxxxxxxxxxxxxxx
TWILIO_PHONE_NUMBER=+2126xxxxxxx

# إعدادات Google Sheets
GOOGLE_SHEETS_CLIENT_EMAIL=xxxxx@project.iam.gserviceaccount.com
GOOGLE_SHEETS_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----"
GOOGLE_SHEETS_SPREADSHEET_ID=your-spreadsheet-id-here
```

#### React Frontend

```env
VITE_BACKEND_API_URL=http://localhost:5000
```

### إعداد Shopify

1. **إنشاء تطبيق خاص في Shopify:**
   - اذهب إلى Shopify Partners
   - أنشئ تطبيقاً خاصاً جديداً
   - احصل على API credentials

2. **الصلاحيات المطلوبة:**
   - `read_products`
   - `write_products`
   - `read_orders`
   - `write_orders`
   - `read_customers`
   - `write_customers`
   - `write_draft_orders`

3. **إعداد Webhooks (اختياري):**
   - `orders/create`
   - `orders/updated`
   - `products/create`
   - `products/update`

### إعداد Twilio

1. **إنشاء حساب Twilio:**
   - سجل في Twilio.com
   - احصل على Account SID و Auth Token

2. **شراء رقم هاتف:**
   - اشترِ رقم هاتف مغربي (+212)
   - فعّل خدمة SMS

3. **إعداد الرسائل:**
   - تأكد من أن الرقم يدعم الإرسال إلى المغرب
   - اختبر إرسال رسالة تجريبية

### إعداد Google Sheets

1. **إنشاء مشروع Google Cloud:**
   - اذهب إلى Google Cloud Console
   - أنشئ مشروعاً جديداً
   - فعّل Google Sheets API

2. **إنشاء Service Account:**
   - أنشئ Service Account
   - حمّل ملف JSON الخاص بالمفاتيح
   - انسخ البيانات إلى متغيرات البيئة

3. **إعداد الجدول:**
   - أنشئ Google Sheet جديد
   - شارك الجدول مع Service Account email
   - انسخ Spreadsheet ID من الرابط



## 📡 واجهات برمجة التطبيقات (APIs)

### مسارات المنتجات

```http
GET /api/shopify/products
# الحصول على جميع المنتجات

GET /api/shopify/products/{product_id}
# الحصول على منتج محدد
```

### مسارات الطلبات

```http
POST /api/orders
# إنشاء طلب جديد

GET /api/orders/{order_id}
# الحصول على طلب محدد

POST /api/orders/{order_id}/verify
# التحقق من الطلب
```

### مسارات SMS

```http
POST /api/sms/send-verification
# إرسال رمز التحقق

POST /api/sms/verify-code
# التحقق من الرمز

POST /api/sms/validate-phone
# التحقق من صحة رقم الهاتف
```

### مسارات Upsell

```http
POST /api/upsell/offers
# الحصول على عروض Upsell

POST /api/upsell/calculate-total
# حساب إجمالي الطلب مع العروض

GET /api/upsell/config
# الحصول على تكوين العروض

PUT /api/upsell/config
# تحديث تكوين العروض
```

### مسارات Google Sheets

```http
POST /api/sheets/export-order
# تصدير طلب إلى Google Sheets

POST /api/sheets/auto-export-order
# تصدير تلقائي للطلب
```

## 🎯 كيفية الاستخدام

### تدفق الطلب الأساسي

1. **اختيار المنتجات:**
   - يختار العميل المنتجات المطلوبة
   - يحدد الكميات
   - يراجع المجموع

2. **عروض Upsell (اختياري):**
   - يتم عرض منتجات إضافية مع خصومات
   - يمكن للعميل إضافة أو تخطي العروض

3. **معلومات الطلب:**
   - إدخال الاسم الكامل
   - إدخال رقم الهاتف
   - إدخال العنوان الكامل

4. **التحقق من الهاتف:**
   - إرسال رمز التحقق عبر SMS
   - إدخال الرمز للتأكيد
   - إمكانية إعادة الإرسال

5. **تأكيد الطلب:**
   - عرض ملخص الطلب
   - إنشاء Draft Order في Shopify
   - تصدير البيانات إلى Google Sheets
   - عرض رقم الطلب والتفاصيل

### إدارة عروض Upsell

يمكن تخصيص عروض Upsell من خلال ملف التكوين:

```json
{
  "enabled": true,
  "rules": [
    {
      "trigger_product_ids": ["*"],
      "upsell_products": [
        {
          "product_id": "upsell_accessory_1",
          "title": "إكسسوار مميز",
          "price": 75.0,
          "discount_percentage": 25,
          "description": "احصل على خصم 25% على هذا الإكسسوار المميز"
        }
      ],
      "max_offers": 3
    }
  ]
}
```

### تخصيص التصميم

يمكن تخصيص التصميم من خلال:

1. **ألوان Tailwind:** تعديل ملف `tailwind.config.js`
2. **مكونات React:** تعديل المكونات في مجلد `src/components`
3. **أنماط CSS:** إضافة أنماط مخصصة في `src/index.css`


## ☁️ النشر على الخدمات السحابية

### النشر على Vercel (Frontend)

1. **ربط المشروع بـ Vercel:**
   ```bash
   npm install -g vercel
   cd easysell-app
   vercel
   ```

2. **إعداد متغيرات البيئة في Vercel:**
   - `VITE_BACKEND_API_URL`: رابط Backend API

3. **إعداد Build Commands:**
   - Build Command: `npm run build`
   - Output Directory: `dist`

### النشر على Railway (Backend)

1. **إنشاء مشروع جديد في Railway:**
   - اربط مستودع GitHub
   - اختر مجلد `easysell-backend`

2. **إعداد متغيرات البيئة:**
   - أضف جميع متغيرات البيئة المطلوبة
   - تأكد من إعداد `PORT=5000`

3. **إعداد النشر:**
   - Railway سيتعرف تلقائياً على `Dockerfile`
   - أو يمكن استخدام `requirements.txt`

### النشر على Render

#### Backend على Render

1. **إنشاء Web Service جديد:**
   - اربط مستودع GitHub
   - اختر مجلد `easysell-backend`

2. **إعدادات النشر:**
   - Build Command: `pip install -r requirements.txt`
   - Start Command: `python -m src.main`
   - Environment: `Python 3.11`

#### Frontend على Render

1. **إنشاء Static Site جديد:**
   - اربط مستودع GitHub
   - اختر مجلد `easysell-app`

2. **إعدادات البناء:**
   - Build Command: `npm run build`
   - Publish Directory: `dist`

### النشر على Heroku

#### Backend على Heroku

1. **إنشاء تطبيق Heroku:**
   ```bash
   heroku create easysell-backend
   ```

2. **إعداد Procfile:**
   ```
   web: python -m src.main
   ```

3. **نشر التطبيق:**
   ```bash
   git subtree push --prefix=easysell-backend heroku main
   ```

#### Frontend على Heroku

1. **إعداد buildpack:**
   ```bash
   heroku buildpacks:set https://github.com/mars/create-react-app-buildpack.git
   ```

2. **نشر التطبيق:**
   ```bash
   git subtree push --prefix=easysell-app heroku main
   ```

## 🔧 استكشاف الأخطاء

### مشاكل شائعة وحلولها

#### 1. خطأ في الاتصال بـ Shopify

```
Error: Failed to fetch products from Shopify
```

**الحل:**
- تأكد من صحة `SHOPIFY_ACCESS_TOKEN`
- تحقق من صلاحيات API
- تأكد من أن المتجر نشط

#### 2. خطأ في إرسال SMS

```
Error: Unable to send SMS
```

**الحل:**
- تحقق من رصيد Twilio
- تأكد من صحة أرقام الهاتف
- تحقق من إعدادات الرقم المرسل

#### 3. خطأ في Google Sheets

```
Error: Google Sheets authentication failed
```

**الحل:**
- تحقق من صحة Service Account keys
- تأكد من مشاركة الجدول مع Service Account
- تحقق من تفعيل Google Sheets API

#### 4. مشاكل CORS

```
Error: CORS policy blocked
```

**الحل:**
- تأكد من إعداد Flask-CORS بشكل صحيح
- تحقق من رابط Frontend في إعدادات CORS
- استخدم HTTPS في النشر

### سجلات الأخطاء

#### Backend Logs

```bash
# في التطوير
python -m src.main

# في Docker
docker-compose logs backend

# في الإنتاج
tail -f /var/log/easysell-backend.log
```

#### Frontend Logs

```bash
# في التطوير
npm run dev

# في Docker
docker-compose logs frontend

# في المتصفح
F12 -> Console
```


## 🔐 الأمان

### أفضل الممارسات الأمنية

1. **متغيرات البيئة:**
   - لا تشارك ملف `.env` أبداً
   - استخدم مفاتيح قوية ومعقدة
   - قم بتدوير المفاتيح بانتظام

2. **HTTPS:**
   - استخدم HTTPS في الإنتاج دائماً
   - فعّل SSL/TLS certificates
   - استخدم HSTS headers

3. **التحقق من البيانات:**
   - تحقق من جميع المدخلات
   - استخدم validation على الخادم والعميل
   - فلتر البيانات الحساسة

4. **معدل الطلبات:**
   - طبق rate limiting على APIs
   - استخدم CAPTCHA عند الحاجة
   - راقب الطلبات المشبوهة

### إعدادات الأمان الموصى بها

```python
# في Flask
app.config['SESSION_COOKIE_SECURE'] = True
app.config['SESSION_COOKIE_HTTPONLY'] = True
app.config['SESSION_COOKIE_SAMESITE'] = 'Lax'
```

```nginx
# في Nginx
add_header X-Frame-Options "SAMEORIGIN" always;
add_header X-XSS-Protection "1; mode=block" always;
add_header X-Content-Type-Options "nosniff" always;
```

## 📊 المراقبة والتحليلات

### مؤشرات الأداء الرئيسية

1. **معدل التحويل:**
   - نسبة الزوار الذين يكملون الطلب
   - متوسط قيمة الطلب
   - تأثير عروض Upsell

2. **الأداء التقني:**
   - زمن استجابة APIs
   - معدل نجاح إرسال SMS
   - معدل نجاح تصدير البيانات

3. **تجربة المستخدم:**
   - زمن تحميل الصفحات
   - معدل الارتداد
   - نقاط الخروج الشائعة

### أدوات المراقبة الموصى بها

- **Google Analytics:** لتتبع سلوك المستخدمين
- **Sentry:** لمراقبة الأخطاء
- **New Relic:** لمراقبة الأداء
- **Uptime Robot:** لمراقبة توفر الخدمة

## 🤝 المساهمة

نرحب بالمساهمات من المجتمع! إليك كيفية المساهمة:

### خطوات المساهمة

1. **Fork المشروع**
2. **إنشاء branch جديد:**
   ```bash
   git checkout -b feature/amazing-feature
   ```
3. **إجراء التغييرات والاختبار**
4. **Commit التغييرات:**
   ```bash
   git commit -m 'Add some amazing feature'
   ```
5. **Push إلى Branch:**
   ```bash
   git push origin feature/amazing-feature
   ```
6. **إنشاء Pull Request**

### إرشادات المساهمة

- اتبع معايير الكود الموجودة
- أضف اختبارات للميزات الجديدة
- حدّث التوثيق عند الحاجة
- اكتب commit messages واضحة

### أنواع المساهمات المرحب بها

- 🐛 إصلاح الأخطاء
- ✨ ميزات جديدة
- 📚 تحسين التوثيق
- 🎨 تحسينات التصميم
- ⚡ تحسينات الأداء
- 🔒 تحسينات الأمان

## 📝 الترخيص

هذا المشروع مرخص تحت رخصة MIT - راجع ملف [LICENSE](LICENSE) للتفاصيل.

## 🙏 شكر وتقدير

- **Shopify** لتوفير APIs قوية ومرنة
- **Twilio** لخدمات SMS الموثوقة
- **Google** لخدمات Sheets API
- **React** و **Flask** للأطر التقنية الممتازة
- **Tailwind CSS** للتصميم السريع والجميل

## 📞 الدعم والتواصل

- **البريد الإلكتروني:** support@selena.ma
- **الموقع الرسمي:** https://selena.ma
- **التوثيق:** راجع هذا الملف
- **المشاكل:** استخدم GitHub Issues

---

**تم تطوير هذا المشروع بـ ❤️ للمجتمع العربي**

© 2024 EasySell COD - نظام الطلب السريع مع الدفع عند الاستلام | مدعوم بواسطة Shopify

