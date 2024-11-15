
# بايثون 3.13: كل ما هو جديد ومثير!
<center>
  <a href="https://youtu.be/DE77J_HIixo">
    <img src="https://img.youtube.com/vi/DE77J_HIixo/0.jpg" alt="شاهد الفيديو">
  </a>
</center>

## القسم الأول: مترجم تفاعلي مُحسّن (REPL)

- **واجهة ملونة:**  موجه الأوامر repl أصبح ملونًا لسهولة قراءة الأكواد.
- **تحسين التعامل مع الأكواد متعددة الأسطر:**  استدعاء وتعديل وتنفيذ كتل التعليمات البرمجية بسهولة.
- **حل مشكلة لصق الأكواد:**  لا مزيد من مشاكل الفراغات والأسطر الفارغة عند نسخ ولصق الأكواد.

*

## القسم الثاني: رسائل خطأ أوضح وأكثر فائدة

- **رسائل خطأ ملونة:**  تُعرض رسائل الخطأ بألوان لتسليط الضوء على مكامن الخلل.
- **إيقاف الألوان:** `PYTHON_COLORS = 0`
- **اقتراحات حلول:**  بايثون 3.13 تقترح تصحيحات للأخطاء الشائعة.


## القسم الثالث: التخلص من قفل المُفسّر العام (GIL)

- **نسخة تجريبية بدون GIL:**  تحسين الأداء في الأنظمة متعددة النواة.

**تثبيت النسخة بدون GIL:**

1. **تحميل الشيفرة المصدرية وإعادة بنائها:** مع خيار no GIL.
2. **تثبيت من خلال "customize installation":**  تفعيل خيار "download the free thredded binaries".

**التحقق من التثبيت:**

```bash
py -0p
```

**تشغيل النسخة بدون GIL:**

```bash
py 3.13t
```

**مثال كود المعالجة المتوازية:**

```python
import time
from concurrent.futures import ThreadPoolExecutor

def task():
    result = 0
    for i in range(10000000):
        result += i
    return result

start_time = time.time()

with ThreadPoolExecutor(max_workers=6) as executor:
    futures = [executor.submit(task) for _ in range(6)]
    results = [f.result() for f in futures]

end_time = time.time()

print(f"Time taken: {end_time - start_time:.2f} seconds")
```

## القسم الرابع: مُترجم JIT تجريبي

- **تحسين الأداء:**  ترجمة أجزاء من الكود إلى لغة الآلة أثناء التشغيل.

**متطلبات بناء JIT على ويندوز:**

- ** ـ Visual Studio 2019 (أو أحدث) مع حزمة تطوير ++C و Windows SDK:**  [تحميل Visual Studio](https://visualstudio.microsoft.com/vs/) (تأكد من تحديد حزمة تطوير  C++ و Windows 10 SDK أثناء التثبيت).  أثناء التثبيت، ابحث عن "Desktop development with C++" و "Windows 10 SDK".

- **ـLLVM/Clang 19:** [تحميل LLVM](https://releases.llvm.org/download.html) (اختر الإصدار المناسب لنظام التشغيل الخاص بك - Windows).  تأكد من اختيار الإصدار 19 specifically.  بعد التحميل، قم بفك الضغط، ثم أضف مسار مجلد `bin` (على سبيل المثال: `C:\LLVM\bin`) إلى متغيرات بيئة نظامك.

- **ـGit:** [تحميل Git](https://git-scm.com/downloads)


**ملاحظات مهمة:**

* **Visual Studio:**  حجم التحميل كبير (عدة غيغابايت) وتثبيته قد يستغرق وقتًا.  تأكد من وجود مساحة كافية على القرص الصلب.
* **LLVM/Clang:** تأكد من تحميل الإصدار 19 تحديدًا، لأن الإصدارات الأخرى قد لا تعمل بشكل صحيح مع Python 3.13.  واحرص على إضافة مسار المجلد `bin` بشكل صحيح إلى متغيرات البيئة، حتى يتمكن النظام من العثور على  `clang`.
* **Git:**  ضروري لاستنساخ مستودع Python من GitHub.

بالتوفيق في بناء Python 3.13 مع JIT!

**خطوات بناء JIT:**

1. **تحميل الكود المصدري:**
   ```bash
   git clone https://github.com/python/cpython.git
   cd cpython
   ```

2. **بدء عملية البناء:**
   ```bash
   PCbuild\build.bat --experimental-jit
   ```

**إضافة نسخة Python مع JIT إلى VS Code:**

- إضافة مسار مجلد `amd64`  عبر `add interpreter path`.

**مثال كود اختبار JIT:**

```python
import time

def compute():
    result = 0
    for i in range(10000000):
        result += i
    return result


times = []
for _ in range(5):
    start = time.time()
    compute()
    end = time.time()
    times.append(end - start)
    
print(f"Fastest: {min(times):.3f} seconds")
print(f"Average: {sum(times)/len(times):.3f} seconds")


```


## القسم الخامس: تحسينات في الكتابة الساكنة

- **TypeIs:**  تضييق أنواع البيانات وتعزيز دقة فحص الأنواع.

**مثال:**

```python
from typing import Union, TypeIs

def is_valid_number(value: Union[str, int]) -> TypeIs[int]:
    if isinstance(value, str):
        return value.isdigit()
    return isinstance(value, int)

def calculate_inverse(value: Union[str, int]) -> float:
    if is_valid_number(value):
        if isinstance(value, str):
            num = int(value) # تحويل السلسلة إلى رقم إذا لزم
        else:
            num = value #  الرقم جاهز للاستخدام
        return 1.0 / num
    raise TypeError("قيمة غير صالحة")


print(calculate_inverse("10"))  # يُخرج 0.1
print(calculate_inverse(20))  # يُخرج 0.05
print(calculate_inverse("hello"))  # يُرفع TypeError
```


## القسم السادس: ميزات أخرى رائعة

- **واجهة سطر أوامر لوحدة random:**
    ```bash
    python -m random "أحمر" "أخضر" "أزرق"
    python -m random 10
    python -m random 5.5
    ```

- **`copy.replace()`:**  تعديل الكائنات غير القابلة للتغيير.

- **تحسينات في `pathlib.glob()`:**  دعم `**` و `glob.translate()`.

- **إزالة المسافات الزائدة من سلاسل التوثيق.**

- **دعم iOS و Android.**

- **تمديد دورة حياة الإصدارات.**

- **تحسينات في `argparse` و `locals()`**.

- **إزالة الوحدات المهملة.**



## الخاتمة

بايثون 3.13 مليئة بالتحسينات! شاركنا برأيك في التعليقات.

<center>
  <a href="https://youtu.be/DE77J_HIixo">
    <img src="https://img.youtube.com/vi/DE77J_HIixo/0.jpg" alt="شاهد الفيديو">
  </a>
</center>
