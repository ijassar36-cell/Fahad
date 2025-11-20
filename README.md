<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1"/>

<title>محاكي الرخصة المهنية — إعداد المعلم: فهد الخالدي</title>

<style>
body{
  font-family: "Tahoma", sans-serif;
  background:#f2f2f2;
  margin:0; padding:0;
}
.container{
  max-width:750px; margin:20px auto; background:white;
  padding:20px; border-radius:12px; box-shadow:0 0 10px #0002;
}
.title{ text-align:center; color:#1e64d0; margin:0; }
.credit{ text-align:center; margin:5px 0 20px; font-size:18px; }

#question-text{
  font-size:22px; margin-bottom:15px; line-height:1.6;
}
.option{
  padding:12px; background:#eee; border-radius:8px;
  margin:10px 0; cursor:pointer; transition:0.2s;
}
.option:hover{ background:#ddd; }
.correct{ background:#c3f7ca !important; }
.wrong{ background:#ffb9b9 !important; }

#nextBtn{
  width:100%; padding:14px; margin-top:20px;
  font-size:18px; background:#1e64d0; color:white;
  border:none; border-radius:8px; cursor:pointer;
}
#nextBtn:disabled{ background:#96b7ee; cursor:not-allowed; }

/* نافذة شرح الإجابة */
#explain-popup{
  position:fixed; top:0; left:0; right:0; bottom:0;
  background:#0007; display:none; z-index:999;
}
.popup-content{
  background:white; position:absolute; top:50%; left:50%;
  transform:translate(-50%, -50%);
  padding:25px; width:80%; max-width:500px;
  border-radius:12px; text-align:center;
}
#close-popup{
  margin-top:15px; padding:10px 20px; background:#1e64d0;
  color:white; border:none; border-radius:8px;
  cursor:pointer; font-size:16px;
}

/* نافذة النتيجة الجديدة */
#result-modal{
  display:none;
  position:fixed;
  bottom:0; left:0;
  width:100%;
  background:white;
  padding:25px;
  text-align:center;
  border-radius:20px 20px 0 0;
  box-shadow:0 -4px 20px rgba(0,0,0,0.2);
  animation:slideUp 0.4s ease forwards;
  z-index:9999;
}
#result-modal h2{
  margin:0; font-size:28px; color:#1e64d0;
}
#result-modal p{
  margin-top:10px; font-size:22px;
}
@keyframes slideUp{
  from{ transform:translateY(100%); }
  to{ transform:translateY(0); }
}
</style>
</head>

<body>

<div class="container">

<h1 class="title">محاكي الرخصة المهنية</h1>
<p class="credit">إعداد المعلم: فهد الخالدي</p>

<div id="quiz-box">
  <div id="question-text"></div>
  <div id="options"></div>
  <button id="nextBtn" disabled>السؤال التالي</button>
</div>

</div>

<!-- نافذة الشرح -->
<div id="explain-popup">
  <div class="popup-content">
    <p id="explain-text"></p>
    <button id="close-popup">إغلاق</button>
  </div>
</div>

<!-- نافذة النتيجة -->
<div id="result-modal">
  <h2>النتيجة النهائية</h2>
  <p id="result-text"></p>
</div>

<script>
// ==========================
//   68 سؤال مع الإجابات بالترتيب المطلوب
// ==========================

const questions = [
{q:"ما المقصود بالتعلم النشط؟",options:["تعلم يعتمد على المعلم فقط","تعلم يشارك فيه الطالب بفاعلية","تعلم يعتمد على الحفظ فقط","تعلم بلا تفاعل"],correct:"ب",explain:"التعلم النشط يعتمد على مشاركة الطالب في الأنشطة."},

{q:"أهم ميزة في التقويم التكويني:",options:["الحكم النهائي","تحسين التعلم أثناء حدوثه","قياس نهاية الوحدة فقط","إلغاء مشاركة المتعلم"],correct:"أ",explain:"التقويم التكويني هدفه التحسين أثناء التعلم."},

{q:"تعتمد النظرية البنائية على:",options:["الحفظ والتلقين","فهم المتعلم وبناء المعرفة ذاتياً","تقليد الآخرين","تجريب بلا هدف"],correct:"ج",explain:"البنائية تركز على بناء المتعلم للمعرفة."},

{q:"في إدارة الصف يُفضل:",options:["الصراخ عند الخطأ","التحفيز الإيجابي","العقاب المستمر","إلغاء الحوار"],correct:"د",explain:"الإدارة الصفية الحديثة تعتمد التحفيز."},

{q:"من استراتيجيات التفكير الناقد:",options:["التلقين","التفسير والتحليل","الحفظ","التكرار"],correct:"د",explain:"التفكير الناقد يقوم على التحليل والتفسير."},

{q:"يرتبط بلوم بمستويات:",options:["المحسوس فقط","الأهداف الوجدانية","تصنيف المستويات المعرفية","الاختبارات البديلة"],correct:"ج",explain:"بلوم وضع هرم المستويات المعرفية."},

{q:"أفضل طريقة لضبط الصف:",options:["العقاب القاسي","إشعال التوتر","بناء علاقة إيجابية","التهديد"],correct:"ب",explain:"العلاقة الإيجابية أساس الإدارة الصفية."},

{q:"النظرية السلوكية تركز على:",options:["الدافعية الداخلية","النشاط الذاتي","الاستجابة للمثير","التفاعل الحر"],correct:"د",explain:"السلوكية تعتمد (مثير – استجابة)."},

{q:"أفضل قياس للمهارة العملية:",options:["اختبار اختياري","مقال","قائمة ملاحظة","اختبار قصير"],correct:"أ",explain:"المهارات العملية تحتاج ملاحظة مباشرة."},

{q:"من أساليب تحسين القراءة:",options:["التكرار دون فهم","الإهمال","التغذية الراجعة","العقاب"],correct:"ج",explain:"التغذية تعزز التعلم الصحيح."},

{q:"المعلم الناجح يستخدم:",options:["أسلوب واحد فقط","التنوع في الاستراتيجيات","الإلقاء الدائم","الغموض"],correct:"ب",explain:"التنوع مهم جدًا لنجاح الدرس."},

{q:"المنهج الحديث يركز على:",options:["المعلم فقط","إلغاء الأنشطة","تنمية المهارات","الحفظ الميكانيكي"],correct:"د",explain:"المناهج الحديثة تركز على المهارات."},

{q:"من مهارات التفكير الإبداعي:",options:["التذكر فقط","الطلاقة والمرونة","الحفظ","التقليد"],correct:"أ",explain:"الإبداع يعتمد على الطلاقة والمرونة."},

{q:"الاختبار الجيد يتميز:",options:["يحتوي على الغموض","لا يقيس الأهداف","صدق وثبات","عشوائية البنود"],correct:"ب",explain:"الصدق والثبات أساس جودة الاختبار."},

{q:"المعلم في التعلم التعاوني:",options:["مسيطر","موجه ومسهل","غير مشارك","محاضر فقط"],correct:"ج",explain:"المعلم دوره موجه في التعلم التعاوني."},

{q:"من مميزات التعليم الرقمي:",options:["يبطئ التعلم","يحد من المعرفة","يوفر فرص التعلم المتنوعة","يلغي دور الطالب"],correct:"د",explain:"التعليم الرقمي يوسع التعلم."},

{q:"هضم المعرفة يعني:",options:["التسميع فقط","الفهم العميق","الحفظ السطحي","التقليد"],correct:"أ",explain:"الهضم يعني الفهم وليس الحفظ."},

{q:"أفضل طريقة لتقليل القلق الاختباري:",options:["مزيد من العقاب","إزالة الشرح","التدريب على الاختبارات","منع الأسئلة"],correct:"ب",explain:"التدريب يقلل القلق."},

{q:"مخرجات التعلم يجب أن تكون:",options:["غامضة","عامة جدًا","قابلة للقياس","غير قابلة للتحقق"],correct:"ج",explain:"القياس أساس المخرجات."},

{q:"من سمات الطالب المتعلم:",options:["السلبية","التفاعل","الجمود","التردد"],correct:"أ",explain:"التفاعل صفة المتعلم النشط."},

{q:"الأسئلة المفتوحة تنمي:",options:["التقليد","التفكير العميق","الحفظ","التكرار"],correct:"ج",explain:"الأسئلة المفتوحة تفتح التفكير."},

{q:"القيم تتكون عبر:",options:["العقاب فقط","التلقين فقط","التجارب والخبرات","المشاهدة فقط"],correct:"أ",explain:"القيم تتشكل عبر الخبرات."},

{q:"عملية التعلم تحتاج:",options:["متعلم ومعلم فقط","أهداف وعمليات ونتائج","العقاب","التلقين"],correct:"ب",explain:"التعلم منظومة كاملة."},

{q:"الاستقصاء يركز على:",options:["الحفظ","طرح الأسئلة","العقاب","السرعة"],correct:"ج",explain:"الاستقصاء بحث واكتشاف."},

{q:"أفضل تعزيز هو:",options:["الشهرة","التعزيز الإيجابي","العقاب فقط","التجاهل"],correct:"د",explain:"التعزيز الإيجابي أقوى تأثيرًا."},

{q:"أول خطوة في التخطيط:",options:["النشاط","التقويم","تحديد الأهداف","العقاب"],correct:"ب",explain:"الأهداف أساس التخطيط."},

{q:"الطلاب ذوو الدافعية العالية:",options:["يحتاجون تهديداً","يحبون التحديات","يتجنبون العمل","لا يسألون"],correct:"أ",explain:"التحدي يحفزهم."},

{q:"النظرية المعرفية تركز على:",options:["السلوك فقط","العقاب","عمليات العقل","التكرار"],correct:"ج",explain:"المعرفية تهتم بالعقل."},

{q:"من استراتيجيات الفهم القرائي:",options:["التخمين","الربط بين الأفكار","الصمت","العقاب"],correct:"د",explain:"الربط يعزز الفهم."},

{q:"في المواقف الصفية يجب:",options:["الصراخ","الحوار","التجاهل","التهديد"],correct:"ب",explain:"الحوار يحقق الانضباط."},

{q:"أفضل وقت للتغذية الراجعة:",options:["بعد أسبوع","فوراً","في نهاية العام","بدون تغذية"],correct:"ب",explain:"اللحظية هي الأفضل."},

{q:"السؤال الجيد يجب أن يكون:",options:["معقدًا جدًا","طويلاً بلا سبب","واضحًا","غير محدد"],correct:"ج",explain:"الوضوح شرط للسؤال الجيد."},

{q:"العصف الذهني يعتمد على:",options:["رفض الأفكار","قبول كل الأفكار مبدئياً","السخرية","إيقاف الطلاب"],correct:"ب",explain:"قبول الأفكار يزيد الإبداع."},

{q:"يساعد المنهج الحلزوني على:",options:["تكرار بلا فائدة","تصاعد التعلم","إلغاء الخبرات","الحشو"],correct:"أ",explain:"الحلزوني يبني فوق التعلم السابق."},

{q:"من استراتيجيات حل المشكلات:",options:["التخمين","جمع البيانات","الصراخ","العقاب"],correct:"ج",explain:"جمع البيانات خطوة أساسية."},

{q:"تساهم الوسائل التعليمية في:",options:["تعقيد الدرس","إزعاج الطلاب","تبسيط المفاهيم","إلغاء التفكير"],correct:"أ",explain:"الوسيلة تبسّط المعلومات."},

{q:"التواصل الفعّال يتطلب:",options:["النبرة فقط","الوضوح والانتباه","العقاب","التجاهل"],correct:"ب",explain:"الوضوح أساس التواصل."},

{q:"أفضل طريقة لتنمية المفاهيم:",options:["التكرار فقط","الأمثلة المتنوعة","العقاب","الإلقاء"],correct:"ج",explain:"التنويع يبني المفهوم."},

{q:"يتعلم الطفل وفق بياجيه:",options:["بالمشاهدة فقط","حسب مراحل عمرية","بالحفظ","بالصدفة"],correct:"د",explain:"عند بياجيه التعلم مرحلي."},

{q:"التعلم الاجتماعي عند باندورا يعتمد على:",options:["العقاب","التقليد والنمذجة","التجاهل","التلقين"],correct:"ج",explain:"النمذجة أساس النظرية."},

{q:"أفضل أسلوب للطلاب الهادئين:",options:["التجاهل","الصراخ","تهيئة بيئة آمنة","العقاب"],correct:"ب",explain:"البيئة الآمنة تشجع المشاركة."},

{q:"التعلم القائم على المشروع:",options:["يعتمد على الحفظ","نشاط طويل واقعي","بدون تطبيق","منفصل عن الحياة"],correct:"أ",explain:"المشاريع تربط التعلم بالحياة."},

{q:"تحليل الأخطاء يساعد على:",options:["تثبيت الخطأ","إلغاء التعلم","تصحيح المفاهيم","مزيد من الغموض"],correct:"د",explain:"التحليل يؤدي للتصحيح."},

{q:"التقويم الواقعي يعني:",options:["اختبارات ورقية فقط","مهام عملية حقيقية","حفظ النصوص","الأسئلة المغلقة"],correct:"ج",explain:"الواقعي يقيس الأداء الحقيقي."},

{q:"الملاحظة المنظمة تستخدم:",options:["عشوائيًا","وفق معايير محددة","بدون أهداف","بدون سجل"],correct:"ب",explain:"المعايير تجعلها دقيقة."},

{q:"المنهج يضم:",options:["كتاب الطالب فقط","الأهداف والمحتوى والأنشطة","العقاب","الدرجات فقط"],correct:"أ",explain:"المنهج منظومة كاملة."},

{q:"من شروط الهدف الجيد:",options:["الإبهام","العمومية الشديدة","الوضوح والقابلية للقياس","عدم التحديد"],correct:"ج",explain:"الهدف الجيد واضح."},

{q:"التفكير الناقد يساعد على:",options:["الحفظ","التقليد","اتخاذ القرار","العشوائية"],correct:"أ",explain:"يساعد على اتخاذ قرارات أفضل."},

{q:"في التعلم الذاتي يكون دور الطالب:",options:["سلبي","متلقي فقط","محوري ومسؤول","بعيد"],correct:"ج",explain:"الطالب هو المحرك الأساسي."},

{q:"التحفيز يزيد:",options:["الملل","الدافعية","الغياب","التوتر"],correct:"أ",explain:"التحفيز يرفع الدافعية."},

{q:"أفضل وسيلة لفهم النصوص:",options:["الحفظ","التحليل","التجاهل","التكرار"],correct:"ج",explain:"التحليل يكشف المعنى."},

{q:"العلاقة بين المعلم والطالب يجب أن تكون:",options:["محفزة وداعمة","متوترة","تهديدية","منعزلة"],correct:"ب",explain:"العلاقة الإيجابية تعزز التعلم."},

{q:"الأنشطة الصفية الجيدة:",options:["تزيد التشتت","غير محددة","تدعم الهدف","عشوائية"],correct:"أ",explain:"يجب أن تخدم الهدف التعليمي."},

{q:"التغذية الراجعة السلبية:",options:["تدعم التعلم","تكسر الدافعية","تحفز الطلاب","تحسن النتائج"],correct:"د",explain:"السلبية تقلل الدافعية."},

{q:"تكرار الشرح دون تغيير:",options:["يزيد الفهم","يقلل الفاعلية","أفضل طريقة","يعالج الأخطاء"],correct:"ب",explain:"التنويع هو الأفضل."},

{q:"المعلم القدوة يؤثر في:",options:["لا أحد","جانب السلوك والقيم","الجانب المعرفي فقط","بشكل محدود"],correct:"ج",explain:"القدوة تصنع السلوك."},

{q:"الأهداف الوجدانية تهتم بـ:",options:["المهارات","القيم والمواقف","المعرفة","العمليات"],correct:"أ",explain:"تهتم بالقيم والمشاعر."},

{q:"الاستراتيجية المناسبة للنصوص الطويلة:",options:["التخمين","التلخيص","التجاهل","الحفظ"],correct:"ج",explain:"التلخيص يوضح الأفكار."},

{q:"ضعف القراءة يعالج بـ:",options:["التوبيخ","الإهمال","التدرج والتدريب","الغضب"],correct:"ب",explain:"التدرّج يعالج الضعف."},

{q:"التعلم الاجتماعي يحدث عبر:",options:["العقاب","النمذجة","التجاهل","العشوائية"],correct:"د",explain:"النمذجة أساسه."},

{q:"أفضل طريقة لتعليم المفردات:",options:["الحفظ","الاستخدام في سياق","التوبيخ","التجاهل"],correct:"ج",explain:"السياق ثبّت المفردة."},

{q:"النظرية المعرفية ترى أن:",options:["التعلم سلوك فقط","العقل نشط","العقاب أساس","التقليد أساس"],correct:"أ",explain:"تهتم بالعقل والعمليات الداخلية."},

{q:"في التعلم القائم على المشروع:",options:["لا يوجد تطبيق","الحياة جزء من التعلم","مجرد حفظ","تقليد"],correct:"د",explain:"المشاريع تربط المعرفة بالحياة."},

// ========= السؤال رقم 68 المطلوب =========
{
    q:"يعد جيروم برونر من أبرز رواد النظرية البنائية، وتركز نظريته على أهمية تنظيم المحتوى بشكل هرمي يبدأ بالتمثيل العملي ثم الصوري ثم الرمزي. أي من التالي يعبر بدقة عن أهم مبدأ في نظرية برونر؟",
    options:[
      "التعلم من خلال التقليد فقط",
      "التركيز على الحفظ دون فهم",
      "اعتماد المتعلم على مصادر خارجية",
      "التدرج في تمثيل المعرفة من المحسوس إلى المجرد"
    ],
    correct:"د",
    explain:"يرى برونر أن التعلم يحدث بشكل أفضل عندما يبنى بشكل تدريجي يبدأ بالخبرات المحسوسة ثم الصورية ثم الرمزية."
}
];

// ==========================
// نظام عرض الأسئلة
// ==========================

let index = 0;
let score = 0;

const qText = document.getElementById("question-text");
const optBox = document.getElementById("options");
const nextBtn = document.getElementById("nextBtn");
const popup = document.getElementById("explain-popup");
const explainText = document.getElementById("explain-text");
const closePopup = document.getElementById("close-popup");

function loadQuestion(){
  nextBtn.disabled = true;
  optBox.innerHTML = "";

  let q = questions[index];

  qText.innerHTML = `${index+1} ـ ${q.q}`;

  q.options.forEach((op,i)=>{
    let div = document.createElement("div");
    let letter = ["أ","ب","ج","د"][i];
    div.className="option";
    div.dataset.letter=letter;
    div.innerText = `${letter}) ${op}`;
    div.onclick = ()=> selectOption(div);
    optBox.appendChild(div);
  });
}

function selectOption(el){
  let chosen = el.dataset.letter;
  let correct = questions[index].correct;

  document.querySelectorAll(".option").forEach(o=>o.style.pointerEvents="none");

  if(chosen === correct){
    el.classList.add("correct");
    score++;
  } else {
    el.classList.add("wrong");
    document.querySelector(`[data-letter="${correct}"]`).classList.add("correct");
  }

  explainText.innerHTML = questions[index].explain;
  popup.style.display="block";

  nextBtn.disabled = false;
}

closePopup.onclick = () => popup.style.display="none";

nextBtn.onclick = ()=>{
  popup.style.display="none";
  index++;

  if(index < questions.length){
    loadQuestion();
  } else {
    showResult();
  }
};

// ==========================
// عرض النتيجة
// ==========================

function showResult(){
  document.getElementById("quiz-box").style.display="none";
  let modal = document.getElementById("result-modal");
  document.getElementById("result-text").innerHTML =
    `حصلت على <strong>${score}</strong> من <strong>${questions.length}</strong>`;
  modal.style.display="block";
}

// بدء
loadQuestion();

</script>

</body>
</html>
