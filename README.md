# ETEA12-online-test-for-practice

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>ETEA Practice Test</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
<style>
body{font-family:'Poppins',sans-serif;background:linear-gradient(135deg,#ff9a9e,#fecfef);margin:0;color:#333;overflow-x:hidden}
.container{max-width:900px;margin:20px auto;background:#fff;padding:30px;border-radius:20px;box-shadow:0 10px 30px rgba(0,0,0,.2);animation:fadeIn .5s ease-in}
@keyframes fadeIn{from{opacity:0;transform:translateY(20px)}to{opacity:1;transform:translateY(0)}}
h2{text-align:center;color:#2c3e50;margin-bottom:20px}
.center{text-align:center}
input{width:100%;padding:12px;margin-bottom:15px;font-size:16px;border:2px solid #ddd;border-radius:10px;transition:border-color .3s}
input:focus{border-color:#3498db;outline:none}
button{padding:12px 20px;border:none;border-radius:10px;font-size:16px;font-weight:bold;cursor:pointer;transition:all .3s;margin:5px}
#startBtn{background:linear-gradient(45deg,#3498db,#2980b9);color:white;width:100%;animation:bounce 2s infinite}
@keyframes bounce{0%,20%,50%,80%,100%{transform:translateY(0)}40%{transform:translateY(-10px)}60%{transform:translateY(-5px)}}
#timer{background:linear-gradient(45deg,#e74c3c,#c0392b);color:white;padding:15px;text-align:center;border-radius:10px;font-weight:bold;position:relative}
#timerBar{width:100%;height:5px;background:#ddd;border-radius:5px;margin-top:10px}
#timerBarFill{height:100%;background:#27ae60;border-radius:5px;transition:width 1s linear}
.warning{color:#e74c3c;font-weight:bold;text-align:center;margin:15px 0;font-size:18px;animation:blink 1s infinite}
@keyframes blink{0%,50%{opacity:1}51%,100%{opacity:0}}
.option{border:2px solid #ecf0f1;padding:15px;border-radius:10px;margin-bottom:12px;cursor:pointer;transition:all .3s;background:#f9f9f9}
.option:hover{border-color:#3498db;background:#ecf0f1;transform:scale(1.02)}
.option.selected{border-color:#3498db;background:#d4edda}
.option.correct{background:#3498db;color:white;border-color:#3498db}
.option.wrong{background:#e74c3c;color:white;border-color:#e74c3c;border:3px solid red}
.buttons{display:flex;justify-content:space-between;margin-top:25px;flex-wrap:wrap}
#prevBtn{background:#7f8c8d;color:white}
#skipBtn{background:#f39c12;color:white}
#nextBtn{background:#2980b9;color:white}
#submitBtn{background:#27ae60;color:white;width:100%;margin-top:20px;display:none}
.progress{margin-bottom:20px}
.progress-bar{width:100%;height:10px;background:#ddd;border-radius:5px}
.progress-fill{height:100%;background:linear-gradient(45deg,#3498db,#2980b9);border-radius:5px;transition:width .5s}
.footer{text-align:center;font-size:14px;margin-top:30px;color:#777}
.dark-mode{background:#2c3e50;color:#ecf0f1}
.dark-mode .container{background:#34495e}
.review-question{border-bottom:1px solid #ddd;padding:15px 0}
.review-option.correct{background:#3498db;color:white;border:2px solid #3498db;padding:8px;margin:3px;border-radius:8px}
.review-option.wrong{background:#e74c3c;color:white;border:2px solid #e74c3c;padding:8px;margin:3px;border-radius:8px}
.review-explanation{font-size:14px;color:#555;margin-top:8px}
@media (max-width:600px){.buttons{flex-direction:column}button{width:100%}}
</style>
</head>
<body>
<!-- LOGIN -->
<div class="container center" id="loginDiv">
<h2><i class="fas fa-graduation-cap"></i> ETEA Practice Test</h2>
<input id="studentName" placeholder="Student Name">
<input id="rollNo" placeholder="Roll Number">
<button id="startBtn" onclick="startCountdown()">Start Test</button>
<div id="countdown" style="font-size:30px;color:#e74c3c;margin-top:15px"></div>
<label><input type="checkbox" id="randomize"> Randomize Questions</label>
<label><input type="checkbox" id="darkMode" onchange="toggleDarkMode()"> Dark Mode</label>
</div>

<!-- QUIZ -->
<div class="container" id="quizDiv" style="display:none">
<h2 class="center"><i class="fas fa-question-circle"></i> ETEA Practice Test</h2>
<div id="timer">Time Left: 40:00</div>
<div id="timerBar"><div id="timerBarFill"></div></div>
<div class="warning" id="warn20" style="display:none"><i class="fas fa-exclamation-triangle"></i> Only 20 minutes remaining!</div>
<div id="attemptInfo" class="center"></div>
<div class="progress">
<div class="progress-bar"><div class="progress-fill" id="progressFill"></div></div>
</div>
<div id="questionBox"></div>
<div id="optionsBox"></div>
<div class="buttons">
<button id="prevBtn" onclick="prevQ()">Previous</button>
<button id="skipBtn" onclick="skipQ()">Skip</button>
<button id="nextBtn" onclick="nextQ()" disabled>Next</button>
</div>
<button id="submitBtn" onclick="confirmSubmit()">Submit Test</button>
</div>

<!-- RESULT -->
<div class="container center" id="resultDiv" style="display:none">
<h2 id="resStatus"></h2>
<p id="resScore"></p>
<p id="resTime"></p>
<button onclick="showReview()" style="background:#2980b9;color:white;width:100%">Review Test</button>
<button onclick="location.reload()">Restart</button>
</div>

<!-- REVIEW -->
<div class="container" id="reviewDiv" style="display:none">
<h2 class="center">Review Your Test</h2>
<div id="reviewContent"></div>
<button onclick="backToResult()" style="width:100%;background:#7f8c8d;color:white">Back to Result</button>
</div>

<div class="footer">Created by <b>Muhammad Faizan Nawab</b></div>

<script>
// ==========================
// 100 MCQs (example 4 added; expand as needed)
// ==========================

const questions = [
    

{ q: "1. Which of the following correctly represents the set of integers?", 
  o: ["{1, 2, 3, 4}", "{0, 1, 2, 3}", "{-3, -2, -1, 0, 1, 2, 3}", "{1/2, 2/3, 3/4}"], 
  c: 2 
},

{ q: "2. Which number is neither positive nor negative?", 
  o: ["-1", "1", "0", "10"], 
  c: 2 
},

{ q: "3. The additive identity for integers is:", 
  o: ["1", "-1", "0", "None"], 
  c: 2 
},

{ q: "4. Which integer has the greatest absolute value?", 
  o: ["-5", "3", "-7", "6"], 
  c: 2 
},

{ q: "5. | -12 | − | 7 | equals:", 
  o: ["5", "-5", "19", "-19"], 
  c: 1 
},

{ q: "6. Which law states that a + b = b + a for all integers a and b?", 
  o: ["Closure law", "Associative law", "Commutative law", "Distributive law"], 
  c: 2 
},

{ q: "7. Which operation on integers always satisfies the closure property?", 
  o: ["Division", "Subtraction", "Addition", "Both B and C"], 
  c: 3 
},

{ q: "8. (-6) + 6 is an example of:", 
  o: ["Multiplicative identity", "Additive inverse", "Commutative law", "Associative law"], 
  c: 1 
},

{ q: "9. The additive inverse of -15 is:", 
  o: ["-15", "15", "0", "1"], 
  c: 1 
},

{ q: "10. Which integer has no multiplicative inverse in integers?", 
  o: ["1", "-1", "0", "2"], 
  c: 2 
},

{ q: "11. The multiplicative identity of integers is:", 
  o: ["0", "-1", "1", "2"], 
  c: 2 
},

{ q: "12. Which expression shows the associative law of addition?", 
  o: ["a + b = b + a", "(a + b) + c = a + (b + c)", "a(b + c) = ab + ac", "a + 0 = a"], 
  c: 1 
},

{ q: "13. Which law is used in: 4 × (6 + 3) = (4 × 6) + (4 × 3)?", 
  o: ["Commutative law", "Associative law", "Closure law", "Distributive law"], 
  c: 3 
},

{ q: "14. According to BODMAS, which operation is performed first?", 
  o: ["Division", "Addition", "Brackets", "Multiplication"], 
  c: 2 
},

{ q: "15. Simplify: -8 + 3 × 4", 
  o: ["4", "-20", "12", "-4"], 
  c: 0 
},

{ q: "16. Simplify: (-6)² − | -10 |", 
  o: ["26", "36", "46", "16"], 
  c: 0 
},

{ q: "17. Which of the following does NOT obey the commutative law?", 
  o: ["Addition", "Multiplication", "Subtraction", "Both A and B"], 
  c: 2 
},

{ q: "18. Simplify using BODMAS: 12 ÷ (3 × 2) + 4", 
  o: ["6", "4", "2", "8"], 
  c: 1 
},

{ q: "19. If a = -3 and b = 5, then a + b − |a| equals:", 
  o: ["-1", "2", "-6", "5"], 
  c: 2 
},

{ q: "20. Which statement is TRUE?", 
  o: ["Every integer has a multiplicative inverse", "Zero has an additive inverse", "Zero has a multiplicative inverse", "Negative integers are not real numbers"], 
  c: 1 
},

{ q: "21. Simplify: (-4) × (-3) − 6", 
  o: ["-18", "6", "18", "12"], 
  c: 1 
},

{ q: "22. Which real-life situation best represents negative integers?", 
  o: ["Number of books", "Height of a tree", "Temperature below zero", "Distance traveled"], 
  c: 2 
},

{ q: "23. Simplify: | -5 + 2 | × 3", 
  o: ["9", "-9", "3", "-3"], 
  c: 0 
},

{ q: "24. Which law explains that the sum of two integers is always an integer?", 
  o: ["Associative law", "Closure law", "Distributive law", "Identity law"], 
  c: 1 
},

{ q: "25. Simplify: 7 − [3 + (−5)]", 
  o: ["-1", "9", "5", "-5"], 
  c: 2 
},

{ q: "26. If the temperature was -6°C and increased by 9°C, the final temperature is:", 
  o: ["-15°C", "-3°C", "3°C", "15°C"], 
  c: 2 
},

{ q: "27. Which of the following equals zero?", 
  o: ["(-7) + 7", "(-3) × 3", "| -5 |", "6 − (-6)"], 
  c: 0 
},

{ q: "28. Simplify: (-2)³ + | -4 |", 
  o: ["-4", "-12", "0", "-8"], 
  c: 2 
},

{ q: "29. Which property is used when 9 × 1 = 9?", 
  o: ["Closure", "Additive identity", "Multiplicative identity", "Commutative"], 
  c: 2 
},

{ q: "30. A shopkeeper loses Rs. 50 daily for 4 days. The total loss in integers is:", 
  o: ["200", "-200", "50", "-50"], 
  c: 1 
},

{ q: "31. The structural and functional hierarchy in living organisms is correctly arranged as:", 
  o: ["Cell → Tissue → Organ → Organ system → Organism", "Tissue → Cell → Organ → Organism → System", "Cell → Organ → Tissue → Organism → System", "Organ → Tissue → Cell → Organism → System"], 
  c: 0 
},

{ q: "32. Cellular organization primarily refers to:", 
  o: ["Arrangement of cells in tissues", "Chemical composition of cells", "Levels of biological organization", "Size of different cells"], 
  c: 2 
},

{ q: "33. Which statement best defines a cell?", 
  o: ["A group of tissues performing a function", "The smallest unit capable of independent life processes", "A microscopic organ", "A visible structure in organisms"], 
  c: 1 
},

{ q: "34. Which structure is present in plant cells but absent in animal cells?", 
  o: ["Nucleus", "Mitochondria", "Cell wall", "Ribosomes"], 
  c: 2 
},

{ q: "35. The organelle responsible for energy production in both plant and animal cells is:", 
  o: ["Chloroplast", "Ribosome", "Nucleus", "Mitochondrion"], 
  c: 3 
},

{ q: "36. Vacuoles in plant cells are generally:", 
  o: ["Small and numerous", "Absent", "Large and central", "Identical to animal cells"], 
  c: 2 
},

{ q: "37. Which cellular component controls all metabolic activities of the cell?", 
  o: ["Cytoplasm", "Cell membrane", "Nucleus", "Vacuole"], 
  c: 2 
},

{ q: "38. Tissues are best described as:", 
  o: ["Groups of cells with similar structure and function", "Groups of organs working together", "Independent living units", "Chemical substances of cells"], 
  c: 0 
},

{ q: "39. Which of the following is an example of a plant tissue?", 
  o: ["Muscle tissue", "Nervous tissue", "Xylem", "Blood"], 
  c: 2 
},

{ q: "40. Animal tissues differ from plant tissues mainly because animal tissues:", 
  o: ["Perform photosynthesis", "Have cell walls", "Are specialized for movement and coordination", "Contain chloroplasts"], 
  c: 2 
},

{ q: "41. An organ is defined as:", 
  o: ["A single specialized cell", "A group of similar tissues", "A group of different tissues performing a specific function", "A complete organism"], 
  c: 2 
},

{ q: "42. Which of the following is an example of an organ in humans?", 
  o: ["Neuron", "Muscle tissue", "Heart", "Blood"], 
  c: 2 
},

{ q: "43. The main function of the leaf as an organ in plants is:", 
  o: ["Absorption", "Transportation", "Photosynthesis", "Respiration"], 
  c: 2 
},

{ q: "44. An organ system is formed when:", 
  o: ["Similar cells group together", "Different organs work in coordination", "Tissues perform independent functions", "Cells divide repeatedly"], 
  c: 1 
},

{ q: "45. Which pair correctly represents an organ system and its function?", 
  o: ["Nervous system – Digestion", "Digestive system – Breathing", "Circulatory system – Transport of materials", "Skeletal system – Photosynthesis"], 
  c: 2 
},

{ q: "46. The highest level of biological organization is:", 
  o: ["Organ", "Organ system", "Tissue", "Organism"], 
  c: 3 
},

{ q: "47. Which statement about an organism is CORRECT?", 
  o: ["It is a group of organs only", "It can perform all life processes independently", "It consists of one tissue only", "It cannot respond to stimuli"], 
  c: 1 
},

{ q: "48. A unicellular organism differs from a multicellular organism because it:", 
  o: ["Lacks a nucleus", "Is made of only one cell performing all functions", "Cannot reproduce", "Has tissues and organs"], 
  c: 1 
},

{ q: "49. Which level of organization shows maximum specialization?", 
  o: ["Cell", "Tissue", "Organ system", "Organism"], 
  c: 2 
},

{ q: "50. Which structure allows selective movement of substances into and out of the cell?", 
  o: ["Cell wall", "Cytoplasm", "Cell membrane", "Nucleus"], 
  c: 2 
},

{ q: "51. In plant cells, chloroplasts are primarily involved in:", 
  o: ["Respiration", "Excretion", "Photosynthesis", "Protein synthesis"], 
  c: 2 
},

{ q: "52. Which feature is common to both plant and animal cells?", 
  o: ["Cell wall", "Chloroplast", "Large central vacuole", "Cell membrane"], 
  c: 3 
},

{ q: "53. A tissue composed of elongated cells capable of contraction is:", 
  o: ["Epithelial tissue", "Muscle tissue", "Connective tissue", "Nervous tissue"], 
  c: 1 
},

{ q: "54. Which organ system is responsible for coordination and control in animals?", 
  o: ["Digestive system", "Circulatory system", "Nervous system", "Respiratory system"], 
  c: 2 
},

{ q: "55. The term \"cellular organization\" emphasizes:", 
  o: ["Chemical reactions in cells", "Orderly arrangement from cells to organism", "Shape of the cell", "Division of the nucleus"], 
  c: 1 
},

{ q: "56. Which sequence correctly shows increasing complexity?", 
  o: ["Tissue → Cell → Organ", "Organ → Tissue → Cell", "Cell → Tissue → Organ", "Organism → Organ → Cell"], 
  c: 2 
},

{ q: "57. Which of the following is NOT an organ system?", 
  o: ["Digestive system", "Nervous system", "Muscle tissue", "Respiratory system"], 
  c: 2 
},

{ q: "58. The basic functional unit of life is:", 
  o: ["Tissue", "Organ", "Cell", "Organ system"], 
  c: 2 
},

{ q: "59. Which level of organization enables division of labor in multicellular organisms?", 
  o: ["Cell only", "Tissue and organ formation", "Unicellular level", "Molecular level"], 
  c: 1 
},

{ q: "60. The coordinated functioning of all organ systems results in:", 
  o: ["Cellular respiration", "Tissue formation", "Survival of the organism", "Cell division"], 
  c: 2 
},

{ q: "61. Which of the following best defines a noun?", 
  o: ["A word that shows action", "A word that describes a noun", "A word that names a person, place, thing, or idea", "A word used instead of a noun"], 
  c: 2 
},

{ q: "62. The word \"honesty\" is an example of:", 
  o: ["Common noun", "Proper noun", "Collective noun", "Abstract noun"], 
  c: 3 
},

{ q: "63. Which of the following is a proper noun?", 
  o: ["city", "teacher", "Pakistan", "country"], 
  c: 2 
},

{ q: "64. Identify the collective noun in the sentence: \"A herd of cattle was grazing in the field.\"", 
  o: ["cattle", "herd", "field", "grazing"], 
  c: 1 
},

{ q: "65. Which noun names something that can be seen or touched?", 
  o: ["Abstract noun", "Proper noun", "Material noun", "Concrete noun"], 
  c: 3 
},

{ q: "66. The noun \"gold\" is classified as a:", 
  o: ["Common noun", "Material noun", "Collective noun", "Abstract noun"], 
  c: 1 
},

{ q: "67. Which sentence contains an abstract noun?", 
  o: ["The boy kicked the ball.", "She showed great courage.", "The dog is barking.", "The book is on the table."], 
  c: 1 
},

{ q: "68. Choose the correct article: \"____ honest man is respected by everyone.\"", 
  o: ["A", "An", "The", "No article"], 
  c: 1 
},

{ q: "69. Which article is used before words that begin with a vowel sound?", 
  o: ["A", "An", "The", "No article"], 
  c: 1 
},

{ q: "70. Identify the correct use of the definite article:", 
  o: ["I saw a moon last night.", "She bought an umbrella.", "The sun rises in the east.", "He is a honest boy."], 
  c: 2 
},

{ q: "71. Which noun is both common and concrete?", 
  o: ["happiness", "bravery", "book", "honesty"], 
  c: 2 
},

{ q: "72. The word \"team\" is an example of:", 
  o: ["Proper noun", "Abstract noun", "Collective noun", "Material noun"], 
  c: 2 
},

{ q: "73. Choose the sentence with a material noun:", 
  o: ["The chair is broken.", "The ring is made of silver.", "The child is happy.", "The birds are flying."], 
  c: 1 
},

{ q: "74. Which option correctly completes the sentence? \"He is ____ European citizen.\"", 
  o: ["a", "an", "the", "no article"], 
  c: 0 
},

{ q: "75. The noun \"crowd\" refers to:", 
  o: ["One person", "A place", "A group taken as one", "A quality"], 
  c: 2 
},

{ q: "76. Which sentence shows incorrect use of articles?", 
  o: ["She has an orange.", "He is the best player.", "I saw an one-eyed man.", "The Earth moves around the Sun."], 
  c: 2 
},

{ q: "77. Which noun in the sentence is abstract? \"Kindness wins hearts.\"", 
  o: ["Kindness", "wins", "hearts", "none"], 
  c: 0 
},

{ q: "78. Choose the correct article: \"He wants to be ____ engineer.\"", 
  o: ["a", "an", "the", "no article"], 
  c: 1 
},

{ q: "79. Which of the following is NOT a kind of noun?", 
  o: ["Proper noun", "Abstract noun", "Adjective noun", "Collective noun"], 
  c: 2 
},

{ q: "80. Identify the noun and its type: \"Freedom is priceless.\"", 
  o: ["Freedom – Proper noun", "Freedom – Common noun", "Freedom – Abstract noun", "Freedom – Material noun"], 
  c: 2 
},

{ q: "81. وہ اسم جو کسی خاص شخص، جگہ یا چیز کا نام بتائے کہلاتا ہے:", 
  o: ["اسمِ ذات", "اسمِ عام", "اسمِ علم", "اسمِ کیفیت"], 
  c: 2 
},

{ q: "82. لفظ \"سچائی\" نحوی اعتبار سے کس قسم کا اسم ہے؟", 
  o: ["اسمِ ذات", "اسمِ آلہ", "اسمِ معنی", "اسمِ جمع"], 
  c: 2 
},

{ q: "83. درج ذیل میں کون سا لفظ اسمِ آلہ کی مثال ہے؟", 
  o: ["خوشی", "قلم", "بچپن", "بہادری"], 
  c: 1 
},

{ q: "84. وہ جملہ جس میں ایک ہی خیال مکمل ہو کہلاتا ہے:", 
  o: ["مرکب جملہ", "مفرد جملہ", "ناقص جملہ", "معترض جملہ"], 
  c: 1 
},

{ q: "85. \"اور، لیکن، کیونکہ\" اردو گرامر میں کیا کہلاتے ہیں؟", 
  o: ["حروفِ جار", "حروفِ عطف", "حروفِ ندا", "حروفِ شرط"], 
  c: 1 
},

{ q: "86. محاورہ \"آنکھیں دکھانا\" کا درست مفہوم کیا ہے؟", 
  o: ["محبت کرنا", "ناراض ہونا", "ڈانٹ ڈپٹ کرنا", "مدد کرنا"], 
  c: 2 
},

{ q: "87. درج ذیل میں کون سا جملہ اسمیہ ہے؟", 
  o: ["علی اسکول جاتا ہے", "بچے کھیل رہے ہیں", "علم طاقت ہے", "وہ کتاب پڑھتا ہے"], 
  c: 2 
},

{ q: "88. اسلامی اصطلاح میں وہ جنگ جس میں نبی کریم ﷺ نے خود شرکت فرمائی ہو کہلاتی ہے:", 
  o: ["سریہ", "معرکہ", "غزوہ", "جہاد"], 
  c: 2 
},

{ q: "89. غزوۂ بدر ہجری کے کس سال پیش آیا؟", 
  o: ["1 ہجری", "2 ہجری", "3 ہجری", "5 ہجری"], 
  c: 1 
},

{ q: "90. غزوۂ اُحد کا سب سے بڑا اخلاقی سبق کیا ہے؟", 
  o: ["تعداد کی اہمیت", "مالِ غنیمت", "اطاعتِ امیر", "ہجرت"], 
  c: 2 
},

{ q: "91. غزوۂ خندق میں خندق کھودنے کا مشورہ کس صحابیؓ نے دیا؟", 
  o: ["حضرت ابو بکرؓ", "حضرت عمرؓ", "حضرت علیؓ", "حضرت سلمان فارسیؓ"], 
  c: 3 
},

{ q: "92. وہ عبادت جو دن میں پانچ وقت فرض ہے:", 
  o: ["روزہ", "زکوٰۃ", "نماز", "حج"], 
  c: 2 
},

{ q: "93. اسلام میں نیت کا تعلق کس عبادت سے ہے؟", 
  o: ["صرف روزہ", "صرف نماز", "تمام عبادات", "صرف زکوٰۃ"], 
  c: 2 
},

{ q: "94. \"اللہ ایک ہے\" یہ عقیدہ کہلاتا ہے:", 
  o: ["رسالت", "توحید", "آخرت", "شریعت"], 
  c: 1 
},

{ q: "95. درج ذیل میں کون سا فعلِ ماضی ہے؟", 
  o: ["جائے گا", "جا رہا ہے", "گیا", "جاتا ہے"], 
  c: 2 
},

{ q: "96. وہ ضمیر جو بولنے والے کے لیے استعمال ہو:", 
  o: ["ضمیرِ غائب", "ضمیرِ مخاطب", "ضمیرِ متکلم", "ضمیرِ موصولہ"], 
  c: 2 
},

{ q: "97. قرآنِ مجید کس زبان میں نازل ہوا؟", 
  o: ["فارسی", "عبرانی", "عربی", "اردو"], 
  c: 2 
},

{ q: "98. وہ جملہ جو سوالیہ انداز رکھتا ہو کہلاتا ہے:", 
  o: ["خبریہ", "امریہ", "استفہامیہ", "فجائیہ"], 
  c: 2 
},

{ q: "99. اسلام میں سب سے پہلا فرض کیا گیا عمل ہے:", 
  o: ["روزہ", "زکوٰۃ", "نماز", "کلمہ"], 
  c: 3 
},

{ q: "100. درج ذیل میں کون سا حرفِ ندا ہے؟", 
  o: ["کیونکہ", "اور", "اے!", "لیکن"], 
  c: 2 
},




];



// ==========================
// Quiz Logic
// ==========================
let answers=new Array(questions.length).fill(null);
let attempted=0,index=0,time=4000,startTime=Date.now(),timerInt;

function playSound(f,d){
const a=new (window.AudioContext||window.webkitAudioContext)();
const o=a.createOscillator();const g=a.createGain();
o.connect(g);g.connect(a.destination);o.frequency.value=f;o.type='sine';g.gain.setValueAtTime(0.3,a.currentTime);o.start(a.currentTime);o.stop(a.currentTime+d);
}

function speakText(t){
if('speechSynthesis'in window){let u=new SpeechSynthesisUtterance(t);u.lang='en-US';u.rate=.8;u.pitch=1;speechSynthesis.speak(u);}
}

function startCountdown(){
if(!studentName.value||!rollNo.value){alert("Please fill in all fields!");return;}
let c=5;countdown.innerText=c;
let i=setInterval(()=>{c--;countdown.innerText=c;if(c===0){clearInterval(i);loginDiv.style.display="none";quizDiv.style.display="block";loadQ();startTimer()}},1000);
}

function loadQ(){
attemptInfo.innerText=`Attempted: ${answers.filter(a=>a!==null).length}/${questions.length}`;
progressFill.style.width=`${(answers.filter(a=>a!==null).length/questions.length)*100}%`;

let q=questions[index];
questionBox.innerHTML=`<h3>${index+1}. ${q.q}</h3>`;
optionsBox.innerHTML="";
q.o.forEach((op,i)=>{
let d=document.createElement("div");
d.className="option";
d.innerText=op;
if(answers[index]===i)d.classList.add("selected");
d.onclick=()=>{
if(answers[index]===null)attempted++;
answers[index]=i;
if(i===q.c){d.classList.add("correct");playSound(900,.2);}
else{d.classList.add("wrong");playSound(500,.2);}
nextBtn.disabled=false;
};
optionsBox.appendChild(d);
});
prevBtn.disabled=index===0;
nextBtn.textContent=index===questions.length-1?"Finish":"Next";
submitBtn.style.display=index===questions.length-1?"block":"none";
}

function nextQ(){if(index<questions.length-1)index++;nextBtn.disabled=true;loadQ();}
function prevQ(){if(index>0)index--;nextBtn.disabled=answers[index]===null;loadQ();}
function skipQ(){index=(index+1)%questions.length;nextBtn.disabled=true;loadQ();}

function startTimer(){
timerInt=setInterval(()=>{
let m=Math.floor(time/60),s=time%60;
timer.innerText=`Time Left: ${m}:${s<10?'0':''}${s}`;
timerBarFill.style.width=`${(time/2400)*100}%`;
if(time===1200){warn20.style.display="block";playSound(600,1);}
if(time--<=0){clearInterval(timerInt);submitTest();}
},1000);
}

function confirmSubmit(){if(confirm("Submit Test?"))submitTest();}

function submitTest(){
clearInterval(timerInt);
quizDiv.style.display="none";resultDiv.style.display="block";
let score=answers.filter((a,i)=>a===questions[i].c).length;
let pct=Math.round(score/questions.length*100);
let tTaken=Math.floor((Date.now()-startTime)/60000);
if(pct>=50){resStatus.innerHTML='🎉 PASSED! 🎉';speakText("Congratulations! You passed the test. Great job!");}
else{resStatus.innerHTML='❌ FAILED ❌';speakText("Sorry! You failed. Try again to improve!");}
resScore.innerText=`${studentName.value} (${rollNo.value}) scored ${score}/${questions.length} (${pct}%)`;
resTime.innerText=`Time Taken: ${tTaken} minutes`;
}

function showReview(){
resultDiv.style.display="none";reviewDiv.style.display="block";
reviewContent.innerHTML="";
questions.forEach((q,i)=>{
let div=document.createElement("div");div.className="review-question";
let opts="";
q.o.forEach((op,idx)=>{
let cls="review-option";
if(idx===q.c)cls+=" correct";
else if(answers[i]===idx)cls+=" wrong";
opts+=`<div class="${cls}">${op}</div>`;
});
div.innerHTML=`<h4>${i+1}. ${q.q}</h4>${opts}<div class="review-explanation"><b>Explanation:</b> ${q.exp}</div>`;
reviewContent.appendChild(div);
});
}

function backToResult(){reviewDiv.style.display="none";resultDiv.style.display="block";}
function toggleDarkMode(){document.body.classList.toggle("dark-mode");}
</script>
</body>
</html>
