# الاتصال البرمجي بالمستودع
### Programmatic access

---

## 1) احفظ المفتاح الخاص

احفظ المفتاح الذي استلمته في ملف (مثلاً `~/.ssh/keif_output_key`) ثم:

```bash
chmod 600 ~/.ssh/keif_output_key
```

⚠️ لا تشارك المفتاح ولا ترفعه في أي مستودع.

---

## 2) استنسخ المستودع

```bash
GIT_SSH_COMMAND="ssh -i ~/.ssh/keif_output_key -o IdentitiesOnly=yes -o StrictHostKeyChecking=accept-new" \
  git clone git@github.com:moain2026/keif-aldiyafa-output.git

cd keif-aldiyafa-output
```

---

## 3) اجعل المفتاح دائماً لهذا المستودع

```bash
git config core.sshCommand "ssh -i ~/.ssh/keif_output_key -o IdentitiesOnly=yes"
git config user.name  "Creative Agent"
git config user.email "agent@keifaldiafa.com"
```

بعدها كل `git push` يعمل مباشرة بلا متغيرات إضافية.

---

## 4) ارفع مخرجاتك

```bash
cp my-reel.mp4 reels/2026-08-01_reel_قهوة-الترحيب.mp4
cp my-reel.md  reels/2026-08-01_reel_قهوة-الترحيب.md

git add -A
git commit -m "reels: إضافة ريل قهوة الترحيب"
git push
```

---

## 5) تحقّق من الاتصال

```bash
ssh -i ~/.ssh/keif_output_key -T git@github.com
```

الرد المتوقّع:
```
Hi moain2026/keif-aldiyafa-output! You've successfully authenticated,
but GitHub does not provide shell access.
```

---

## قراءة المواد الخام

المواد **عامة** ولا تحتاج مفتاحاً:

```bash
curl -s https://raw.githubusercontent.com/moain2026/keif-aldiyafa-media/main/index.json -o index.json

python3 - <<'PY'
import json, urllib.request, os
d = json.load(open('index.json'))
print(d['totals'])

# مثال: تنزيل أول 5 فيديوهات
vids = [f for f in d['files'] if f.get('kind') == 'video'][:5]
os.makedirs('raw', exist_ok=True)
for v in vids:
    urllib.request.urlretrieve(v['url'], 'raw/' + os.path.basename(v['file']))
    print('✓', v['file'], v.get('duration'), 's')
PY
```

**فلترة سريعة:**

```python
logos  = [f for f in d['files'] if f['source'] == 'brand']
svgs   = [f for f in logos if f.get('format') == 'vector']
events = [f for f in d['files'] if f.get('category') == 'events']
short  = [f for f in d['files'] if (f.get('duration') or 0) < 8]
```

---

## حدود المفتاح

| | |
|---|---|
| النطاق | هذا المستودع فقط |
| الصلاحية | قراءة + كتابة |
| الوصول لباقي حساب المالك | ❌ لا يوجد |
| الإلغاء | `gh repo deploy-key delete 158432384 --repo moain2026/keif-aldiyafa-output` |
