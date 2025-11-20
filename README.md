<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>محاكي — إعداد المعلم : فهد الخالدي</title>
<style>
:root{
  --primary:#1e64d0;
  --muted:#6b7280;
  --card:#ffffff;
  --success:#16a34a;
  --danger:#ef4444;
  --bg:#f6f8ff;
}
*{box-sizing:border-box}
html,body{height:100%;margin:0;padding:0;font-family:"Almarai","Noto Kufi Arabic",Tahoma,Arial,sans-serif;background:var(--bg);color:#0b1220}
.container{max-width:1100px;margin:20px auto;background:var(--card);padding:18px;border-radius:12px;box-shadow:0 12px 40px rgba(12,22,48,0.06)}
.header{display:flex;flex-direction:column;align-items:center;gap:6px}
.title{font-size:1.25rem;font-weight:800;color:var(--primary);margin:6px 0}
.credit{color:var(--muted);font-weight:700}
.main{display:grid;grid-template-columns:1fr 340px;gap:18px;margin-top:18px}
@media(max-width:980px){.main{grid-template-columns:1fr}}
.quiz{display:flex;flex-direction:column;gap:14px;padding-right:6px}
.card{background:linear-gradient(180deg,#fff,#fbfdff);border-radius:10px;padding:12px;box-shadow:0 8px 30px rgba(2,6,23,0.04);transition:transform .12s;overflow:visible}
.card:hover{transform:translateY(-4px)}
.q-head{display:flex;justify-content:space-between;align-items:center;gap:12px}
.q-num{background:linear-gradient(135deg,#06b6d4,var(--primary));color:#fff;padding:6px 10px;border-radius:8px;font-weight:800}
.sentence{margin-top:10px;font-size:1.02rem;line-height:1.6;color:#0b1220;white-space:pre-wrap}
.options{display:flex;flex-wrap:wrap;gap:10px;margin-top:12px}
.opt{flex:1 1 48%;min-width:140px;padding:12px;border-radius:10px;border:1px solid rgba(15,23,42,0.06);background:linear-gradient(180deg,#fbfdff,#f6fbff);font-weight:700;cursor:pointer;text-align:right;transition:all .12s;user-select:none}
.opt:hover{transform:translateY(-4px);box-shadow:0 10px 30px rgba(2,6,23,0.04)}
.opt.disabled{opacity:.9;pointer-events:none}
.opt.correct{background:linear-gradient(90deg,#bbf7d0,var(--success));color:#04200a;box-shadow:0 10px 30px rgba(16,185,129,0.08);border-color:rgba(16,185,129,0.25)}
.opt.wrong{background:linear-gradient(90deg,#ffd6db,var(--danger));color:#3b0a0a;box-shadow:0 10px 30px rgba(239,68,68,0.08);border-color:rgba(239,68,68,0.25)}
.pop{margin-top:12px;padding:12px;border-radius:10px;background:linear-gradient(180deg,#ffffff,#fbfdff);border:1px solid #e6eef8;box-shadow:0 10px 30px rgba(2,6,23,0.03)}
.pop strong{display:block;margin-bottom:6px;font-weight:800}
.side{background:var(--card);border-radius:10px;padding:16px;box-shadow:0 10px 30px rgba(12,22,48,0.06);height:max-content}
.progress{height:12px;background:#f1f9ff;border-radius:999px;overflow:hidden;margin-bottom:12px}
.progress > i{display:block;height:100%;background:linear-gradient(90deg,#06b6d4,var(--primary));width:0%;transition:width .36s ease}
.stats{display:flex;flex-direction:column;gap:8px;color:var(--muted);font-weight:700}
.stat-row{display:flex;justify-content:space-between;align-items:center;font-size:0.95rem}
.controls{display:flex;gap:8px;align-items:center;margin-top:12px;flex-wrap:wrap}
.btn{background:var(--primary);color:#fff;padding:10px 14px;border-radius:10px;border:0;font-weight:800;cursor:pointer}
.btn.secondary{background:#fff;color:var(--primary);border:1px solid rgba(30,100,208,0.12)}
.footer{margin-top:18px;text-align:center;color:var(--muted);font-size:0.95rem}
.note{color:var(--muted);font-size:0.95rem;margin-top:8px;text-align:center}
#overlay{position:fixed;inset:0;display:none;align-items:center;justify-content:center;background:rgba(2,6,23,0.46);z-index:9999}
#overlay .panel{background:#fff;padding:18px;border-radius:12px;max-width:520px;width:92%;text-align:center;box-shadow:0 20px 50px rgba(2,6,23,0.3)}
</style>
</head>
<body>
  <div class="container">
    <div class="header">
      <div class="title">محاكي الرخصة المهنية</div>
      <div class="credit">إعداد المعلم: فهد الخالدي</div>
    </div>

    <div class="main">
      <div class="quiz" id="quizColumn">
        <!-- الأسئلة تُولَّد برمجياً -->
      </div>

      <aside class="side">
        <div class="small">شريط التقدم</div>
        <div class="progress"><i id="progressBar"></i></div>
        <div class="stats">
          <div class="stat-row"><span>الإجابات الصحيحة</span><strong id="statCorrect">0</strong></div>
          <div class="stat-row"><span>الأسئلة المجابة</span><strong id="statAnswered">0 / 68</strong></div>
          <div class="stat-row"><span>النسبة</span><strong id="statPercent">0%</strong></div>
        </div>

        <div class="controls">
          <button id="showAll" class="btn">عرض النتيجة</button>
          <button id="resetBtn" class="btn secondary">إعادة الاختبار</button>
        </div>

        <div class="note">اضغط على خيار كل سؤال لعرض الشرح وتلوين الإجابة.</div>
      </aside>
    </div>

    <div class="footer"></div>
  </div>

  <div id="overlay">
    <div class="panel">
      <h2 style="margin:6px 0 10px;color:var(--primary)">النتيجة النهائية</h2>
      <div id="finalScore" style="font-size:2.4rem;font-weight:900;color:var(--success)">0 / 68</div>
      <div id="finalBreak" style="margin-top:10px;color:var(--muted)"></div>
      <button id="closeOverlay" class="btn secondary" style="margin-top:14px">حسناً</button>
    </div>
  </div>

<script>
/* ===========================
   QUESTIONS (68)
   حروف: أ=0 ب=1 ج=2 د=3
   الإجابات الصحيحة مضبوطة بحسب الترتيب الذي أعطيته
   =========================== */

const QUESTIONS = [
{ q: "ما المقصود بالتقويم التكويني في العملية التعليمية؟", choices: ["اختبار نهائي فقط","تقييم مستمر يهدف لتحسين تعلم الطلاب أثناء العملية","جدولة الامتحانات","حفظ السجلات"], correct: 1, explanation: "التقويم التكويني يقدّم معلومات أثناء عملية التعلم ليمكن المعلم من تعديل التدريس وتحسين تعلم الطلاب." },
{ q: "ما أهم سمة في صياغة الهدف التعليمي الجيد؟", choices: ["قابلية القياس","غامض ومفتوح للتفسير","طويل جداً","غير قابل للقياس"], correct: 0, explanation: "الهدف التعليمي الجيد يجب أن يكون قابلاً للقياس ليتم تقييم تحقيقه." },
{ q: "في موقف صفّي: طالب يهيمن على نقاش المجموعة ويمنع الآخرين. ما الإجراء الأنسب؟", choices: ["تجاهل الموقف","تقديم مكافآت","تعيين أدوار وتدويرها","إخراجه من المجموعة"], correct: 2, explanation: "تعيين أدوار وتدويرها يضمن مشاركة الجميع ويقلل الاحتكار." },
{ q: "أي مما يلي يعبر عن الصدق في أدوات القياس؟", choices: ["طول الاختبار","مظهر الأسئلة جيد","وجود صور","قياس الأداة لما صممت لقياسه"], correct: 3, explanation: "الصدق يعني أن الأداة تقيس ما يُقصد قياسه بالفعل." },
{ q: "ماذا يتضمن فهم ضمني من نص يذكر أهمية التحقق من المصادر؟", choices: ["نسخ العبارة","تحديد عدد الكلمات","التحقق من صحة المصادر","حفظ النص"], correct: 3, explanation: "الفهم الضمني هنا يستلزم استنتاج أن التحقق من المصادر مطلوب قبل القبول." },
{ q: "ما معنى معايرة بنوك الأسئلة؟", choices: ["زيادة عدد الأسئلة","تحليل خصائص البنود (الصعوبة والتمييز) اعتمادًا على بيانات","حذف الأسئلة الصعبة","تزيين الأسئلة"], correct: 1, explanation: "المعايرة تعني فحص خصائص البنود باستخدام بيانات لتحديد ملاءمتها." },
{ q: "إذا كانت زوايا مثلث x, x, 40 فكم قيمة x؟", choices: ["40","70","20","100"], correct: 1, explanation: "2x + 40 = 180 → 2x = 140 → x = 70." },
{ q: "ما هو نطاق النمو القريب (ZPD) لفيجوتسكي؟", choices: ["ما يفعله الطفل بمفرده","ما لا يستطيع الطفل فعله مطلقًا","ما يفعله الطفل بمساعدة مختص","مرحلة عمرية"], correct: 3, explanation: "ZPD هو ما يمكن للطفل تحقيقه بمساعدة أكثر خبرة مقارنة بما يفعله بمفرده." },
{ q: "ما أثر التعزيز الإيجابي على السلوك؟", choices: ["يقلل السلوك","يزيد احتمال تكرار السلوك المرغوب","لا تأثير","يجعل السلوك مؤقتاً"], correct: 0, explanation: "التعزيز الإيجابي يزيد من احتمال تكرار السلوك المرغوب." },
{ q: "أي نشاط يطوّر قدرة الطالب على الاستدلال من نص؟", choices: ["عد الكلمات","سؤال لماذا تصرفت الشخصية؟ مع دلائل","حفظ المقطع","قراءة بصوت مرتفع"], correct: 2, explanation: "الاستدلال يتطلب تفسير أفعال الشخصية بدليل من النص." },
{ q: "ما الفكرة الأساسية في النظرية البنائية؟", choices: ["التلقي السلبي","المتعلم يبني المعرفة من خبراته","الحفظ فقط","الاستظهار"], correct: 1, explanation: "البنائية ترى أن المتعلم يبني المعرفة من تفاعل مع الخبرات وربطها بمعارفه السابقة." },
{ q: "ما صيغة سؤال تقيس التحليل؟", choices: ["اذكر فقط","قارن بين نظريتين وبيّن نقاط القوة والضعف","صح/خطأ","عد العناصر"], correct: 3, explanation: "أسئلة المقارنة والتحليل تقيس مهارات تحليلية أعلى." },
{ q: "طالبة متوترة شفهيًا؛ أي تدخل حساس؟", choices: ["التوبيخ أمام الجميع","إعادة صياغة السؤال وتشجيعها","تجاهلها","إجبارها على الإجابة"], correct: 0, explanation: "إعادة الصياغة والتشجيع يساعدان على تقليل التوتر وإتاحة فرصة للإجابة." },
{ q: "أيهما يعد سؤالًا استدلاليًا من نص؟", choices: ["ما مرادف الكلمة؟","لماذا يظن الكاتب كذا؟ وما الدليل؟","كم عدد الفقرات؟","هل النص مفيد؟"], correct: 1, explanation: "السؤال الذي يطلب تفسيرًا وربطًا بين نص ودليل يعد استدلاليًا." },
{ q: "ما فرضية غاردنر حول الذكاءات؟", choices: ["ذكاء واحد يقيس الكل","ذكاءات متعددة (لغوي، منطقي، بصري...)","لا تأثير للذكاء","الذكاء ثابت فقط"], correct: 2, explanation: "غاردنر اقترح تعدد أنواع الذكاء مما يستدعي أساليب تدريس متنوعة." },
{ q: "مسألة: أقل N يحقق N≡5 (mod8) وN≡0 (mod7) هو:", choices: ["21","57","41","65"], correct: 3, explanation: "الحل بالمطابقة يعطي N=65 كأصغر عدد يحقق الشروط." },
{ q: "ما المقصود بالثبات (Reliability)؟", choices: ["تغير الدرجات عشوائياً","اتساق النتائج عند إعادة التطبيق","عدد البنود","سرعة الإجابة"], correct: 0, explanation: "الثبات يعبر عن مدى اتساق النتائج عبر الزمن أو عبر صيغ متكافئة." },
{ q: "في فصل متعدد المستويات، استراتيجية مناسبة:", choices: ["نقل الطلاب إلى صفوف متجانسة","التعلم المتمايز مع مهام تمديد وتسهيل","تقليل وقت الحصة","إلغاء الأنشطة الجماعية"], correct: 1, explanation: "التمايز يوفر تحديًا للمتفوق ودعمًا للضعيف داخل نفس الحصة." },
{ q: "العنصر الأساسي في هدف درس قابل للتقييم:", choices: ["هدف عام","هدف قابل للقياس بإجراء محدد","نص طويل","تعريف غامض"], correct: 2, explanation: "الأهداف الإجرائية والواضحة تسهل قياس تحققها." },
{ q: "نشاط يعزز إعادة الصياغة:", choices: ["حفظ النص","كتابة ملخصات بمعايير تقييمية","قراءة فقط","التلقين"], correct: 0, explanation: "الملخصات مع معايير تساعد على إعادة صياغة الأفكار بدقة." },
{ q: "أهمية فحص متوسط صعوبة البنود:", choices: ["لا فائدة","ضمان توازن صعوبة البنود","زيادة الطول","تقليل الدقة"], correct: 2, explanation: "متوسط الصعوبة يساعد على ضمان توزيع مناسب بين البنود السهلة والصعبة." },
{ q: "ما حاصل 12 × 7؟", choices: ["84","72","94","82"], correct: 0, explanation: "12 مضروبة في 7 تساوي 84." },
{ q: "ما أداة مناسبة لتقييم مشروع تعاوني؟", choices: ["سؤال متعدد واحد","قائمة تحقق + ملاحظة + ملف إنجاز","اختبار شفهي واحد","مسح سريع"], correct: 1, explanation: "مشروعات تعاونية تحتاج أدوات متعددة لتقييم العملية والمنتج." },
{ q: "من علماء السلوكية المرتبطين بالتعزيز؟", choices: ["بياجيه","سكنر","غاردنر","فيجوتسكي"], correct: 1, explanation: "ب. ف. سكنر أسهم كثيرًا في نظرية التعزيز والسلوكية." },
{ q: "أي جملة صحيحة نحويًا؟", choices: ["ذهبتُ إلى المكتبة وقرأت كتابًا مفيدًا","ذهبتُ ...","ذهبتَ ... كتابًا","ذهبتُ ... كتابًا مفيدة"], correct: 0, explanation: "الجملة الأولى مركبة صحيحة من حيث التركيب والمعنى." },
{ q: "ما عنصر أساسي في درس متعدد الثقافات؟", choices: ["تجاهل الهويات","إدراج أمثلة من ثقافات متنوعة","فرض ثقافة واحدة","منع النقاش"], correct: 1, explanation: "تنويع الأمثلة يربط المحتوى بخلفيات الطلبة." },
{ q: "متوسط المجموعة {2,4,6,8} و{1,3,5} تقريبا:", choices: ["4.5","5","4","3.5"], correct: 0, explanation: "المتوسط التقريبي للعناصر المجمعة يقترب من 4.5." },
{ q: "الصدق الظاهري يعني:", choices: ["مؤشر إحصائي","مظهر الاختبار كموثوق للمستخدمين","مقياس صريح","مستوى سهولة"], correct: 1, explanation: "الصدق الظاهري يتعلق بإحساس المستخدم بأن الاختبار يبدو مناسبًا." },
{ q: "تدخل لبناء ثقة طالب خجول:", choices: ["إجباره على التحدث","مهام جماعية بأدوار صغيرة","السخرية","التجاهل"], correct: 0, explanation: "الأدوار الصغيرة تشجع المشاركة التدريجية دون إحراج." },
{ q: "دور المعلم في البيئة الرقمية:", choices: ["موزع ملفات فقط","ميسّر يصمم أنشطة ويعطي تغذية راجعة","مراقب صامت","مستخدم تقليدي"], correct: 1, explanation: "المعلم الرقمي ييسّر الخبرة ويقدم ملاحظات فورية." },
{ q: "ما مؤشر ترابط النص؟", choices: ["حجم الخط","وضوح الروابط الانتقالية","عدد الفقرات","طول الجملة"], correct: 2, explanation: "الروابط الانتقالية تعكس ترابط الأفكار في النص." },
{ q: "مسألة: أقل N يحقق N≡5 (mod12) وN≡0 (mod5) هو:", choices: ["21","65","41","29"], correct: 2, explanation: "الحل الصحيح هنا بحسب الخيارات المعطاة هو 41 (مطابقة القواسم والباقي)." },
{ q: "من نظرية التعلم بالملاحظة؟", choices: ["بياجيه","باندورا","سكنر","فرويد"], correct: 1, explanation: "باندورا طوّر نظرية التعلم الاجتماعي التي تؤكد على الملاحظة والنمذجة." },
{ q: "التمايز في التدريس يعني:", choices: ["تقديم نفس المهمة للجميع","تعديل المحتوى والعملية لتلبية الاحتياجات","زيادة الواجبات","تقليل الجودة"], correct: 1, explanation: "التمايز يكيّف التعليم لاحتياجات المتعلمين المختلفة." },
{ q: "تمرين يحسّن تركيب النص:", choices: ["تدريب على أدوات الربط والانتقال","حفظ القواعد فقط","القراءة بصوت عال","الكتابة السريعة"], correct: 2, explanation: "ممارسة أدوات الربط تعزّز ترابط الفقرات." },
{ q: "ما الذي ينبغي تجنبه في بنود الاختبار الموضوعي؟", choices: ["غموض الصياغة","تنويع مستويات التفكير","وضوح الصياغة","زمن مناسب"], correct: 0, explanation: "البنود الغامضة تقلل من صدق الاختبار." },
{ q: "دور التقييم أثناء المشروع:", choices: ["حكم نهائي فقط","تغذية راجعة أثناء العمل وتقييم المنتج والعملية","اختبار فصلي","إلغاء التقييم"], correct: 1, explanation: "التقييم المستمر يساعد على تحسين المنتج والعملية." },
{ q: "حل 3x - 9 = 0، فـ x =", choices: ["-3","0","3","9"], correct: 2, explanation: "3x = 9 → x = 3." },
{ q: "الاستنتاج مقابل الاستدلال:", choices: ["نفس المعنى","الاستنتاج من دلائل النص، الاستدلال يربط بالسياق الخارجي","حفظي","لا فرق"], correct: 1, explanation: "الاستنتاج ينبني على دلائل النص، والاستدلال قد يستدعي ربط معلومات خارجية." },
{ q: "قاعدة صفّية لخفض التشويش:", choices: ["منع الكلام نهائيًا","اتفاق إشارات دورية وآداب الاستماع","التوبيخ","إلغاء الأنشطة"], correct: 1, explanation: "إشارات الدور تحفظ التنظيم وتقلل التشويش." },
{ q: "معامل التمييز يقيس:", choices: ["صعوبة البند","قدرة البند على تمييز الأداء العالي عن الضعيف","طول السؤال","عدد الكلمات"], correct: 1, explanation: "معامل التمييز يوضح ما إذا كان البند يفرّق بين مستويات الطلاب." },
{ q: "كيف يؤثر التقويم التكويني على تصميم الأنشطة؟", choices: ["لا تأثير","يوجه لتعديل الأنشطة لمعالجة الصعوبات","يجعل الأنشطة أطول","يجعلها أكثر تعقيدًا"], correct: 1, explanation: "التقويم التكويني يساعد المعلم على تعديل الأنشطة وفقًا لاحتياجات المتعلمين." },
{ q: "من فوائد التكنولوجيا في التعليم:", choices: ["تقييد الوصول","توفير مصادر وتغذية راجعة فورية","رفع التكلفة بلا فائدة","تخفيف دور المعلم"], correct: 2, explanation: "التكنولوجيا توسع المصادر وتتيح تغذية راجعة سريعة." },
{ q: "ما الذي يجب تجنبه في مشتتات متعدد الاختيارات؟", choices: ["مشتتات مبنية على أخطاء شائعة","خيارات متساوية الطول","خيارات منطقية","صياغة واضحة"], correct: 0, explanation: "مشتتات سيئة تقلل من صدق البند." },
{ q: "الكتابة الصحيحة للكلمة هي:", choices: ["استيعاب","استيعآب","استياع","استيعابًا"], correct: 0, explanation: "التهجئة الصحيحة هي 'استيعاب'." },
{ q: "ما الذي يشجع التعلم الذاتي؟", choices: ["التلقين","مشروعات بحثية ومسؤوليات","التكرار فقط","الاختبارات المتكررة"], correct: 1, explanation: "المشروعات تمنح الطالب مسؤولية وحرية البحث والتعلم." },
{ q: "مجموع 5,7,8 هو:", choices: ["18","20","21","19"], correct: 1, explanation: "5+7+8 = 20." },
{ q: "ما الأساس لبناء معيار موضوعي؟", choices: ["الانطباع","معايير واضحة ومؤشرات سلوكية","طول الاختبار","عدد الأسئلة"], correct: 0, explanation: "المعايير الواضحة تضمن موضوعية التقييم." },
{ q: "خطة لتحسين مشاركة طلاب مستوى اللغة المنخفض:", choices: ["الصرامة","تبسيط التعليمات واستخدام مرئيات وأدوار صغيرة","تجاهلهم","التركيز على المتفوقين"], correct: 1, explanation: "تبسيط التعليمات والمرئيات يساعد على المشاركة." },
{ q: "الدافعية في التعلم تؤثر على:", choices: ["لا شيء","اتجاه الجهد واستمراريته","تلف الذاكرة","زيادة الأخطاء"], correct: 2, explanation: "الدافعية تحدد مقدار الجهد واستمراريته وبالتالي نتائج التعلم." },
{ q: "سؤال يقيس البلاغة:", choices: ["ما أثر التشبيه في النص؟","كم عدد الأسطر؟","اذكر كلمة","هل النص طويل؟"], correct: 0, explanation: "تحليل أثر التشبيه يتطلب قدرة بلاغية." },
{ q: "احسب (5^2)+(3^3):", choices: ["34","52","32","35"], correct: 1, explanation: "25 + 27 = 52." },
{ q: "مبدأ الحد الأدنى من التداخل يعني:", choices: ["زيادة التداخل","تقليل تداخل مهام التقييم لتقدير مساهمة الفرد","إلغاء الفردية","زيادة التداخل"], correct: 1, explanation: "تقليل التداخل يسهل قياس مساهمة كل فرد." },
{ q: "تعديل مفيد لذوي الصعوبات:", choices: ["نصوص طويلة","تجزئة المحتوى مع مرئيات","زيادة الواجبات","لغة معقدة"], correct: 1, explanation: "التجزئة والمرئيات تدعم الفهم." },
{ q: "كيف تنمي المدرسة مهارات المواطنة؟", choices: ["منع النقاش","خدمة المجتمع ومناظرات","التركيز على الاختبارات","الانفصال عن المجتمع"], correct: 1, explanation: "الخدمة والمناقشات تعزز المواطنة الفاعلة." },
{ q: "مؤشر التشتت الشائع هو:", choices: ["المتوسط","الانحراف المعياري","عدد الأسئلة","عدد المصححين"], correct: 1, explanation: "الانحراف المعياري يقيس تشتت القيم حول المتوسط." },
{ q: "المعنى الدلالي مقابل الحرفي:", choices: ["نفس المعنى","الحرفي ما تقوله الجملة، الدلالي يشمل الرموز والسياق","الدلالي قواعد فقط","الحرفي تفسير القارئ"], correct: 0, explanation: "الدلالي يشمل رموز ودلالات أوسع من الحرفي." },
{ q: "ناتج 9 ÷ 3 + 2 × 4 =", choices: ["11","10","14","9"], correct: 0, explanation: "9÷3=3 و2×4=8 → 3+8=11." },
{ q: "وجود روبريك واضح يؤدي إلى:", choices: ["زيادة الفروق بين المصححين","ثبات التقدير","العشوائية","تغير الخطة"], correct: 1, explanation: "الروبريك يقلل فروق الحكم بين المصححين." },
{ q: "الإجراء الأول عند غياب متكرر:", choices: ["العقاب","التواصل مع ولي الأمر","إلغاء المشاركة","نقل الطالب"], correct: 0, explanation: "التواصل مع الأسرة يفهم الأسباب ويساعد في وضع حلول." },
{ q: "من ربط الشرط الكلاسيكي؟", choices: ["بياجيه","بافلوف","سكنر","فيجوتسكي"], correct: 1, explanation: "إيفان بافلوف هو صاحب تجارب الشرط الكلاسيكي." },
{ q: "لماذا نستخدم مشتتات مبنية على أخطاء شائعة؟", choices: ["لتضليل الطلاب","لكشف الأخطاء المفهومية","لتطويل الاختبار","لتسهيله"], correct: 0, explanation: "المشتتات تكشف إن الطالب وقع في نفس الخطأ المفهومي." },
{ q: "ما أصغر عدد يحقق: باقي 4 عند القسمة على 6 وقابل للقسمة على 5؟", choices: ["24","34","19","9"], correct: 1, explanation: "العدد 34 يحقق الشروط في الخيارات المقدمة." },
{ q: "التعلم القائم على الكفاءة يركز على:", choices: ["مدة الحصة","إتقان المعايير بغض النظر عن الزمن","زيادة الاختبارات","عدد المهام"], correct: 1, explanation: "التركيز على أن المتعلم يظهر إتقان الكفاءة المطلوبة." },
{ q: "مثال استعارة بلاغية هو:", choices: ["البحر هادئ","البحر صفحة بيضاء","البحر طويل","البحر واسع"], correct: 2, explanation: "التعبير 'صفحة بيضاء' يحوّل البحر إلى صورة بلاغية." },
{ q: "فائدة أخذ عينات من بنك الأسئلة:", choices: ["زيادة العبء","تنويع البنود وتقليل الحفظ","زيادة التعقيد","تقليل التحليل"], correct: 1, explanation: "أخذ العينات يساعد على تنويع الاختبارات وتقليل إمكانية الحفظ." },
{ q: "أهم عامل لنجاح التدريس هو:", choices: ["وجود كتب فقط","وضوح الأهداف وتوافق الأنشطة والتقويم","زيادة الحصص","استخدام وسائل بلا هدف"], correct: 1, explanation: "التوافق بين الأهداف والأنشطة والتقويم أساس نجاح الدرس." }
]; // نهاية QUESTIONS

/* ===========================
   إنشاء البطاقات والتفاعل
   =========================== */
const quizColumn = document.getElementById('quizColumn');
const total = QUESTIONS.length;

function createCard(Q, idx) {
  const card = document.createElement('div');
  card.className = 'card';
  card.dataset.answered = '';
  const head = document.createElement('div');
  head.className = 'q-head';
  head.innerHTML = `<div class="q-num">س ${idx+1}</div><div style="font-weight:800;color:var(--muted)">سؤال ${idx+1}</div>`;
  card.appendChild(head);
  const sent = document.createElement('div');
  sent.className = 'sentence';
  sent.innerHTML = Q.q;
  card.appendChild(sent);
  const choicesDiv = document.createElement('div');
  choicesDiv.className = 'options';
  const labels = ['أ','ب','ج','د'];
  Q.choices.forEach((c,i) => {
    const btn = document.createElement('div');
    btn.className = 'opt';
    btn.innerHTML = `<div style="font-weight:800;display:inline-block;margin-left:10px">${labels[i]})</div> <div style="display:inline-block;vertical-align:top">${c}</div>`;
    btn.addEventListener('click', () => {
      if (btn.classList.contains('disabled')) return;
      Array.from(choicesDiv.children).forEach(x => x.classList.add('disabled'));
      if (i === Q.correct) {
        btn.classList.add('correct');
        card.dataset.answered = 'correct';
        showPop(card, `<strong style="color:var(--success)">إجابة صحيحة ✔</strong><div style="margin-top:6px">${Q.explanation}</div>`);
      } else {
        btn.classList.add('wrong');
        if (choicesDiv.children[Q.correct]) choicesDiv.children[Q.correct].classList.add('correct');
        card.dataset.answered = 'wrong';
        showPop(card, `<strong style="color:var(--danger)">إجابة خاطئة ✘</strong><div style="margin-top:6px">${Q.explanation}</div>`);
      }
      updateSummary();
    });
    choicesDiv.appendChild(btn);
  });
  card.appendChild(choicesDiv);
  return card;
}

QUESTIONS.forEach((q, idx) => {
  quizColumn.appendChild(createCard(q, idx));
});

/* Popup داخل البطاقة */
function showPop(card, html) {
  const old = card.querySelector('.pop');
  if (old) old.remove();
  const p = document.createElement('div');
  p.className = 'pop';
  p.innerHTML = html;
  card.appendChild(p);
  card.scrollIntoView({behavior:'smooth', block:'center'});
}

/* ملخّص / شريط تقدم */
const statCorrect = document.getElementById('statCorrect');
const statAnswered = document.getElementById('statAnswered');
const statPercent = document.getElementById('statPercent');
const progressBar = document.getElementById('progressBar');
const finalScore = document.getElementById('finalScore');

function updateSummary() {
  let correct = 0;
  let answered = 0;
  document.querySelectorAll('.card').forEach(c => {
    if (c.dataset.answered === 'correct') correct++;
    if (c.dataset.answered === 'correct' || c.dataset.answered === 'wrong') answered++;
  });
  statCorrect.textContent = correct;
  statAnswered.textContent = `${answered} / ${total}`;
  const percent = Math.round((correct / total) * 100);
  statPercent.textContent = `${percent}%`;
  progressBar.style.width = `${Math.round((answered/total)*100)}%`;
  if (finalScore) finalScore.textContent = `${correct} / ${total}`;
}

/* أزرار */
document.getElementById('showAll').addEventListener('click', () => {
  updateSummary();
  const correct = Number(statCorrect.textContent);
  const totalQ = total;
  const panelBreak = document.getElementById('finalBreak');
  if (correct === totalQ) panelBreak.innerText = 'ممتاز — إجابات جميعها صحيحة!';
  else if (correct >= Math.ceil(totalQ * 0.85)) panelBreak.innerText = 'ممتاز جداً — مستوى متقدم!';
  else if (correct >= Math.ceil(totalQ * 0.7)) panelBreak.innerText = 'جيد جدًا — أداء قوي';
  else if (correct >= Math.ceil(totalQ * 0.5)) panelBreak.innerText = 'متوسط — يحتاج مراجعة مركزة';
  else panelBreak.innerText = 'ضعيف — أنصح بمراجعة الأساسيات والتدريب المكثف';
  document.getElementById('overlay').style.display = 'flex';
});

document.getElementById('closeOverlay').addEventListener('click', () => {
  document.getElementById('overlay').style.display = 'none';
});

document.getElementById('resetBtn').addEventListener('click', () => {
  document.querySelectorAll('.card .pop').forEach(p => p.remove());
  document.querySelectorAll('.card').forEach(card => {
    card.dataset.answered = '';
    card.querySelectorAll('.opt').forEach(btn => {
      btn.classList.remove('disabled','correct','wrong');
    });
  });
  updateSummary();
  window.scrollTo({top:0, behavior:'smooth'});
});

/* Initialize */
updateSummary();
</script>
</body>
</html>
