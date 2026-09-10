# كتابة قواعد كشف مخصصة (Custom Detection Rules)

## نظرة عامة

تصميم واختبار قواعد Wazuh مخصصة تتجاوز القواعد الافتراضية، لمعالجة احتياجات كشف محددة غير مغطاة تلقائياً.

**الأدوات المستخدمة:** Wazuh Rules Engine، XML، `xmllint` للتحقق من الصحة

## القاعدة الأولى: تنبيه عالي لملف حساس محدد

```xml
<rule id="100002" level="12">
  <if_group>syscheck</if_group>
  <field name="file">/etc/critical_config_test.conf</field>
  <description>تحذير: تم تعديل ملف حساس جداً!</description>
  <group>syscheck,</group>
</rule>
```

**تحدٍ تقني وحلّه:** المحاولة الأولى استخدمت `<if_sid>554</if_sid>` (يخص حدث "إضافة ملف" فقط)، فلم تُفعَّل القاعدة عند **تعديل** الملف (حدث مختلف، rule.id 550). الحل: استبدالها بـ `<if_group>syscheck</if_group>` لتغطية كل أحداث FIM (إضافة/تعديل/حذف) دفعة واحدة.

## القاعدة الثانية: كشف الأنماط المتكررة (Correlation Rule)

```xml
<rule id="100003" level="10" frequency="5" timeframe="60">
  <if_matched_sid>5760</if_matched_sid>
  <description>تحذير: 5 محاولات SSH فاشلة خلال دقيقة - احتمال Brute-force!</description>
  <group>authentication_failures,</group>
</rule>
```

**الفجوة المكتشفة:** القواعد الافتراضية تربط محاولات `sudo` الفاشلة المتكررة تلقائياً (rule.id 5404)، لكن لا توجد قاعدة مكافئة لمحاولات SSH الفاشلة — كل محاولة تُسجَّل منفصلة دون تجميع.

**عناصر بناء قاعدة ارتباط:**
| العنصر | الوظيفة |
|---|---|
| `frequency` | عدد التكرارات المطلوبة لتفعيل القاعدة |
| `timeframe` | النافذة الزمنية بالثواني لحساب التكرار |
| `if_matched_sid` | (وليس `if_sid`) — ضروري لتفعيل منطق الحساب التراكمي |

## Active Response — أتمتة الاستجابة

ربط قاعدة مخصصة بسكربت استجابة تلقائي:

```xml
<command>
  <name>log_alert_test</name>
  <executable>log_alert_test.sh</executable>
  <timeout_allowed>no</timeout_allowed>
</command>

<active-response>
  <command>log_alert_test</command>
  <location>local</location>
  <rules_id>100002</rules_id>
</active-response>
```

تم اختبار السلسلة الكاملة: **تعديل ملف → كشف FIM → تفعيل القاعدة المخصصة → تنفيذ سكربت تلقائي** خلال أقل من 15 ثانية، دون أي تدخل بشري.

## مبدأ أمني مهم: Human-in-the-loop

الاختبار استخدم إجراء آمن (كتابة سطر في ملف log) عمداً، وليس إجراءً فعلياً (حظر IP، إيقاف عملية). في بيئات الإنتاج الحقيقية، القرارات عالية الأثر تمر عبر تحقق بشري لتجنب أضرار False Positive (مثل حظر IP شرعي).

## الدرس المنهجي

بناء قاعدة كشف هو عملية تكرارية: كتابة → اختبار فعلي → اكتشاف قصور → تعديل دقيق. نادراً ما تكون القاعدة الأولى مثالية — هذا نمط عمل طبيعي لمهندسي الكشف (Detection Engineers).
