# 🛡️ SOC Analyst Portfolio

السلام عليكم ورحمة الله وبركاته 
أنا خريجه أمن سيبراني، هذا المستودع يوثق مشاريع عملية طبّقتها بنفسي في تحليل حركة الشبكة، بناء وإدارة أنظمة SIEM، وكتابة قواعد كشف مخصصة.

## 🔧 المهارات التقنية

- **تحليل حركة الشبكة (Network Traffic Analysis):** Wireshark — TCP/DNS/TLS handshakes، Follow Stream، Conversations
- **SIEM:** تنصيب وإعداد Wazuh من الصفر (Manager, Dashboard, Rules Engine)
- **كتابة قواعد كشف مخصصة (Custom Detection Rules):** بناء قواعد ارتباط (Correlation Rules) باستخدام frequency وtimeframe
- **File Integrity Monitoring (FIM):** مراقبة تغييرات الملفات الحساسة في الوقت الفعلي
- **Active Response:** أتمتة الاستجابة التلقائية عند اكتشاف تهديد
- **إطار MITRE ATT&CK:** ربط التنبيهات بالتقنيات والتكتيكات المعيارية
- **Endpoint Visibility (EDR):** Syscollector وrootcheck وWazuh Vulnerability Detection وWazuh REST API
## 📁 المشاريع

| المشروع | الوصف |
|---|---|
| [تحليل حادثة Exploit Kit](./01-network-traffic-analysis) | تحليل ملف pcap حقيقي، تحديد جهاز مصاب بهجوم Drive-by Download |
| [إعداد SIEM (Wazuh)](./02-siem-setup-wazuh) | تنصيب وإعداد Wazuh SIEM كامل من الصفر |
| [قواعد كشف مخصصة](./03-custom-detection-rules) | كتابة قواعد Correlation لاكتشاف تهديدات متقدمة |
| [اكتشاف هجوم Brute-force](./04-brute-force-detection) | محاكاة واكتشاف هجوم SSH Brute-force |
| [الرؤية على الجهاز Syscollector](05-endpoint-visibility-syscollector) | اكتشاف بورت مفتوح جديد وتحديد البرنامج المسؤول عنه عبر Wazuh Syscollector وAPI |
| [كشف Rootkit](06-rootkit-detection-rootcheck) | اختبار وحدة rootcheck بملف وهمي آمن وتحليل حدود الكشف المعتمد على التواقيع |
| [كشف الثغرات](07-vulnerability-detection-wazuh) | تشخيص نتيجة فارغة في Wazuh Vulnerability Detection خطوة بخطوة |
## 📫 تواصل معي
aldosarilatifh@gmail.com
