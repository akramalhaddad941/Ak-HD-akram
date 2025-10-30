# واجهة تخصيص STT (STT Customization Interface)

توفر واجهة تخصيص STT للمستخدمين تحكمًا دقيقًا في عملية تحويل الكلام إلى نص (STT)، مما يسمح لهم بتحسين دقة التحويل بناءً على خصائص الفيديو والمحتوى.

## الميزات الرئيسية

1.  **اختيار محرك STT**:
    *   السماح للمستخدم بالاختيار بين محركات STT المتاحة (مثل Whisper، Google STT، أو النماذج المتخصصة).
    *   عرض معلومات حول كل محرك (مثل الدقة المتوقعة للغة معينة، التكلفة، والسرعة).

2.  **تحديد اللغة واللهجة**:
    *   تمكين المستخدم من تحديد اللغة واللهجة المنطوقة بدقة (مثل: العربية المصرية، الإنجليزية الأمريكية).
    *   هذا التحديد يوجه `وحدة تمييز المتحدثين واللغات` و`محركات التعرف على الكلام`.

3.  **التخصيص السياقي (Contextual Boost)**:
    *   توفير حقل إدخال للمصطلحات المتخصصة أو الأسماء الخاصة التي من المرجح أن تظهر في الفيديو.
    *   يتم تمرير هذه القائمة إلى `وحدة التخصيص السياقي` لتعزيز دقة التعرف على هذه الكلمات.
    *   **مثال**: إذا كان الفيديو عن "الذكاء الاصطناعي"، يمكن للمستخدم إدخال "التعلم العميق، الشبكات العصبية، GPT-4".

4.  **خيارات تمييز المتحدثين (Diarization Options)**:
    *   السماح للمستخدم بتحديد العدد التقريبي للمتحدثين في الفيديو (إذا كان معروفًا).
    *   خيار لتمكين أو تعطيل ميزة تمييز المتحدثين.

5.  **معالجة الصوت المسبقة**:
    *   عرض خيارات للتحكم في معالجة الصوت المسبقة (مثل: تمكين/تعطيل تقليل الضوضاء، أو تحديد مستوى التصفية).

## التنفيذ التقني (مثال)

```typescript
// src/components/STTCustomizationForm.tsx (React/Frontend Example)
import React, { useState } from 'react';
import { STTConfiguration } from '../interfaces/configuration';

const STTCustomizationForm: React.FC = () => {
  const [config, setConfig] = useState<STTConfiguration>({
    engine: 'Whisper',
    language: 'ar-EG',
    dialect: 'Egyptian',
    contextualTerms: '',
    speakerCount: 2,
    noiseReduction: true,
  });

  const handleChange = (e: React.ChangeEvent<HTMLInputElement | HTMLSelectElement | HTMLTextAreaElement>) => {
    const { name, value, type } = e.target;
    const isCheckbox = type === 'checkbox';
    setConfig(prev => ({
      ...prev,
      [name]: isCheckbox ? (e.target as HTMLInputElement).checked : value,
    }));
  };

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    console.log('STT Configuration Submitted:', config);
    // Call the backend service to save the configuration
  };

  return (
    <form onSubmit={handleSubmit}>
      <h3>تخصيص التحويل النصي (STT)</h3>

      <label htmlFor="engine">محرك STT المفضل:</label>
      <select id="engine" name="engine" value={config.engine} onChange={handleChange}>
        <option value="Whisper">Whisper (عام، دقة جيدة)</option>
        <option value="GoogleSTTExpert">Google STT Expert (متخصص، دقة عالية)</option>
        {/* Add other engines */}
      </select>

      <label htmlFor="language">اللغة:</label>
      <select id="language" name="language" value={config.language} onChange={handleChange}>
        <option value="ar-EG">العربية (مصر)</option>
        <option value="ar-SA">العربية (السعودية)</option>
        <option value="en-US">الإنجليزية (أمريكا)</option>
      </select>

      <label htmlFor="contextualTerms">مصطلحات تعزيز الدقة (كلمات مفتاحية متخصصة مفصولة بفواصل):</label>
      <textarea id="contextualTerms" name="contextualTerms" value={config.contextualTerms} onChange={handleChange} rows={3} placeholder="مثال: الذكاء الاصطناعي، التعلم العميق، الشبكات العصبية"></textarea>

      <label htmlFor="speakerCount">العدد التقريبي للمتحدثين:</label>
      <input type="number" id="speakerCount" name="speakerCount" value={config.speakerCount} onChange={handleChange} min="1" max="10" />

      <label>
        <input type="checkbox" name="noiseReduction" checked={config.noiseReduction} onChange={handleChange} />
        تمكين تقليل الضوضاء (معالجة الصوت المسبقة)
      </label>

      <button type="submit">حفظ إعدادات STT</button>
    </form>
  );
};

// Define the interface for configuration data
export interface STTConfiguration {
  engine: string;
  language: string;
  dialect: string;
  contextualTerms: string;
  speakerCount: number;
  noiseReduction: boolean;
}
```

تضمن هذه الواجهة أن المستخدمين يمكنهم توجيه نظام STT المتقدم بشكل فعال للحصول على أفضل تحويل نصي ممكن، والذي يُعد أساسًا للترجمة عالية الجودة.
