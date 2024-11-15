
# بايثون 3.13: كل ما هو جديد ومثير!

[![شاهد الفيديو](https://img.youtube.com/vi/DE77J_HIixo/0.jpg)](https://youtu.be/DE77J_HIixo)

مع كل إصدار جديد من بايثون، تتجدد الإثارة! في هذا الفيديو، نستكشف كل ما هو جديد في Python 3.13، من التحسينات الطفيفة إلى التغييرات الجذرية.


## القسم الأول: مترجم تفاعلي مُحسّن (REPL)

- **واجهة ملونة:**  موجه الأوامر repl أصبح ملونًا لسهولة قراءة الأكواد.
- **تحسين التعامل مع الأكواد متعددة الأسطر:**  استدعاء وتعديل وتنفيذ كتل التعليمات البرمجية بسهولة.
- **حل مشكلة لصق الأكواد:**  لا مزيد من مشاكل الفراغات والأسطر الفارغة عند نسخ ولصق الأكواد.

**مثال:**

```python
def my_function():
    print("Hello, world!")

my_function()
```

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

- Visual Studio 2019 (أو أحدث) مع حزمة تطوير ++C و Windows SDK 10.0.18362.0 (أو أحدث).
- LLVM/Clang 19 (إضافة مسار مجلد bin إلى متغيرات البيئة).
- Git.

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
[![شاهد الفيديو](https://img.youtube.com/vi/DE77J_HIixo/0.jpg)](https://youtu.be/DE77J_HIixo)

</center>
