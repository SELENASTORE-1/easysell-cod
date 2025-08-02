# دليل النشر المفصل - EasySell COD

هذا الدليل يوضح خطوات النشر التفصيلية للتطبيق على مختلف المنصات السحابية.

## 🎯 خيارات النشر الموصى بها

### الخيار الأول: Vercel + Railway (موصى به)
- **Frontend:** Vercel (مجاني)
- **Backend:** Railway (مجاني مع حدود)
- **قاعدة البيانات:** Railway PostgreSQL

### الخيار الثاني: Netlify + Render
- **Frontend:** Netlify (مجاني)
- **Backend:** Render (مجاني مع حدود)
- **قاعدة البيانات:** Render PostgreSQL

### الخيار الثالث: Docker على VPS
- **الخادم:** DigitalOcean/Linode/Vultr
- **الحاويات:** Docker Compose
- **قاعدة البيانات:** PostgreSQL في حاوية

## 🚀 النشر على Vercel + Railway

### الخطوة 1: إعداد Railway (Backend)

1. **إنشاء حساب Railway:**
   - اذهب إلى [railway.app](https://railway.app)
   - سجل باستخدام GitHub

2. **إنشاء مشروع جديد:**
   ```bash
   # في مجلد المشروع
   npm install -g @railway/cli
   railway login
   railway init
   ```

3. **إعداد قاعدة البيانات:**
   ```bash
   railway add postgresql
   ```

4. **رفع Backend:**
   ```bash
   cd easysell-backend
   railway up
   ```

5. **إعداد متغيرات البيئة في Railway:**
   ```bash
   railway variables set FLASK_ENV=production
   railway variables set SECRET_KEY=your-secret-key
   railway variables set SHOPIFY_STORE_URL=selenastor.myshopify.com
   railway variables set SHOPIFY_ACCESS_TOKEN=your-token
   railway variables set TWILIO_ACCOUNT_SID=your-sid
   railway variables set TWILIO_AUTH_TOKEN=your-token
   railway variables set TWILIO_PHONE_NUMBER=+2126xxxxxxx
   railway variables set GOOGLE_SHEETS_CLIENT_EMAIL=your-email
   railway variables set GOOGLE_SHEETS_PRIVATE_KEY="your-private-key"
   railway variables set GOOGLE_SHEETS_SPREADSHEET_ID=your-id
   ```

### الخطوة 2: إعداد Vercel (Frontend)

1. **تثبيت Vercel CLI:**
   ```bash
   npm install -g vercel
   ```

2. **نشر Frontend:**
   ```bash
   cd easysell-app
   vercel
   ```

3. **إعداد متغيرات البيئة:**
   - اذهب إلى Vercel Dashboard
   - اختر المشروع
   - اذهب إلى Settings > Environment Variables
   - أضف: `VITE_BACKEND_API_URL=https://your-railway-backend-url.railway.app`

4. **إعادة النشر:**
   ```bash
   vercel --prod
   ```

## 🔧 النشر على DigitalOcean باستخدام Docker

### الخطوة 1: إعداد الخادم

1. **إنشاء Droplet:**
   - اختر Ubuntu 22.04
   - حجم 2GB RAM على الأقل
   - إعداد SSH key

2. **الاتصال بالخادم:**
   ```bash
   ssh root@your-server-ip
   ```

3. **تثبيت Docker:**
   ```bash
   apt update
   apt install -y docker.io docker-compose
   systemctl start docker
   systemctl enable docker
   ```

### الخطوة 2: رفع المشروع

1. **استنساخ المشروع:**
   ```bash
   git clone https://github.com/your-username/easysell-clone.git
   cd easysell-clone
   ```

2. **إعداد متغيرات البيئة:**
   ```bash
   cp .env.example .env
   nano .env
   # قم بتعديل جميع القيم
   ```

3. **بناء وتشغيل الحاويات:**
   ```bash
   docker-compose up -d
   ```

### الخطوة 3: إعداد Nginx Reverse Proxy

1. **تثبيت Nginx:**
   ```bash
   apt install -y nginx
   ```

2. **إعداد التكوين:**
   ```bash
   nano /etc/nginx/sites-available/easysell
   ```

   ```nginx
   server {
       listen 80;
       server_name your-domain.com;

       location / {
           proxy_pass http://localhost:80;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header X-Forwarded-Proto $scheme;
       }

       location /api/ {
           proxy_pass http://localhost:5000/api/;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header X-Forwarded-Proto $scheme;
       }
   }
   ```

3. **تفعيل التكوين:**
   ```bash
   ln -s /etc/nginx/sites-available/easysell /etc/nginx/sites-enabled/
   nginx -t
   systemctl reload nginx
   ```

### الخطوة 4: إعداد SSL

1. **تثبيت Certbot:**
   ```bash
   apt install -y certbot python3-certbot-nginx
   ```

2. **الحصول على شهادة SSL:**
   ```bash
   certbot --nginx -d your-domain.com
   ```

## 📱 النشر على Heroku

### Backend على Heroku

1. **تثبيت Heroku CLI:**
   ```bash
   # على Ubuntu/Debian
   curl https://cli-assets.heroku.com/install.sh | sh
   ```

2. **تسجيل الدخول:**
   ```bash
   heroku login
   ```

3. **إنشاء تطبيق:**
   ```bash
   cd easysell-backend
   heroku create easysell-backend-app
   ```

4. **إعداد متغيرات البيئة:**
   ```bash
   heroku config:set FLASK_ENV=production
   heroku config:set SECRET_KEY=your-secret-key
   heroku config:set SHOPIFY_STORE_URL=selenastor.myshopify.com
   # ... باقي المتغيرات
   ```

5. **إعداد قاعدة البيانات:**
   ```bash
   heroku addons:create heroku-postgresql:hobby-dev
   ```

6. **نشر التطبيق:**
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   heroku git:remote -a easysell-backend-app
   git push heroku main
   ```

### Frontend على Heroku

1. **إعداد buildpack:**
   ```bash
   cd easysell-app
   heroku create easysell-frontend-app
   heroku buildpacks:set mars/create-react-app
   ```

2. **إعداد متغيرات البيئة:**
   ```bash
   heroku config:set VITE_BACKEND_API_URL=https://easysell-backend-app.herokuapp.com
   ```

3. **نشر التطبيق:**
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   heroku git:remote -a easysell-frontend-app
   git push heroku main
   ```

## 🔍 اختبار النشر

### اختبارات ما بعد النشر

1. **اختبار Frontend:**
   ```bash
   curl -I https://your-frontend-url.com
   ```

2. **اختبار Backend APIs:**
   ```bash
   curl https://your-backend-url.com/api/orders
   curl https://your-backend-url.com/api/shopify/products
   ```

3. **اختبار تكامل SMS:**
   ```bash
   curl -X POST https://your-backend-url.com/api/sms/send-verification \
     -H "Content-Type: application/json" \
     -d '{"phone": "+212600000000"}'
   ```

4. **اختبار Google Sheets:**
   ```bash
   curl -X POST https://your-backend-url.com/api/sheets/export-order \
     -H "Content-Type: application/json" \
     -d '{"order_id": "test-order"}'
   ```

### مراقبة الصحة

1. **إعداد Health Checks:**
   ```bash
   # إضافة إلى crontab
   */5 * * * * curl -f https://your-app.com/health || echo "App is down"
   ```

2. **مراقبة السجلات:**
   ```bash
   # Railway
   railway logs

   # Heroku
   heroku logs --tail

   # Docker
   docker-compose logs -f
   ```

## 🛠️ صيانة النشر

### النسخ الاحتياطية

1. **نسخ احتياطي لقاعدة البيانات:**
   ```bash
   # PostgreSQL
   pg_dump $DATABASE_URL > backup.sql

   # Railway
   railway db backup

   # Heroku
   heroku pg:backups:capture
   ```

2. **نسخ احتياطي للملفات:**
   ```bash
   tar -czf easysell-backup-$(date +%Y%m%d).tar.gz easysell-clone/
   ```

### التحديثات

1. **تحديث Backend:**
   ```bash
   cd easysell-backend
   git pull origin main
   
   # Railway
   railway up
   
   # Heroku
   git push heroku main
   
   # Docker
   docker-compose build backend
   docker-compose up -d backend
   ```

2. **تحديث Frontend:**
   ```bash
   cd easysell-app
   git pull origin main
   
   # Vercel
   vercel --prod
   
   # Heroku
   git push heroku main
   
   # Docker
   docker-compose build frontend
   docker-compose up -d frontend
   ```

### مراقبة الأداء

1. **مراقبة استخدام الموارد:**
   ```bash
   # Docker
   docker stats
   
   # System resources
   htop
   df -h
   ```

2. **مراقبة السجلات:**
   ```bash
   # تصفية الأخطاء
   grep -i error /var/log/easysell/*.log
   
   # مراقبة الطلبات
   tail -f /var/log/nginx/access.log
   ```

## 🚨 استكشاف أخطاء النشر

### مشاكل شائعة

1. **خطأ 500 في Backend:**
   - تحقق من متغيرات البيئة
   - راجع سجلات الخطأ
   - تأكد من اتصال قاعدة البيانات

2. **خطأ CORS:**
   - تحقق من إعدادات Flask-CORS
   - تأكد من رابط Frontend الصحيح

3. **فشل في تحميل المنتجات:**
   - تحقق من Shopify API credentials
   - تأكد من صلاحيات API

4. **فشل في إرسال SMS:**
   - تحقق من رصيد Twilio
   - تأكد من صحة أرقام الهاتف

### أدوات التشخيص

```bash
# اختبار الاتصال
ping your-domain.com

# اختبار DNS
nslookup your-domain.com

# اختبار SSL
openssl s_client -connect your-domain.com:443

# اختبار البورتات
telnet your-server-ip 80
telnet your-server-ip 443
```

---

هذا الدليل يغطي معظم سيناريوهات النشر الشائعة. لمزيد من المساعدة، راجع التوثيق الرسمي لكل منصة.

