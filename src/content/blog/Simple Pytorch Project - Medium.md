---
title: Pytorch 101 بالعربي
description: Pytorch 101 بالعربي
pubDate: Feb 28 2026
---

# مشروعك الأول في PyTorch: بناء Neural Network لتمييز الأرقام المكتوبة بخط اليد

أهلاً بك في رحلتك إلى عالم التعلم العميق مع PyTorch! هذا الدليل سيرشدك خطوة بخطوة لبناء Neural Network كامل من الصفر لتمييز الأرقام المكتوبة بخط اليد. سنغطي كل شيء من إعداد البيئة إلى نشر النموذج، مع شرح مفصل لكل مفهوم على طول الطريق.

---

## جدول المحتويات

1. [فهم هيكل ملفات PyTorch](#فهم-هيكل-ملفات-pytorch)
2. [المتطلبات وإعداد البيئة](#المتطلبات-وإعداد-البيئة)
3. [فهم مجموعة بيانات MNIST](#فهم-مجموعة-بيانات-mnist)
4. [بناء أول Neural Network لك](#بناء-أول-neural-network-لك)
5. [تحميل البيانات ومعالجتها بتفصيل](#تحميل-البيانات-ومعالجتها-بتفصيل)
6. [تدريب النموذج](#تدريب-النموذج)
7. [تقييم النموذج](#تقييم-النموذج)
8. [تصور نتائجك](#تصور-نتائجك)
9. [تكامل سير العمل الكامل](#تكامل-سير-العمل-الكامل)
10. [الخطوات التالية والتحسينات](#الخطوات-التالية-والتحسينات)

---

## فهم هيكل ملفات PyTorch

قبل البدء في كتابة الكود، من المهم فهم الهيكل الأساسي لمشاريع PyTorch والمكونات الرئيسية التي سنتعامل معها.

### الClasses الشائعة في مشاريع PyTorch

#### Model Architecture Classes

- **`nn.Module`** - الـ base class لكل الـ neural network models. كل نموذج مخصص تنشئه يجب أن يرث من هذا الـ class
- **`nn.Linear(in_features, out_features)`** - Fully connected layer يحول الـ input features إلى output features
- **`nn.Conv2d(in_channels, out_channels, kernel_size)`** - 2D convolutional layer لمعالجة الصور
- **`nn.MaxPool2d(kernel_size)`** - Pooling layer للـ downsampling
- **`nn.Dropout(p)`** - تقنية regularization تقوم بتعيين بعض الـ neurons إلى صفر بشكل عشوائي أثناء التدريب

#### Activation Function Classes

- **`nn.ReLU()`** - Rectified Linear Unit (الأكثر استخداماً)
- **`nn.Sigmoid()`** - دالة على شكل S تخرج قيم بين 0 و 1
- **`nn.Tanh()`** - دالة الـ hyperbolic tangent تخرج قيم بين -1 و 1
- **`nn.Softmax(dim)`** - تحول الـ logits إلى probabilities (مفيدة للـ multi-class classification)

#### Loss Function Classes

- **`nn.CrossEntropyLoss()`** - تجمع بين LogSoftmax و Negative Log Likelihood Loss (مثالية للـ multi-class classification)
- **`nn.MSELoss()`** - Mean Squared Error (تستخدم في مهام الـ regression)
- **`nn.BCELoss()`** - Binary Cross Entropy Loss (للـ binary classification)

#### Optimizer Classes

- **`optim.Adam(model.parameters(), lr=learning_rate)`** - Adaptive Moment Estimation (الخيار الأكثر شعبية)
- **`optim.SGD(model.parameters(), lr=learning_rate)`** - Stochastic Gradient Descent
- **`optim.RMSprop(model.parameters(), lr=learning_rate)`** - Root Mean Square Propagation

### الدوال والـ Methods الأساسية

#### Model Structure Methods

- **`__init__()`** - Constructor method حيث تعرف كل الـ layers في نموذجك
- **`forward(x)`** - Method يحدد كيف تتدفق البيانات خلال شبكتك (يتم استدعاؤه تلقائياً)
- **`model.to(device)`** - ينقل النموذج إلى CPU أو GPU
- **`model.train()`** - يضبط النموذج في وضع التدريب (يفعل الـ dropout، تحديثات الـ batch norm)
- **`model.eval()`** - يضبط النموذج في وضع التقييم (يعطل الـ dropout، يثبت الـ batch norm)

#### Training Loop Functions

- **`optimizer.zero_grad()`** - يمسح الـ gradients السابقة (يجب استدعاؤه قبل كل backward pass)
- **`loss.backward()`** - يحسب الـ gradients باستخدام الـ backpropagation
- **`optimizer.step()`** - يحدث معلمات النموذج باستخدام الـ gradients المحسوبة
- **`torch.max(tensor, dim)`** - يرجع القيم القصوى وفهارسها
- **`torch.no_grad()`** - Context manager يعطل حساب الـ gradients

### مكونات الـ Utility الرئيسية

- **`DataLoader(dataset, batch_size, shuffle)`** - يتعامل مع الـ batching، الـ shuffling، والتحميل الموازي
- **`transforms.Compose([...])`** - سلسلة من تحويلات البيانات
- **`transforms.ToTensor()`** - يحول صور PIL أو مصفوفات numpy إلى tensors
- **`transforms.Normalize(mean, std)`** - يقوم بـ normalize قيم الـ tensor
- **`torch.save(model.state_dict(), path)`** - يحفظ معلمات النموذج
- **`torch.load(path)`** - يحمل معلمات النموذج المحفوظة

---

## المتطلبات وإعداد البيئة

### متطلبات النظام

- Python 3.7 أو أعلى
- 4GB RAM على الأقل
- 2GB مساحة خالية على القرص لمجموعة البيانات والنماذج

### الخطوة 1: إنشاء Virtual Environment

**على نظام Windows:**

```bash
python -m venv pytorch_env
pytorch_env\Scripts\activate
```

**على أنظمة macOS/Linux:**

```bash
python3 -m venv pytorch_env
source pytorch_env/bin/activate
```

### الخطوة 2: تثبيت الحزم المطلوبة

قم بإنشاء ملف requirements.txt بالمحتوى التالي:

```txt
torch>=1.9.0
torchvision>=0.10.0
matplotlib>=3.3.0
numpy>=1.21.0
```

ثبت الحزم:

```bash
pip install -r requirements.txt
```

### الخطوة 3: التحقق من التثبيت

```python
import torch
import torchvision
import matplotlib.pyplot as plt
import numpy as np

print(f"PyTorch version: {torch.__version__}")
print(f"Torchvision version: {torchvision.__version__}")
print("All packages installed successfully!")
```

---

## فهم مجموعة بيانات MNIST

### ما هي MNIST؟

MNIST (Modified National Institute of Standards and Technology) هي "Hello World" للتعلم العميق. تتكون من:

- 60,000 صورة تدريب
- 10,000 صورة اختبار
- صور رمادية 28x28 للأرقام المكتوبة بخط اليد (0-9)
- كل صورة مصنفة بالرقم المقابل لها

### لماذا MNIST مثالية للمبتدئين؟

1. **صغيرة وقابلة للإدارة**: الصور فقط 784 بكسل لكل صورة
2. **مفهومة جيداً**: توثيق شامل ودعم مجتمعي
3. **تدريب سريع**: النماذج تتدرب في دقائق، ليس ساعات
4. **مقاييس واضحة**: سهل فهم الـ accuracy والـ loss

### هيكل البيانات

كل صورة MNIST لها هذه الخصائص:

- **الحجم**: 28×28 بكسل = 784 بكسل إجمالي
- **اللون**: رمادي (قناة واحدة)
- **القيم**: قيم البكسل من 0 (أسود) إلى 255 (أبيض)
- **التصنيفات**: أعداد صحيحة من 0 إلى 9

### أهمية الـ Normalization

الـ Normalization تحول قيم البكسل من [0, 255] إلى نطاق قياسي، عادة [0, 1] أو [-1, 1]. هذا مهم لأن:

1. **تقارب أسرع**: النماذج تتعلم بشكل أسرع مع البيانات المُعالجة
2. **تدريب مستقر**: يمنع exploding/vanishing gradients
3. **أداء أفضل**: يحسن الـ accuracy الإجمالي للنموذج

---

## بناء أول Neural Network لك

لنحلل بنية الـ neural network من ملف main.py الخاص بنا:

### هيكل الـ Model Class

```python
class ANN(nn.Module):
    def __init__(self, input_size=784, hidden_size1=128, hidden_size2=64, num_classes=10):
        super(ANN, self).__init__()
        # Layer definitions تذهب هنا
        
    def forward(self, x):
        # Data flow definition يذهب هنا
        return x
```

### تحليل البية

الـ neural network لدينا له البنية التالية:

**Input Layer**: 784 neurons (28×28 بكسل)
**Hidden Layer 1**: 128 neurons مع ReLU activation
**Hidden Layer 2**: 64 neurons مع ReLU activation
**Output Layer**: 10 neurons (واحد لكل فئة رقم)

### فهم كل مكون

#### Linear Layers (Fully Connected)

```python
self.fc1 = nn.Linear(input_size, hidden_size1)  # 784 → 128
self.fc2 = nn.Linear(hidden_size1, hidden_size2)  # 128 → 64
self.fc3 = nn.Linear(hidden_size2, num_classes)  # 64 → 10
```

- **الغرض**: يحول بيانات الإدخال عبر مصفوفات أوزان متعلمة
- **كيف يعمل**: `output = input × weights + bias`
- **المعلمات**: كل اتصال له وزن خاص به يتم تعلمه أثناء التدريب

#### Dropout Regularization

```python
self.dropout = nn.Dropout(0.2)
```

- **الغرض**: يمنع الـ overfitting عن طريق إسقاط 20% من الـ neurons بشكل عشوائي
- **متى يطبق**: فقط أثناء التدريب (يتم تعطيله تلقائياً أثناء التقييم)
- **لماذا يعمل**: يجبر الشبكة على تعلم تمثيلات زائدة

#### ReLU Activation Function

```python
x = F.relu(self.fc1(x))
```

- **الصيغة**: `ReLU(x) = max(0, x)`
- **الغرض**: يقدم الـ non-linearity، مما يسمح للشبكة بتعلم أنماط معقدة
- **الفوائد**: فعال حسابياً ويتجنب مشاكل vanishing gradient

#### Forward Pass Mechanics

```python
def forward(self, x):
    x = x.view(-1, 784)  # تحويل الصورة 28x28 إلى متجه 784-dimensional
    x = F.relu(self.fc1(x))  # تطبيق أول linear layer + ReLU
    x = self.dropout(x)  # تطبيق الـ dropout
    x = F.relu(self.fc2(x))  # تطبيق ثاني linear layer + ReLU
    x = self.dropout(x)  # تطبيق الـ dropout
    x = self.fc3(x)  # تطبيق الـ output layer (بدون activation هنا)
    return x
```

**نقاط رئيسية:**
- خطوة التسوية (flattening) تحول الصور ثنائية الأبعاد إلى متجهات أحادية البعد
- الـ dropout يطبق فقط بين الـ hidden layers
- الـ output layer ليس لديه activation لأن الـ CrossEntropyLoss يتعامل معه داخلياً

---

## تحميل البيانات ومعالجتها بتفصيل

### شرح دالة load_data()

```python
def load_data():
    # حساب mean و std لبيانات التدريب
    print("Calculating dataset mean and std...")
    raw_train_dataset = torchvision.datasets.MNIST(
        root='./data', train=True, download=True
    )
    
    # حساب mean و std
    data = raw_train_dataset.data.float() / 255.0
    mean = data.mean().item()
    std = data.std().item()
    
    print(f"Calculated Mean: {mean:.4f}, Std: {std:.4f}")
```

### عملية الـ Normalization خطوة بخطوة

1. **تحميل البيانات الخام**: أولاً، نحمل مجموعة البيانات بدون أي تحويلات
2. **التحويل إلى Float**: نحول قيم uint8 [0, 255] إلى float [0.0, 255.0]
3. **الـ Normalize إلى [0, 1]**: نقسم على 255.0 للحصول على قيم في نطاق [0, 1]
4. **حساب الإحصائيات**: نحسب الـ mean والـ standard deviation عبر مجموعة البيانات بأكملها
5. **تطبيق التحويل**: نستخدم هذه الإحصائيات للـ normalization السليم

### لماذا نحسب إحصائيات خاصة بمجموعة البيانات؟

```python
# القيم النموذجية لـ MNIST
Mean: 0.1307, Std: 0.3081
```

استخدام إحصائيات خاصة بمجموعة البيانات بدلاً من قيم ثابتة يضمن:
- **الاتساق**: نفس الـ normalization بغض النظر عن اختلافات مجموعة البيانات
- **الدقة**: توحيد قياسي سليم بناءً على توزيع البيانات الفعلي
- **المرونة**: الكود يعمل مع مجموعات بيانات أخرى بدون تعديل

### Transform Pipeline

```python
transform = transforms.Compose([
    transforms.ToTensor(),  # يحول إلى tensor ويقيس إلى [0, 1]
    transforms.Normalize((mean,), (std,))  # يوحد إلى mean=0, std=1
])
```

**transforms.Compose()**: ينشئ خط أنابيب من التحويلات المتسلسلة
**ToTensor()**: يتعامل مع تحويل الـ tensor والـ normalization الأساسي
**Normalize()**: يطبق z-score normalization باستخدام الإحصائيات المحسوبة

### إعدادات DataLoader

```python
train_loader = DataLoader(train_dataset, batch_size=64, shuffle=True)
test_loader = DataLoader(test_dataset, batch_size=1000, shuffle=False)
```

**اعتبارات حجم الدفعة:**
- **التدريب (64)**: صغير بما يكفي لتحديثات الأوزان المتكررة، كبير بما يكفي لـ gradients مستقرة
- **الاختبار (1000)**: دفعة أكبر للتقييم الأسرع، لا حاجة للـ shuffling

**استراتيجية الـ Shuffling:**
- **التدريب**: `shuffle=True` يمنع النموذج من تعلم أنماط الترتيب
- **الاختبار**: `shuffle=False` يضمن تقييم متسق

---

## تدريب النموذج

### تحليل شامل لدالة التدريب

```python
def train_model(model, train_loader, criterion, optimizer, device, epochs=10):
    model.train()  # ضبط النموذج في وضع التدريب
    train_losses = []  # تتبع الـ loss للرسم
    train_accuracies = []  # تتبع الـ accuracy للرسم
    
    for epoch in range(epochs):
        running_loss = 0.0  # تجميع الـ loss لهذه الـ epoch
        correct = 0  # عدد التنبؤات الصحيحة
        total = 0  # إجمالي العينات
        
        for batch_idx, (data, target) in enumerate(train_loader):
            data, target = data.to(device), target.to(device)
            
            # خطوات التدريب الأساسية
            optimizer.zero_grad()  # 1. مسح الـ gradients
            output = model(data)  # 2. Forward pass
            loss = criterion(output, target)  # 3. حساب الـ loss
            loss.backward()  # 4. Backpropagation
            optimizer.step()  # 5. تحديث الأوزان
            
            # تتبع المقاييس
            running_loss += loss.item()
            _, predicted = torch.max(output.data, 1)
            total += target.size(0)
            correct += (predicted == target).sum().item()
            
            # إبلاغ التقدم
            if batch_idx % 100 == 0:
                print(f'Epoch {epoch+1}/{epochs}, Batch {batch_idx}, Loss: {loss.item():.4f}')
        
        # حساب مقاييس الـ epoch
        epoch_loss = running_loss / len(train_loader)
        epoch_acc = 100 * correct / total
        train_losses.append(epoch_loss)
        train_accuracies.append(epoch_acc)
        
        print(f'Epoch {epoch+1}/{epochs} - Loss: {epoch_loss:.4f}, Accuracy: {epoch_acc:.2f}%')
    
    return train_losses, train_accuracies
```

### فهم Loss Functions

**CrossEntropyLoss**: مثالية للـ multi-class classification لأنها:
- تجمع بين LogSoftmax و Negative Log Likelihood
- تتعامل مع الاستقرار العددي تلقائياً
- تعمل مباشرة مع مخرجات النموذج الخام (logits)

**Adam Optimizer**: خيار شائع لأنه:
- يكيف معدلات التعلم لكل معلمة
- يجمع بين فوائد momentum و RMSprop
- يعمل بشكل جيد مع الـ hyperparameters الافتراضية

### شرح Training Loop

#### 5 خطوات تدريب أساسية:

1. **`optimizer.zero_grad()`**: يمسح الـ gradients السابقة
   - PyTorch يتراكم الـ gradients بشكل افتراضي
   - يجب مسحها قبل كل backward pass

2. **`output = model(data)`**: Forward pass
   - البيانات تتدفق عبر طبقات الشبكة
   - تنتج تنبؤات (logits)

3. **`loss = criterion(output, target)`**: حساب الـ loss
   - يقيس مدى خطأ التنبؤات
   - يوفر إشارة للتعلم

4. **`loss.backward()`**: Backpropagation
   - يحسب الـ gradients لكل المعلمات
   - قاعدة السلسلة مطبقة تلقائياً

5. **`optimizer.step()`**: تحديث الأوزان
   - يطبق الـ gradients المحسوبة على المعلمات
   - ينقل النموذج في اتجاه الـ loss الأقل

### تتبع المقاييس

**حساب الـ Loss:**
```python
running_loss += loss.item()
epoch_loss = running_loss / len(train_loader)
```

**حساب الـ Accuracy:**
```python
_, predicted = torch.max(output.data, 1)  # الحصول على الفئة المتنبأة
correct += (predicted == target).sum().item()  # عد التنبؤات الصحيحة
accuracy = 100 * correct / total  # التحويل إلى نسبة مئوية
```

---

## تقييم النموذج

### شرح دالة التقييم

```python
def evaluate_model(model, test_loader, device):
    model.eval()  # ضبط النموذج في وضع التقييم
    correct = 0
    total = 0
    test_loss = 0.0
    criterion = nn.CrossEntropyLoss()
    
    with torch.no_grad():  # تعطيل حساب الـ gradients
        for data, target in test_loader:
            data, target = data.to(device), target.to(device)
            output = model(data)
            test_loss += criterion(output, target).item()
            _, predicted = torch.max(output.data, 1)
            total += target.size(0)
            correct += (predicted == target).sum().item()
    
    accuracy = 100 * correct / total
    avg_loss = test_loss / len(test_loader)
    
    return accuracy, avg_loss
```

### مفاهيم التقييم الرئيسية

#### model.eval() vs model.train()

- **eval()**: يعطل الـ dropout، يستخدم الإحصائيات الجارية للـ batch normalization
- **train()**: يفعل الـ dropout، يحديث إحصائيات الـ batch normalization
- **لماذا يهم**: يضمن نتائج تقييم متسقة وقابلة للتكرار

#### torch.no_grad() Context

- **الغرض**: يعطل حساب الـ gradients لتوفير الذاكرة والحساب
- **الفوائد**: تقييم أسرع، استخدام ذاكرة أقل
- **السلامة**: يمنع تحديثات الـ gradients العرضية

### مقاييس التقييم

**Accuracy**: نسبة التنبؤات الصحيحة
```python
accuracy = 100 * correct / total
```

**Average Loss**: متوسط الـ loss عبر كل عينات الاختبار
```python
avg_loss = test_loss / len(test_loader)
```

### تفسير النتائج

لتمييز الأرقام المكتوبة بخط اليد في MNIST:
- **Accuracy جيد**: >95%
- **Accuracy ممتاز**: >98%
- **State-of-the-art**: >99%

إذا كان الـ accuracy منخفضاً (<90%)، فكر في:
- زيادة تعقيد النموذج
- التدريب لعدد أكبر من الـ epochs
- تعديل معدل التعلم
- فحص معالجة البيانات

---

## تصور نتائجك

### شرح دالة plot_results()

```python
def plot_results(train_losses, train_accuracies, test_accuracy, test_loss):
    os.makedirs('results', exist_ok=True)  # إنشاء مجلد النتائج
    
    fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 5))  # إنشاء subplots
    
    # رسم Training Loss
    ax1.plot(train_losses, label='Training Loss')
    ax1.set_title('Training Loss')
    ax1.set_xlabel('Epoch')
    ax1.set_ylabel('Loss')
    ax1.legend()
    
    # رسم Training Accuracy
    ax2.plot(train_accuracies, label='Training Accuracy')
    ax2.axhline(y=test_accuracy, color='r', linestyle='--', 
                label=f'Test Accuracy: {test_accuracy:.2f}%')
    ax2.set_title('Accuracy')
    ax2.set_xlabel('Epoch')
    ax2.set_ylabel('Accuracy (%)')
    ax2.legend()
    
    plt.tight_layout()  # منع التداخل
    plt.savefig('results/training_results.png')  # حفظ الرسم
```

### فهم منحنيات التدريب

#### خصائص منحنى الـ Loss:
- **اتجاه تنازلي**: النموذج يتعلم
- **منحنى سلس**: تدريب مستقر
- **Plateau**: النموذج قد يكون متقارباً
- **زيادة**: مشاكل محتملة في معدل التعلم

#### خصائص منحنى الـ Accuracy:
- **اتجاه تصاعدي**: النموذج يتحسن
- **تباين عالٍ**: قد يحتاج regularization أو دفعات أكبر
- **فجوة مع test accuracy**: overfitting محتمل

### ماذا تبحث عن في الرسوم البيانية

**تدريب صحي:**
- loss متناقص سلس
- accuracy متصاعد سلس
- فجوة صغيرة بين أداء التدريب والاختبار

**علامات المشاكل:**
- منحنيات متذبذبة (معدل تعلم مرتفع جداً)
- فجوة كبيرة بين تدريب واختبار (overfitting)
- منحنيات مسطحة (معدل تعلم منخفض جداً أو سعة النموذج غير كافية)

---

## تكامل سير العمل الكامل

### تنسيق دالة main()

```python
def main():
    device = torch.device("cpu")  # اختيار الجهاز
    print(f'Using device: {device}')
    
    train_loader, test_loader = load_data()  # تحميل وإعداد البيانات
    
    # تهيئة مكونات النموذج
    model = ANN().to(device)
    criterion = nn.CrossEntropyLoss()
    optimizer = optim.Adam(model.parameters(), lr=0.001)
    
    # مرحلة التدريب
    print("Starting training...")
    train_losses, train_accuracies = train_model(
        model, train_loader, criterion, optimizer, device, epochs=10
    )
    
    # مرحلة التقييم
    print("Evaluating model...")
    test_accuracy, test_loss = evaluate_model(model, test_loader, device)
    print(f'Test Accuracy: {test_accuracy:.2f}%')
    print(f'Test Loss: {test_loss:.4f}')
    
    # التصور والحفظ
    plot_results(train_losses, train_accuracies, test_accuracy, test_loss)
    torch.save(model.state_dict(), 'models/ann_model.pth')
    print("Model saved to models/ann_model.pth")
```

### أفضل الممارسات لتنظيم المشروع

**هيكل الدليل الموصى به:**
```
mnist_project/
├── main.py
├── requirements.txt
├── data/           # البيانات المحملة
├── models/         # ملفات النماذج المحفوظة
├── results/        # الرسوم البيانية والمخرجات
└── utils/          # دوال المساعدة
```

### حفظ وتحميل النموذج

**حفظ النموذج:**
```python
torch.save(model.state_dict(), 'models/ann_model.pth')
```

**تحميل النموذج:**
```python
model = ANN()
model.load_state_dict(torch.load('models/ann_model.pth'))
model.eval()  # ضبط في وضع التقييم
```

**ملاحظة هامة**: 
- احفظ فقط `state_dict()` (المعلمات) وليس النموذج بأكمله
- هذا يضمن التوافق عبر إصدارات PyTorch المختلفة
- بنية النموذج يجب أن تتطابق عند التحميل

---

## الخطوات التالية والتحسينات

### تجربة بنى مختلفة

جرب تعديل الـ ANN class:

**شبكة أوسع:**
```python
hidden_size1 = 256  # زيادة عدد الـ neurons
hidden_size2 = 128
```

**شبكة أعمق:**
```python
def __init__(self):
    super().__init__()
    self.fc1 = nn.Linear(784, 256)
    self.fc2 = nn.Linear(256, 128)
    self.fc3 = nn.Linear(128, 64)
    self.fc4 = nn.Linear(64, 32)  # إضافة طبقة إضافية
    self.fc5 = nn.Linear(32, 10)
```

**Convolutional Network:**
```python
class CNN(nn.Module):
    def __init__(self):
        super().__init__()
        self.conv1 = nn.Conv2d(1, 32, 3)  # أفضل للصور
        self.conv2 = nn.Conv2d(32, 64, 3)
        self.pool = nn.MaxPool2d(2, 2)
        self.fc1 = nn.Linear(64 * 5 * 5, 128)
        self.fc2 = nn.Linear(128, 10)
```

### ضبط الـ Hyperparameters

**تجارب معدل التعلم:**
```python
# جرب معدلات تعلم مختلفة
optimizer = optim.Adam(model.parameters(), lr=0.01)  # أعلى
optimizer = optim.Adam(model.parameters(), lr=0.0001)  # أقل
```

**تجارب حجم الدفعة:**
```python
train_loader = DataLoader(train_dataset, batch_size=32)  # دفعات أصغر
train_loader = DataLoader(train_dataset, batch_size=128)  # دفعات أكبر
```

**تجارب الـ Epoch:**
```python
train_model(model, train_loader, criterion, optimizer, device, epochs=20)  # تدريب أطول
```

### الانتقال إلى تسريع GPU

**فحص توفر GPU:**
```python
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
print(f'Using device: {device}')
```

**تسريع متوقع:**
- وقت التدريب: 3-10x أسرع
- سعة الذاكرة: التعامل مع نماذج ودفعات أكبر

### تقنيات متقدمة للاستكشاف

1. **Data Augmentation**: تطبيق تحويلات عشوائية لزيادة تنوع مجموعة البيانات
2. **Batch Normalization**: توحيد مدخلات الطبقة لتحسين استقرار التدريب
3. **Learning Rate Scheduling**: تعديل معدل التعلم ديناميكياً أثناء التدريب
4. **Early Stopping**: إيقاف التدريب عندما يتوقف أداء التحقق عن التحسن
5. **Ensemble Methods**: دمج نماذج متعددة لأداء أفضل

### نصائح استكشاف الأخطاء الشائعة

**مشاكل التدريب:**
- **الـ loss لا يتناقص**: تحقق من معدل التعلم، معالجة البيانات
- **الـ accuracy عالق عند ~10%**: النموذج يتنبأ دائماً بنفس الفئة (تحقق من البيانات)
- **أخطاء الذاكرة**: قلل حجم الدفعة أو حجم النموذج

**مشاكل البيانات:**
- **فشل التحميل**: تحقق من اتصال الإنترنت، جرب التحميل اليدوي
- **أخطاء الـ Normalization**: تأكد من حساب الـ mean/std بشكل صحيح
- **عدم تطابق الأشكال**: تحقق من تطابق أبعاد الإدخال مع توقعات الطبقة

### مصادر التعلم الإضافية

**التوثيق الرسمي:**
- [PyTorch Documentation](https://pytorch.org/docs/)
- [PyTorch Tutorials](https://pytorch.org/tutorials/)

**دورات موصى بها:**
- Fast.ai's Practical Deep Learning
- Coursera's Deep Learning Specialization
- MIT's Introduction to Deep Learning

**مجموعات بيانات للممارسة:**
- CIFAR-10 (صور ملونة)
- Fashion-MNIST (تصنيف الملابس)
- IMDb (تصنيف النصوص)

---

## الخاتمة

تهانينا! لقد بنيت أول مشروع PyTorch كامل من الصفر. لقد تعلمت:

✅ **أساسيات PyTorch**: بنية الـ Module، الـ layers، وtraining loops  
✅ **معالجة البيانات**: التحميل، المعالجة المسبقة، وتجميع مجموعات البيانات  
✅ **بنية النموذج**: بناء وتخصيص الـ neural networks  
✅ **عملية التدريب**: loss functions، optimizers، والمراقبة  
✅ **التقييم والتصور**: تقييم أداء النموذج  
✅ **أفضل الممارسات**: تنظيم المشروع وهيكل الكود  

مشروع MNIST هذا يوفر أساساً متيناً للتعامل مع تحديات التعلم العميق الأكثر تعقيداً. المفاهيم التي تعلمتها تنطبق على تصنيف الصور، معالجة اللغات الطبيعية، التعلم المعزز، وما وراء ذلك.

**استمر في التجربة، ابق فضولياً، وتعلم عميق سعيداً!** 🚀

---

*هذا الدليل جزء من سلسلة دروس PyTorch. إذا وجدته مفيداً، فكر في مشاركته مع الآخرين الذين يبدأون رحلتهم في التعلم العميق!*

**النتائج النهائية المتوقعة:**
- Test Accuracy: ~97-98%
- Training Time: 5-10 دقائق (CPU)، 1-2 دقائق (GPU)
- Model Size: < 1MB
- Memory Usage: < 500MB
