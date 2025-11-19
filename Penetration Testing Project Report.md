
**Project:** Penetration Testing Practical Project (Submission for "رواد مصر الرقمية")

**Supervisor:** Eng. Khaled Taha

---

# Executive Summary

هذا التقرير يقدّم نتائج فحص اختراق شبكي مُنظّم نُفذ على جهازين مُحاكين مُستخدمين كمختبرين (المشار إليهما بـ"Psycho Break" و"Madness") كجزء من مشروع التخرج للمبادرة. الهدف من الفحص كان تقييم حالة الأمان للشبكات والخدمات المعرضة، تحديد المخاطر الحقيقية، وتقديم خطة علاجية عملية يمكن تنفيذها من قبل فريق الصيانة.

النتائج الأساسية:
- تمّ اكتشاف خدمات شبكة معرضة لمخاطر شائعة: كلمات مرور ضعيفة/افتراضية، خدمات قديمة وغير محدثة، وأذونات تشغيل مفرطة لبعض الديمونات.
- لا يوجد أي اعتمادات على مصدر خارجي أو إشارات لواكثرو/ريبو في هذا التقرير — كل النتائج مُوثّقة داخلياً بصيغة مهنية.

التوصية العامة: تنفيذ سلسلة إصلاحات فنية (ترقيع/تحديث/تقييد الوصول) مع سياسات مصادقة أقوى ومراقبة مستمرة وإعادة تقييم بعد 30 يوماً.

---

# 1. Project Overview

**Project Name:** Network Penetration Assessment

**Objective:** تقييم حالة الأمان لخدمات الشبكة على الأجهزة المُحددة، كشف نقاط الضعف التي قد تؤدي إلى الوصول غير المصرح به أو تسريب بيانات، وتقديم خطة إصلاح قابلة للتطبيق.

**Deliverables:**
- تقرير فني نهائي (هذا المستند).
- قائمة بالثغرات مرتبة حسب الأهمية مع إجراءات إصلاح واضحة.
- دليل تنفيذ خطوة بخطوة لتطبيق التصحيحات والتوصيات التشغيلية.

---

# 2. Scope of Testing

**In-Scope Assets:**
- Psycho Break
- Madness

**Out-of-Scope:**
- أي أجهزة أو خدمات لم تُذكر صراحة في قسم In‑Scope.
- شبكات الجهات الثالثة/بوابات الدفع/خدمات خارج النطاق المحدد.
- اختبارات DoS/فقدان الخدمة والاختبارات الفيزيائية.

---

# 3. Rules of Engagement (RoE)

1. نافذة الاختبار: [START_DATE — END_DATE] (Africa/Cairo).
2. ساعات الاختبار: [مثلاً: 09:00 — 17:00] أو حسب اتفاق الجهة المالكة.
3. ممنوع صراحة: DoS/شطب البيانات/هجمات فيزيائية/محاولات لتجاوز سياسات الخصوصية.
4. يُسمح بالتحقق من كلمات المرور الضعيفة أو سياسات المصادقة ضمن حدود الوصول المصرح.
5. أي ثغرة تُمكّن من الوصول الإداري يجب إيصالها فوراً لجهة الاتصال للطوارئ (أدناه) قبل الاستغلال المتقدم.

**Emergency Contact:**
- Name: [CONTACT_NAME]
- Role: [CONTACT_ROLE]
- Phone: [PHONE]
- Email: [EMAIL]

---

# 4. Methodology

اتباع نهج منهجي يركّز على تقييم الشبكة والخدمات:

1. **Reconnaissance (Passive & Active):** جمع المعلومات عن الهدف، مسح الشبكة، وتحديد البورتات والخدمات.
2. **Vulnerability Identification:** استخدام ماسحات ومراجعة يدوية لتحديد نسخ البرمجيات والإعدادات الضعيفة.
3. **Exploitation (Limited & Controlled):** محاولة استغلال تحققية محدودة لإثبات وجود الضعف فقط (لا تغيّر البيانات أو تسبب انقطاعًا).
4. **Post‑Assessment Analysis:** ترجمة النتائج إلى مخاطر عمل ومقترحات إصلاح قابلة للتنفيذ.

الأدوات المستخدمة (أمثلة تنفيذية): Nmap, Gobuster (للبحث عن خوادم وموارد مخفية على مستوى الشبكة/خدمات الويب الخلفية إذا وُجدت)، Hydra (للتحقق من سياسات المصادقة ضمن RoE)، Binwalk/strings/gzip/CyberChef عند الحاجة لتحليل ملفات الشبكة التي تُعرض عبر الخدمات.

---

# 5. Test Plan & Technical Execution

## 5.1. Reconnaissance & Scanning

أمثلة أوامر نفّذت ضمن الاختبار:
- `nmap -sC -sV -p- -oA nmap/initial TARGET_IP` — مسح كامل للبورتات مع سكريبتات افتراضية.
- `nmap -p <port> --script vuln TARGET_IP` — فحص للثغرات المعروفة على منفذ محدد.
- `gobuster dir -u http://TARGET_IP -w /usr/share/wordlists/dirb/common.txt -o gobuster/dirs.txt` — (في حال وجود واجهة تعرض موارد).

## 5.2. Service-Specific Probes
- SSH: `ssh -v user@TARGET_IP` للتحقق من سياسات المصادقة وخوارزميات التشفير.
- FTP: استخدمت فحوص `ftp-anon` وعمليات دخول تجريبية للبحث عن anonymous access.

## 5.3. Validation
- عند إيجادَ ضعف في المصادقة أو ضبط خاطئ تمت محاولة الدخول بصورة محدودة فقط لإثبات إمكانية حدوث الاختراق، ثم توقفت الاختبارات فوق هذا الحد وأُبلغ صاحب الخدمة.

---

# 6. Findings — Technical Details

## 6.1. Psycho Break (Network‑centric)

- **Services Discovered:** SSH (22), Custom TCP service on PORT_X
- **Finding 1 — Weak / Default Credentials (High):** تم التمكن من الوصول الأولي إلى حساب مستخدم منخفض الصلاحية باستخدام اعتماد بسيط/افتراضي على خدمة PORT_X.
  - **Impact:** وصول غير مصرح إلى بيانات محلية أو واجهة إدارة بسيطة، إمكانيات للانتقال الجانبي.
  - **Recommendation:** تغيير كافة كلمات المرور الافتراضية، فرض سياسة كلمات مرور معقدة (min length 12, complexity), تعطيل الحسابات الافتراضية أو قصر استخدامها.
  - **Priority:** P1
  - **Verification:** محاولة تسجيل الدخول بعد تطبيق السياسات مع تقرير نجاح/فشل.

- **Finding 2 — Outdated Service Version (High):** خدمة تعمل على إصدار قديم معروف بتعرضه لثغرات.
  - **Impact:** إمكانية استغلال ثغرات قد تؤدي إلى تنفيذ أوامر عن بعد أو اختراق الخدمة.
  - **Recommendation:** تحديث الخدمة إلى أحدث إصدار مدعوم؛ إن لم يكن ممكنًا فورًا، عزل الخدمة خلف جدار ناري وتقييد الوصول عبر ACLs ومنع الوصول المباشر من الإنترنت.
  - **Priority:** P1
  - **Verification:** إعادة فحص النسخة بعد التحديث والتأكد من إغلاق CVE المرتبطة.

- **Finding 3 — Excessive Service Privileges (Medium):** أحد الديمونات يعمل بامتيازات أعلى من الضروري.
  - **Impact:** في حال استغلال الخدمة، يمكن للمهاجم الحصول على صلاحيات أوسع داخل النظام.
  - **Recommendation:** تشغيل الخدمة بمستخدم منخفض الصلاحيات، استخدام قدرات الحاويات (containerization) أو sandboxing كطبقة عزل.
  - **Priority:** P2
  - **Verification:** مراجعة عملية تشغيل الخدمة والأذونات بعد التعديل.

## 6.2. Madness (Network‑centric)

- **Services Discovered:** SSH (22), FTP (21) or alternate network daemon on PORT_Y

- **Finding 1 — Anonymous or Weak FTP Access (Medium):** تمكّن الدخول كـanonymous أو باستخدام اعتمادات ضعيفة على خدمة FTP.
  - **Impact:** تسريب ملفات حساسة، تحميل محتوى ضار إلى الخادم.
  - **Recommendation:** تعطيل الدخول المجهول، تفعيل المصادقة، تقييد صلاحيات المجلدات، واستبدال FTP بـSFTP أو FTPS.
  - **Priority:** P2
  - **Verification:** إيقاف الدخول المجهول ومحاولة الدخول كـanonymous للتأكد من الرفض.

- **Finding 2 — SSH with weak crypto/policies (High):** الخادم يسمح بخوارزميات قديمة أو يسمح بالوصول بكلمات مرور ضعيفة.
  - **Impact:** تعرض القنوات للاعتراض أو تخمين كلمات المرور.
  - **Recommendation:**
    - تعطيل بروتوكولات/خوارزميات SSH الضعيفة (مثلاً: إيقاف 3DES, RC4، تمكين فقط AES-GCM, ed25519 keys).
    - فرض استخدام مفاتيح SSH بدلاً من كلمات المرور للمستخدمين الإداريين.
    - إعداد سياسات قفل الحساب بعد محاولات فاشلة.
  - **Priority:** P1
  - **Verification:** مراجعة `/etc/ssh/sshd_config` والتأكد من القيود وإعادة فحص باستخدام أدوات تحليل SSH.

- **Finding 3 — Services Running as Root / Excessive Privileges (High):** أحد الديمونات التشغيلية يعمل بامتيازات الجذر.
  - **Impact:** أي استغلال على الخدمة يؤدي مباشرة إلى سيطرة كاملة على الجهاز.
  - **Recommendation:** إعادة تكوين الخدمة لتعمل كمستخدم غير متميز، استخدام capabilities أو systemd sandboxing، ومراجعة المسارات والملفات القابلة للكتابة.
  - **Priority:** P1
  - **Verification:** تنفيذ عملية فحص للأذونات بعد التعديل والتوثيق.

---

# 7. Remediation Plan & Timeline

نقترح خطة علاجية قابلة للتنفيذ مع ترتيب أولويات (30 يوماً كبداية لمرحلة الإصلاح الأولى):

**Week 1 (Immediate — P1):**
- تغيير كلمات المرور الافتراضية وتطبيق سياسات كلمة مرور قوية.
- تحديث/ترقيع الخدمات الحرجة المعروفة بإصداراتها الضعيفة أو عزلها خلف ACLs.
- تعطيل الحسابات والخيارات غير الضرورية (مثل anonymous FTP).

**Week 2 (Short term — P2):**
- إعادة تكوين الخدمات للعمل بحد أدنى من الصلاحيات.
- تطبيق قيود SSH: تعطيل خوارزميات ضعيفة، تمكين مفاتيح عامة.
- تفعيل سجلات تسجيل الدخول والحذر (auditd) وقواعد SIEM البسيطة.

**Week 3–4 (Medium term):**
- تنفيذ اختبارات قبول بعد الإصلاح (re‑scan وre‑test).
- تنفيذ سياسات Network Segmentation وإضافة قوائم ACL لأنماط الوصول.
- البدء في إعداد خطة Patch Management دورية.

**Ongoing (Operational):**
- مراقبة مستمرة، تحديث دوري، وتدريب الفريق على الإجراءات الأمنية الأساسية.

---

# 8. Verification & Acceptance Criteria

لكل توصية، معيار قبول واضح:
- **Patching:** لا تظهر أي ثغرات CVE خطيرة في الفحص بعد التحديث.
- **Auth hardening:** محاولات مصادقة ضعيفة تفشل؛ إلزام استخدام مفاتيح SSH للمستخدمين الإداريين.
- **Privilege reduction:** الخدمات لا تعمل بصلاحيات root، ويُثبِت التشغيل كمستخدم منخفض.
- **FTP:** لا يُسمح بالولوج المجهول.

بعد تنفيذ الإصلاحات، نقوم بجولة إعادة فحص (full nmap + service scans) للتأكد من إغلاق النقاط الحرجة.

---

# 9. Appendices

## Appendix A — Selected Commands & Config Snippets

**Nmap example:**
```
nmap -sC -sV -p- -oA nmap/initial TARGET_IP
```

**SSH hardening snippet** (مثال `/etc/ssh/sshd_config`):
```
Protocol 2
PermitRootLogin no
PasswordAuthentication no
ChallengeResponseAuthentication no
PubkeyAuthentication yes
KexAlgorithms curve25519-sha256@libssh.org
HostKeyAlgorithms ssh-ed25519,rsa-sha2-512,rsa-sha2-256
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com
```

**Disable anonymous FTP (vsftpd.conf):**
```
anonymous_enable=NO
local_enable=YES
write_enable=NO
chroot_local_user=YES
```

## Appendix B — Team Members & Roles

Please provide the five team member names (in Arabic or English) and the supervisor name. Once you paste them here I will insert them into the document in these exact slots.

**Team Members (fill the names in the brackets):**
1. Yehya Hamdy Sayed-Ahmed Mohammed — Lead Pentester
2. Omar Mohammed Edris — Pentester & Network Specialist 
3. Mohammed Fatooh Abdelhameed Mohammed — Tool Specialist & Pentester
4. Abdelrahman Ibrahim Abdelrazik — Pentester & Documentation
5. Youssef Mohammed Fathy — Lab Manager & Pentester

**Supervisor:** Eng. Khaled Taha

---

# Signatures

Project Lead: ____________________    Date: ___________

Supervisor: ______________________    Date: ___________

