𝑴𝒏3𝒎 Jobs — نسخة ربط Firebase (ملفات أولية)

1) افتح index.html بمتصفح حديث متصل بالإنترنت.
2) تم إدخال إعدادات Firebase التي أرسلتها.
3) قبل استقبال طلبات حقيقية، فعّل Authentication > Email/Password وأنشئ حساب المدير.
4) انسخ UID لحساب المدير، ثم عدّل قواعد Firestore أدناه واستبدل ADMIN_UID_HERE بالـ UID الحقيقي.
5) انشر القواعد من تبويب Firestore > Rules.

قواعد Firestore المقترحة (استبدل ADMIN_UID_HERE قبل النشر):

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    function isAdmin() {
      return request.auth != null && request.auth.uid == "ADMIN_UID_HERE";
    }
    match /jobs/{jobId} {
      allow read: if true;
      allow create, update, delete: if isAdmin();
    }
    match /applications/{applicationId} {
      allow create: if request.resource.data.keys().hasAll(['name','phone','governorate','status'])
                    && request.resource.data.name is string
                    && request.resource.data.phone is string;
      allow read, update, delete: if isAdmin();
    }
  }
}

مهم:
- لا تنشر القواعد قبل استبدال ADMIN_UID_HERE بالـ UID الحقيقي.
- لا تستخدم وضع الاختبار المفتوح أو قواعد تسمح للجميع بالقراءة والكتابة.
- هذه النسخة واجهة ربط أولية؛ يجب اختبارها بعد إعداد Authentication والقواعد.
- لا تضع كلمات مرور أو مفاتيح خدمة خاصة داخل ملفات الموقع.
