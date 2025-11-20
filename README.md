
<html lang="ar" dir="rtl">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>محاكي — إعداد المعلم : فهد الخالدي</title>
<style>
:root{
  --primary:#155bd7;
  --muted:#6b7280;
  --card:#ffffff;
  --success:#16a34a;
  --danger:#ef4444;
}
*{box-sizing:border-box}
body{
  margin:0;padding:18px;background:linear-gradient(180deg,#f4f8ff 0%,#ffffff 100%);
  font-family: "Almarai", "Noto Kufi Arabic", Tahoma, Arial, sans-serif;
  color:#0b1220;
}
.container{max-width:1100px;margin:0 auto;background:var(--card);padding:18px;border-radius:12px;box-shadow:0 10px 40px rgba(12,22,48,0.06)}
.header{display:flex;justify-content:center;align-items:center;padding:8px 0;border-bottom:1px solid #eef2ff}
.header h1{margin:0;font-size:1.15rem;color:var(--primary);font-weight:800}
.main{display:grid;grid-template-columns:1fr 340px;gap:18px;margin-top:18px}
@media(max-width:980px){.main{grid-template-columns:1fr}}
.quiz{display:flex;flex-direction:column;gap:14px;padding-right:8px}
.card{background:linear-gradient(180deg,#ffffff,#fbfdff);border-radius:10px;padding:12px;box-shadow:0 8px 30px rgba(2,6,23,0.04);transition:transform .15s;overflow:visible}
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
.note{color:var(--muted);font-size:0.95rem;margin-top:8px}
</style>
</head>
<body>
<div class="container">
  <div class="header">
    <h1>إعداد المعلم : فهد الخالدي</h1>
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

<!-- نتيجة نهائية -->
<div id="overlay" style="position:fixed;inset:0;display:none;align-items:center;justify-content:center;background:rgba(2,6,23,0.46);z-index:9999">
  <div style="background:#fff;padding:18px;border-radius:12px;max-width:520px;width:92%;text-align:center;box-shadow:0 20px 50px rgba(2,6,23,0.3)">
    <h2 style="margin:6px 0 10px;color:var(--primary)">النتيجة النهائية</h2>
    <div id="finalScore" style="font-size:2.4rem;font-weight:900;color:var(--success)">0 / 68</div>
    <div id="finalBreak" style="margin-top:10px;color:var(--muted)"></div>
    <button id="closeOverlay" class="btn secondary" style="margin-top:14px">حسناً</button>
  </div>
</div>

<script>
/* ========== مصفوفة الأسئلة (68) ========== */
/* كل عنصر: q, choices (4 عناصر), correct (index 0..3), explanation */
const QUESTIONS = [
{ q:"ما المقصود بالتقويم التكويني في العملية التعليمية؟", choices:["قياس نهائي للمخرجات","تقييم مستمر يهدف لتحسين تعلم الطلاب أثناء العملية","بديل للدرجات","سجل حضور"], correct:1, explanation:"التقويم التكويني هو تقييم مستمر أثناء التعلم يساعد المعلم على تعديل التدريس وتحسين تعلم الطلاب."},
{ q:"حسب بياجيه، أي مرحلة تتصف بالتفكير المجرد؟", choices:["الحسية الحركية","ما قبل التشغيل","العمليات الملموسة","العمليات الشكلية (الصورية)"], correct:3, explanation:"المرحلة العملياتية الشكلية تظهر فيها القدرة على التفكير المجرد والاستنباط المنطقي."},
{ q:"في موقف صفّي: طالب يهيمن على المناقشة. الإجراء المناسب هو:", choices:["تجاهل الأمر","تعيين أدوار وتدويرها داخل المجموعة","منعه من الحضور","مكافأته على الحديث"], correct:1, explanation:"تعيين الأدوار وتدويرها يضمن مشاركة متوازنة ويقلل الاحتكار."},
{ q:"ما معنى الصدق (Validity) للاختبار؟", choices:["اتساق النتائج عند التكرار","قياس الاختبار لما صُمم لقياسه","سرعة الإجابة","طول الاختبار"], correct:1, explanation:"الصدق يعني أن الأداة تقيس الخاصية التي يفترض قياسها بالفعل."},
{ q:"اقرأ المقطع: 'التفكير النقدي يمنع الانسياق وراء معلومات خاطئة'. ما مهارة القياس هنا؟", choices:["حفظ الجملة","استنتاج أن التحقق من المصادر ضروري","عد الكلمات","مرادف الانسياق"], correct:1, explanation:"الفهم الضمني يتطلب استنتاج أهمية التحقق من المصادر من المقطع."},
{ q:"ما المقصود بمعايرة بنوك الأسئلة؟", choices:["صياغة الشّكل فقط","تحليل خصائص البنود (الصعوبة والتمييز) اعتمادًا على بيانات","زيادة عدد الأسئلة","حذف البنود المعقدة"], correct:1, explanation:"المعايرة تعتمد على بيانات سابقة لتحديد صفة البند ومدى ملاءمته."},
{ q:"إذا كانت زوايا مثلث: x,x,40 فما قيمة x؟", choices:["40","70","20","100"], correct:1, explanation:"2x+40=180 → 2x=140 → x=70."},
{ q:"ما هو نطاق النمو القريب (ZPD) عند فيجوتسكي؟", choices:["ما يفعله الطفل بمفرده","ما يفعله الطفل بمساعدة","مهارات خارج قدرة الطفل تماماً","مرحلة عمرية"], correct:1, explanation:"ZPD هو الفرق بين ما يستطيع المتعلم فعله بمفرده وما يستطيع إنجازه بمساعدة شخص أكثر خبرة."},
{ q:"ما تأثير التعزيز الإيجابي وفق السلوكيات التطبيقية؟", choices:["تقليل السلوك","زيادة احتمال تكرار السلوك المرغوب","لا تأثير","تأثير مؤقت فقط"], correct:1, explanation:"التعزيز الإيجابي يزيد من احتمال تكرار السلوك المرغوب بتقديم نتيجة إيجابية."},
{ q:"أي نشاط يطوّر الاستنتاج من نص؟", choices:["نسخ مقطع","السؤال عن سبب تصرف شخصية مع دلائل","عد الأسطر","قراءة بصوت عال"], correct:1, explanation:"الاستنتاج يتطلب ربط دلائل النص لتفسير الأسباب والنتائج."},
{ q:"ما مبدأ البنائية (Constructivism) في التعلم؟", choices:["التلقي السلبي","بناء المتعلم لمعناه من الخبرات السابقة","الحفظ فقط","الانعزال عن الخبرات"], correct:1, explanation:"البنائية ترى أن المتعلم ينشئ المعرفة من خلال التفاعل مع الخبرة وربطها بمعرفته السابقة."},
{ q:"صياغة سؤال لمستوى التحليل تكون كالتالي:", choices:["اذكر فقط","قارن بين نظريتين وبيّن نقاط القوة والضعف","صح/خطأ","ما اسم العالم؟"], correct:1, explanation:"أسئلة المقارنة والتحليل تقيس مهارات تحليلية أعلى من الحفظ."},
{ q:"طالبة متوترة شفهيًا؛ التدخل الحساس هو:", choices:["التوبيخ أمام الجميع","إعادة صياغة السؤال وتشجيعها","التجاهل","خفض الدرجة فورًا"], correct:1, explanation:"إعادة الصياغة والتشجيع يقللان التوتر ويتيحان للطالب فرصة للإجابة."},
{ q:"أي سؤال يقيس الاستدلال من نص؟", choices:["لماذا يظن الكاتب كذا؟ وما الدليل؟","ما تعريف كلمة؟","كم عدد الفقرات؟","هل النص طويل؟"], correct:0, explanation:"سؤال الاستدلال يطلب تفسيرًا ودليلًا من النص."},
{ q:"ما فرضية غاردنر عن الذكاءات؟", choices:["ذكاء واحد فقط","ذكاءات متعددة (لغوي، منطقي، موسيقي...)","لا تأثير للذكاء","الذكاء ثابت فقط"], correct:1, explanation:"غاردنر اقترح وجود أنواع متعددة من الذكاء تؤثر في طرق التدريس."},
{ q:"مسألة: أقل عدد N يحقق N≡5 (mod 8) وN≡0 (mod7) هو:", choices:["21","57","41","65"], correct:0, explanation:"N=8k+5، نبحث k بحيث 8k+5 ≡ 0 mod7 → k≡2 mod7 → k=2 → N=8*2+5=21."},
{ q:"ما المقصود بالثبات (Reliability)؟", choices:["تغير الدرجات عشوائياً","اتساق النتائج عبر مرات التطبيق","عدد البنود","سرعة الإجابة"], correct:1, explanation:"الثبات يعنى أن النتيجة مستقرة ومتسقة عند تكرار القياس."},
{ q:"في فصل متعدد المستويات، استراتيجية مناسبة:", choices:["فصل الطلاب تمامًا","التعلم المتمايز مع مهام تمديدية ومسارات مبسطة","الصرامة فقط","إلغاء الأنشطة الجماعية"], correct:1, explanation:"التعلم المتمايز يوفر تحدياً للمتفوق ودعماً للضعيف داخل نفس الحصة."},
{ q:"الهدف التعليمي الجيد يجب أن يكون:", choices:["غامض","قابلًا للقياس","عامًا جدًا","طويلًا بلا فعل"], correct:1, explanation:"الأهداف الإجرائية والواضحة تسهل قياس تحققها."},
{ q:"نشاط يعزز إعادة الصياغة:", choices:["حفظ النص","كتابة ملخص بمعايير تقييمية","قراءة فقط","التلقين"], correct:1, explanation:"الملخصات مع معايير تمكن الطالب من إعادة صياغة الفكرة بدقة."},
{ q:"أهمية فحص متوسط صعوبة البنود:", choices:["لا أهمية","ضمان توازن صعوبة البنود","زيادة الطول","تقليل الدقة"], correct:1, explanation:"متوسط الصعوبة يساعد على ضبط توازن الاختبار بين سهل وصعب."},
{ q:"ما حاصل 12 × 7؟", choices:["82","84","72","96"], correct:1, explanation:"12×7=84."},
{ q:"أداة مناسبة لتقييم مشروع تعاوني:", choices:["قائمة تحقق + ملاحظة + ملف إنجاز","سؤال متعدد واحد","تقرير غير منظم","اختبار قصير"], correct:0, explanation:"تحتاج المشاريع أدوات متعددة لقياس العملية والمنتج."},
{ q:"من علماء التعلم المرتبطين بالتعزيز؟", choices:["بياجيه","سكنر","غاردنر","فيجوتسكي"], correct:1, explanation:"ب.ف. سكنر طوّر نظرية التعزيز والسلوك."},
{ q:"أي جملة نحوية صحيحة؟", choices:["ذهبتُ إلى المكتبة وقرأت كتابًا مفيدًا","ذهبتُ ... كتابًا مفيد","ذهبتُ ... قرأتَ كتابًا","ذهبتُ ... كتابًا مفيدة"], correct:0, explanation:"الصيغة الأولى صحيحة نحويًا."},
{ q:"عنصرا أساسيًا في درس متعدد الثقافات:", choices:["تجاهل الهويات","إدراج أمثلة من ثقافات متنوعة","فرض ثقافة واحدة","منع المشاركة"], correct:1, explanation:"تنويع الأمثلة يعزز صلة المحتوى بخلفيات الطلاب."},
{ q:"متوسط المجموعة الموحدة {2,4,6,8} و{1,3,5} هو تقريبًا:", choices:["4.5","5","4","3.5"], correct:0, explanation:"عند توحيد القيم المعدل يكون حوالي 4.5."},
{ q:"الصدق الظاهري يعني:", choices:["التوافق مع معيار خارجي","مظهر الاختبار كمعقول للمستخدمين","الثبات عبر الزمن","مستوى الصعوبة"], correct:1, explanation:"الصدق الظاهري يتعلق بمظهر الأداة واعتقاد المستخدمين بملاءمتها."},
{ q:"تدخل لبناء ثقة الطالب الخجول:", choices:["إجباره","مهام جماعية بأدوار صغيرة","السخرية","التجاهل"], correct:1, explanation:"الأدوار الصغيرة تشجع المشاركة بدون ضغط."},
{ q:"دور المعلم في البيئة الرقمية:", choices:["موزع ملفات","ميسر يصمم أنشطة ويعطي تغذية راجعة","مراقب صامت","مستخدم تقليدي"], correct:1, explanation:"المعلم الرقمي ميسر ويصمم أنشطة تفاعلية."},
{ q:"مقياس الاتساق والترابط في النص يقاس بـ:", choices:["حجم الخط","وضوح الروابط الانتقالية بين الفقرات","عدد الفقرات","طول الجملة"], correct:1, explanation:"الروابط الانتقالية تدل على ترابط منطقي بين الأفكار."},
{ q:"مشكلة: أقل N يحقق N≡5 (mod12) وN≡0 (mod5) هو:", choices:["21","65","41","29"], correct:1, explanation:"نحتاج N=12k+5 و12k+5 ≡0 (mod5) → k≡0 mod5 → k=5 → N=65."},
{ q:"باندورا أكد أن التعلم يحدث بـ:", choices:["التجربة فقط","الملاحظة والنمذجة","التلقين فقط","العقاب فقط"], correct:1, explanation:"التعلم الاجتماعي يركز على الملاحظة والنمذجة."},
{ q:"التمايز في التدريس يعني:", choices:["تقديم نفس المهمة","تعديل المحتوى والعملية لتلبية الاحتياجات","زيادة الواجبات","توحيد التقييم"], correct:1, explanation:"التمايز مصمم لمراعاة الفروق بين الطلاب."},
{ q:"تمرين يحسن بنية النص الكتابي:", choices:["تدريب على أدوات الربط والانتقال","حفظ القواعد","قراءة فقط","طباعة النص"], correct:0, explanation:"التدريب العملي على الربط يحسن تركيب الفقرات."},
{ q:"ما الذي يجب تجنبه في بنود الاختبار الموضوعي؟", choices:["بنود غامضة","تنويع مستويات التفكير","وضوح الصياغة","وقت مناسب"], correct:0, explanation:"البنود المربكة تقلل من جودة الاختبار."},
{ q:"دور التقييم في التعلم بالمشروع:", choices:["حكم نهائي فقط","تغذية راجعة أثناء العمل وتقييم المنتج والعملية","اختبارات قصيرة فقط","إلغاء التقييم"], correct:1, explanation:"التقييم أثناء المشروع يدعم تعلم الطالب ويقيس المنتج والعملية."},
{ q:"حل 3x - 9 = 0، x = ?", choices:["-3","0","3","9"], correct:2, explanation:"3x=9 → x=3."},
{ q:"الاستنتاج مقابل الاستدلال:", choices:["نفس المعنى","الاستنتاج من النص، الاستدلال يربط السياق بمعارف خارجية","كلاهما حفظ","لا فرق"], correct:1, explanation:"الاستدلال قد يشمل ربط النص بمعارف خارجية."},
{ q:"قاعدة صفّية لخفض التشويش عند الإجابة:", choices:["منع الكلام مطلقًا","اتفاق إشارات دورية وآداب الاستماع","التوبيخ","إلغاء الأنشطة"], correct:1, explanation:"إشارات الدور والتنظيم تتيح الحوار دون فوضى."},
{ q:"معامل التمييز يقيس:", choices:["صعوبة البند فقط","قدرة البند على تمييز مستويات الطلاب","طول السؤال","عدد الكلمات"], correct:1, explanation:"المعامل يحدد ما إذا كان البند يفرق بين القادرين وغير القادرين."},
{ q:"كيف يؤثر التقويم التكويني على تصميم الأنشطة؟", choices:["لا تأثير","يوجه لتعديل الأنشطة لملاءمة احتياجات المتعلم","يجعل الأنشطة أطول","يخفض المعايير"], correct:1, explanation:"التقويم التكويني يرشد المعلم لتعديل الأنشطة لمعالجة الصعوبات فورًا."},
{ q:"من فوائد التكنولوجيا في التعليم:", choices:["تقييد الوصول","توفير مصادر متنوعة وتغذية راجعة فورية","ارتفاع التكلفة بلا فائدة","تخفيض الجودة"], correct:1, explanation:"التكنولوجيا توسع المصادر وتدعم التفاعل والتقويم الفوري."},
{ q:"ما الذي يجب تجنبه في خيارات الاختيار من متعدد؟", choices:["مشتتات مبنية على أخطاء شائعة","خيارات متساوية الطول","خيارات منطقية","صياغة واضحة"], correct:0, explanation:"مشتتات مبنية بشكل سيء تشوش نتائج القياس."},
{ q:"صيحيح إملائي: الكلمات الصحيحة هي:", choices:["استيعاب","استيعآب","استياع","استيعابًا"], correct:0, explanation:"الكتابة الصحيحة 'استيعاب'."},
{ q:"طرق تشجع التعلم الذاتي:", choices:["التلقين","مشروعات بحثية ومسؤوليات","التكرار فقط","الاختبارات المتكررة فقط"], correct:1, explanation:"المشروعات تمنح الطالب مسؤولية وسعيًا للبحث والتفكير."},
{ q:"ما مجموع 5،7،8؟", choices:["18","20","21","19"], correct:1, explanation:"5+7+8=20."},
{ q:"عامل أساسي لبناء معيار موضوعي:", choices:["الانطباع","معايير واضحة ومؤشرات سلوكية","طول الاختبار","عدد الأسئلة"], correct:1, explanation:"المعايير الواضحة تساعد في تقييم موضوعي ومتسق."},
{ q:"خطة لتحسين مشاركة طلاب ذوي مستوى لغة منخفض:", choices:["الصرامة","تبسيط التعليمات واستخدام مرئيات وأدوار صغيرة","تجاهلهم","تركيز على المتفوقين"], correct:1, explanation:"تبسيط التعليمات والمرئيات مع أدوار صغيرة يساعد المشاركة."},
{ q:"دور الدافعية في التعلم:", choices:["لا دور","توجيه الجهد واستمراريته","تعطل التعلم","تأثير مؤقت"], correct:1, explanation:"الدافعية تؤثر في مستوى الجهد والاستمرار وبالتالي في التعلم."},
{ q:"سؤال يقيس البلاغة:", choices:["ما أثر التشبيه في النص؟","كم عدد الأسطر؟","اذكر كلمة","هل النص مفيد؟"], correct:0, explanation:"تحليل تأثير التشبيه يقيس مهارات البلاغة."},
{ q:"حل (5^2)+(3^3):", choices:["34","52","32","35"], correct:1, explanation:"5^2=25، 3^3=27 → المجموع 52."},
{ q:"مبدأ الحد الأدنى من التداخل يعني:", choices:["زيادة التداخل","تقليل تداخل مهام التقييم لتحديد مسؤولية الفرد","إلغاء الفردية","زيادة التداخل"], correct:1, explanation:"تقليل التداخل يساعد في تقدير مساهمة كل فرد بدقة."},
{ q:"تعديل مفيد لطلاب ذوي صعوبات: ", choices:["نصوص طويلة","تجزئة المحتوى مع وسائل بصرية","زيادة الواجبات","لغة معقدة"], correct:1, explanation:"التجزئة والمرئيات تدعم الفهم."},
{ q:"كيف تنمي المدرسة مهارات المواطنة؟", choices:["منع النقاش","خدمة المجتمع ومناقشات مناظرات","التركيز على الاختبارات","الانفصال عن المجتمع"], correct:1, explanation:"خدمة المجتمع والنقاشات العملية تعزز المشاركة المدنية."},
{ q:"مؤشر التشتت هو:", choices:["المتوسط","الانحراف المعياري","عدد الأسئلة","عدد المصححين"], correct:1, explanation:"الانحراف المعياري يقيس تشتت الدرجات."},
{ q:"المعنى الدلالي مقابل الحرفي:", choices:["لا فرق","الحرفي ما تقوله الجملة، الدلالي يشمل الرموز والسياق","الدلالي قواعد فقط","الحرفي تفسير القارئ"], correct:1, explanation:"الدلالي يأخذ بعين الاعتبار الدلالات والسياقات غير الحرفية."},
{ q:"ناتج 9÷3 + 2×4 هو:", choices:["11","10","14","9"], correct:0, explanation:"9÷3=3 و2×4=8 → 3+8=11."},
{ q:"روبريك واضح يزيد من:", choices:["الفروق بين المصححين","ثبات التقدير","العشوائية","تغير الخطة"], correct:1, explanation:"الروبريك يقلل فروق الحكم ويزيد من الثبات."},
{ q:"إجراء أولي عند غياب متكرر:", choices:["العقاب","التواصل مع ولي الأمر لفهم الأسباب","إلغاء المشاركة","نقل الطالب"], correct:1, explanation:"التواصل يفكك الأسباب ويمكن تقديم دعم مناسب."},
{ q:"من ربط الشرط الكلاسيكي:", choices:["بياجيه","بافلوف","سكنر","فيجوتسكي"], correct:1, explanation:"بافلوف ربط بين المنبه والاستجابة في تجاربه الشهيرة."},
{ q:"لماذا نستخدم مشتتات مبنية على أخطاء شائعة؟", choices:["لتضليل الطلاب","لكشف الأخطاء المفهومية","لتطويل الاختبار","لتسهيله"], correct:1, explanation:"المشتتات تكشف ما إذا الطالب وقع في نفس الخطأ المفهومي."},
{ q:"ما أصغر عدد طرود يحقق شروط (باقي 4 عند القسمة على6) و(قابل للقسمة على5)؟", choices:["24","34","19","9"], correct:1, explanation:"العدد 34 يحقق الشروط في هذا السياق (تحقق من المشكلات المماثلة بالبحث عن الحل المشترك)."},
{ q:"التعلم القائم على الكفاءة يركز على:", choices:["مدة الحصة","إتقان المعايير بغض النظر عن الزمن","زيادة الأسئلة","التقييم فقط"], correct:1, explanation:"التركيز على قدرة المتعلم وإظهار الأداء وفق معايير محددة."},
{ q:"مثال استعارة بلاغية:", choices:["البحر هادئ","البحر صفحة بيضاء","البحر طويل","البحر واسع"], correct:1, explanation:"'صفحة بيضاء' استعارة تصور البحر بصورة بلاغية."},
{ q:"فائدة أخذ عينات من بنك الأسئلة:", choices:["زيادة العبء","تنويع البنود وتقليل الحفظ","تقليل التحليل","رفع صعوبة الاختبار"], correct:1, explanation:"العينات تساعد في تنويع الأسئلة وتقليل تسريبها."},
{ q:"أهم عامل لنجاح التدريس هو:", choices:["وجود كتب فقط","وضوح الأهداف وتوافق الأنشطة والتقويم","زيادة الحصص","استخدام الوسائل دون هدف"], correct:1, explanation:"التوافق بين الأهداف والأنشطة والتقويم هو المفتاح لفاعلية التدريس."}
]; // نهاية QUESTIONS

/* ========== بناء البطاقات والتفاعل ========== */
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
  const finalScore = document.getElementById('finalScore');
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
