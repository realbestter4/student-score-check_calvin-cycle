<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8" />
  <title>ระบบตรวจสอบผลสอบ</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    body { font-family: system-ui, sans-serif; background:#f5f7fa; }
    .card { max-width:420px; margin:80px auto; background:#fff; padding:24px; border-radius:16px; box-shadow:0 10px 25px rgba(0,0,0,.1); }
    h1 { text-align:center; margin-bottom:16px; }
    input { width:100%; padding:12px; font-size:16px; border-radius:10px; border:1px solid #ccc; }
    button { width:100%; margin-top:12px; padding:12px; font-size:16px; border-radius:10px; border:none; background:#2563eb; color:#fff; cursor:pointer; }
    .result { margin-top:20px; text-align:center; font-size:18px; }
    .pass { color:green; font-weight:600; }
    .fail { color:red; font-weight:600; }
  </style>
</head>
<body>
  <div class="card">
    <h1>ตรวจสอบผลสอบ</h1>
    <input id="studentId" placeholder="กรอกเลขประจำตัวนักเรียน" />
    <button onclick="checkResult()">ตรวจสอบ</button>
    <div id="result" class="result"></div>
  </div>

  <script>
    // 🔧 ข้อมูลตัวอย่าง (ครู/แอดมินแก้ตรงนี้)
    const students = {
  "51305": { name: "นางสาวชาลิสา แท่นมณี", status: "ผ่าน", score: 3 },
  "51429": { name: "นายวสวัต ดั่นธนสาร", status: "ผ่าน", score: 5 },
  "51581": { name: "นางสาวปภาวรินทร์ ชุมภูราช", status: "ผ่าน", score: 4 },
  "51769": { name: "นายจิรวัฒน์ เส้งลั่น", status: "ผ่าน", score: 5 },
  "51770": { name: "นายณฐกร แก้วจันทร์", status: "ผ่าน", score: 5 },
  "51775": { name: "นายพงศ์พิพัฒน์ ภิรมย์", status: "ผ่าน", score: 4 },
  "51777": { name: "นายวรพล หนูกล่ำ", status: "ผ่าน", score: 4 },
  "51778": { name: "นายอภิสร เพอแสละ", status: "ผ่าน", score: 5 },
  "51780": { name: "นางสาวชญานี ทองรักษ์", status: "ผ่าน", score: 5 },
  "51782": { name: "นางสาวณปภัสร์ ชูชื่น", status: "ผ่าน", score: 3 },
  "51783": { name: "นางสาวณภัทรสร โชโต", status: "ผ่าน", score: 5 },
  "51785": { name: "นางสาวทรรศนีย์ ครูนิลอาจ", status: "ผ่าน", score: 5 },
  "51788": { name: "นางสาวนภัสสร ประยูร", status: "ผ่าน", score: 4 },
  "51790": { name: "นางสาวนันทวัน ศรีไหม", status: "ผ่าน", score: 5 },
  "51791": { name: "นางสาวปุณยาพร พูลทัศน์", status: "ผ่าน", score: 5 },
  "51792": { name: "นางสาวภคนันท์ ชูแก้ว", status: "ผ่าน", score: 4 },
  "51793": { name: "นางสาวภิญญา คงสีปาน", status: "ผ่าน", score: 4 },
  "51796": { name: "นางสาวอนัญญา จู่เซ่งเจริญ", status: "ผ่าน", score: 5 },
  "51818": { name: "นางสาวมนัสนันท์ จิตประพันธ์", status: "ผ่าน", score: 5 },
  "51824": { name: "นางสาวอาธารี ยี่โชติช่วง", status: "ผ่าน", score: 4 },
  "54214": { name: "นายทรงพล จิระนิล", status: "ไม่ผ่าน", score: 1 },
  "54216": { name: "นางสาวกวินนธิดา เจริญพักตร์", status: "ผ่าน", score: 3 },
  "54218": { name: "นางสาวณิชกานต์ เพ็ชรสงคราม", status: "ผ่าน", score: 3 },
  "54219": { name: "นางสาวปาริชาต อินทร์เอม", status: "ผ่าน", score: 5 },
  "54220": { name: "นางสาวเปี่ยมขวัญ เข้ทอง", status: "ผ่าน", score: 3 },
  "54221": { name: "นางสาวยุวดี เกื้อเส้ง", status: "ผ่าน", score: 3 },
  "54222": { name: "นางสาวอุษามณี จันทร์แก้ว", status: "ผ่าน", score: 3 },
  "51284": { name: "นายกรรวี เวชชูแก้ว", status: "ผ่าน", score: 4 },
  "51296": { name: "นายศุภัทรชัย ผุดผ่อง", status: "ผ่าน", score: 5 },
  "51297": { name: "นายสุกฤษฏิ์ เอียดประพันธ์", status: "ผ่าน", score: 5 },
  "51303": { name: "นางสาวชนัญชิดา อินทสโร", status: "ผ่าน", score: 5 },
  "51306": { name: "นางสาวภาณิชา มณีรัตน์", status: "ผ่าน", score: 5 },
  "51310": { name: "นางสาวทัศนภรณ์ มะเดื่อ", status: "ผ่าน", score: 4 },
  "51313": { name: "นางสาวธาดารัศมิ์ ทองพุด", status: "ผ่าน", score: 5 },
  "51314": { name: "นางสาวปรียรัตน์ อนันตพงศ์", status: "ผ่าน", score: 5 },
  "51315": { name: "นางสาวพชรพรรณ เสริมสุข", status: "ผ่าน", score: 5 },
  "51322": { name: "นางสาวศศินา สวนแสดง", status: "ผ่าน", score: 5 },
  "51327": { name: "นางสาวอรพินทร์ วงศ์เนตร", status: "ผ่าน", score: 5 },
  "51357": { name: "นางสาวนภิสา พัฒนา", status: "ผ่าน", score: 5 },
  "51376": { name: "นายธีรภัทร์ รักนุ้ย", status: "ผ่าน", score: 5 },
  "51391": { name: "นางสาวธัญญรัตน์ พวงวงศ์ตระกูล", status: "ผ่าน", score: 5 },
  "51392": { name: "นางสาวธาดารัศมิ์ แก้วผลึก", status: "ผ่าน", score: 5 },
  "51524": { name: "นางสาวนภัสวัลย์ สวนอินทร์", status: "ผ่าน", score: 5 },
  "51552": { name: "นายฐานทัพ ทองแจ่ม", status: "ผ่าน", score: 5 },
  "51667": { name: "นางสาวเปมิกา ดิสระ", status: "ผ่าน", score: 5 },
  "51682": { name: "นายฐปนนท์ ชูนุ่น", status: "ผ่าน", score: 5 },
  "51685": { name: "นายธนวัฒน์ พาสะโร", status: "ผ่าน", score: 4 },
  "51707": { name: "นางสาวดารินทร์ ทองเนื้อขาว", status: "ผ่าน", score: 5 },
  "51725": { name: "นายกฤษณพงศ์ เจริญรักษ์", status: "ผ่าน", score: 5 },
  "51745": { name: "นางสาวติณณาฎา คณะทอง", status: "ผ่าน", score: 5 },
  "51772": { name: "นายดลย์ วงศ์คุณทรัพย์", status: "ไม่ผ่าน", score: 1 },
  "51776": { name: "นายพีรวัชร์ ประชูสวัสดิ์", status: "ผ่าน", score: 4 },
  "51802": { name: "นายธันยกร ศรีรัง", status: "ผ่าน", score: 5 },
  "51808": { name: "นางสาวจิรัชยา สมจิตต์", status: "ผ่าน", score: 5 },
  "51819": { name: "นางสาวศรัณย์พร พระธาตุ", status: "ผ่าน", score: 5 },
  "51822": { name: "นางสาวอริศรา แก้วเกิด", status: "ผ่าน", score: 5 },
  "51837": { name: "นางสาวณมน พิสุทธิชาติ", status: "ผ่าน", score: 4 },
  "52128": { name: "นางสาวปพิชญา สุขสง", status: "ผ่าน", score: 5 },
  "54223": { name: "นายศุภณัฐ นวลชุม", status: "ผ่าน", score: 0 },
  "54224": { name: "นางสาวกนกวรรณ เจริญมาก", status: "ผ่าน", score: 4 },
  "54225": { name: "นางสาวคณภรณ์ หนูฟอง", status: "ผ่าน", score: 3 },
  "54226": { name: "นางสาวชญานิศ เพ็ชรคม", status: "ผ่าน", score: 5 },
  "54227": { name: "นางสาวณหทัย ศุภภัทรจินดา", status: "ผ่าน", score: 4 },
  "54228": { name: "นางสาวนงนภัส จรูญสอน", status: "ผ่าน", score: 5 },
  "54229": { name: "นางสาวพิชญาภา แก้วยศกุล", status: "ผ่าน", score: 3 },
  "54230": { name: "นางสาวศุภาณี แสงอุทัย", status: "ผ่าน", score: 4 },
  "51317": { name: "นางสาวพรไพลิน สนธิสุทธิ์", status: "ผ่าน", score: 5 },
  "51341": { name: "นายลินคอล์น นิลสุก", status: "ผ่าน", score: 4 },
  "54363": { name: "นางสาวภัทรวดี สกุลทอง", status: "ผ่าน", score: 5 },
  "51404": { name: "นางสาวพิมพ์ธารา พรหมสาลี", status: "ผ่าน", score: 4 },
  "51457": { name: "นางสาวสุนิสา สุวรรณโณ", status: "ไม่ผ่าน", score: 1 },
  "51490": { name: "นางสาวณัฐธิดา เหมือนเกียรติ", status: "ไม่ผ่าน", score: 0 },
  "51544": { name: "นางสาวปัทมวรรณ เทพทอง", status: "ผ่าน", score: 4 },
  "51558": { name: "นายพรสวรรค์ สมแสง", status: "ผ่าน", score: 5 },
  "51668": { name: "นางสาวพรรณภรรค อินทสุวรรณ์", status: "ผ่าน", score: 4 },
  "51722": { name: "นางสาวสุกัญญา สุกใส", status: "ผ่าน", score: 4 },
  "51739": { name: "นางสาวชมพูนุท ไหมพุ่ม", status: "ผ่าน", score: 5 },
  "51755": { name: "นางสาวปุณยศา สุเมธาโสธนา", status: "ไม่ผ่าน", score: 0 },
  "51762": { name: "นางสาวศศิกานต์ จันทวิเศษ", status: "ผ่าน", score: 5 },
  "51773": { name: "นายเตสิทธิ์ ถาวร", status: "ผ่าน", score: 5 },
  "51779": { name: "นางสาวเจนจิรา ถีราวุฒิ", status: "ผ่าน", score: 5 },
  "51842": { name: "นางสาวธนภัทร ชัยรัตน์", status: "ผ่าน", score: 5 },
  "51843": { name: "นางสาวธนมนชนก มากสาขา", status: "ผ่าน", score: 3 },
  "81846": { name: "นางสาวเบญจรัตน์ รามไชย", status: "ผ่าน", score: 3 },
  "54231": { name: "นายชลภัทร อรุโณทัย", status: "ผ่าน", score: 4 },
  "54232": { name: "นายฐิติวัฒน์ ดำมาก", status: "ผ่าน", score: 5 },
  "54233": { name: "นายธารธารา อุทัยรัตน์", status: "ผ่าน", score: 3 },
  "54234": { name: "นายปองภพ ไพสิน", status: "ผ่าน", score: 5 },
  "54236": { name: "นางสาวกมลลักษณ์ ไชยทอง", status: "ขาดสอบ", score: null },
  "54237": { name: "นางสาวกัลญา พลเยี่ยม", status: "ผ่าน", score: 3 },
  "54238": { name: "นางสาวจิราวรรณ พุฒสกุล", status: "ผ่าน", score: 5 },
  "54239": { name: "นางสาวชาลีน่า หมัดอาลี", status: "ไม่ผ่าน", score: 0 },
  "54240": { name: "นางสาวธนพร บุญเผือก", status: "ผ่าน", score: 3 },
  "54241": { name: "นางสาวนภัสสร นวลติ้ง", status: "ผ่าน", score: 4 },
  "54242": { name: "นางสาวพัชรกันย์ นิตย์จรัล", status: "ผ่าน", score: 3 },
  "54243": { name: "นางสาวมุซีนา กะสิรัตน์", status: "ผ่าน", score: 4 },
  "54244": { name: "นางสาวเมธาวดี เวียงหงษ์", status: "ผ่าน", score: 4 },
  "54245": { name: "นางสาววรัญยา ดวงสุวรรณ์", status: "ผ่าน", score: 4 },
  "54246": { name: "นางสาววิยะดา ทองสลับล้วน", status: "ไม่ผ่าน", score: 0 },
  "54247": { name: "นางสาวสุมาเรีย ซาหีมซา", status: "ผ่าน", score: 4 },
  "57248": { name: "นางสาวเสาวลักษณ์ นิลวรรณ", status: "ไม่ผ่าน", score: 0 },
  "54250": { name: "นางสาวอริต้า คณะแนม", status: "ผ่าน", score: 5 },
  "54251": { name: "นางสาวเอกาวัลคุ์ จันทร์ทองมล", status: "ผ่าน", score: 4 },
  "54396": { name: "นางสาวนภัชรสรณ์ นุ่นดิษฐ", status: "ผ่าน", score: 5 },
  "54397": { name: "นางสาวชญานิศ สุวรรณเรืองศรี", status: "ผ่าน", score: 4 }
};
function checkResult() {
    const id = document.getElementById('studentId').value.trim();
    const box = document.getElementById('result');

    if (!students[id]) {
      box.innerHTML = "ไม่พบข้อมูลนักเรียน";
      box.className = "result fail";
      return;
    }

    const s = students[id];

    if (s.status === "ผ่าน") {
      box.innerHTML = `
        ขอแสดงความยินดี<br>
        ${s.name}<br>
        คะแนนสอบ: ${s.score}<br>
        <span class="pass">สอบผ่าน</span>
      `;
      box.className = "result pass";

    } else if (s.status === "ขาดสอบ") {
      box.innerHTML = `
        ${s.name}<br>
        <span class="fail">ขาดสอบ</span>
      `;
      box.className = "result fail";

    } else {
      box.innerHTML = `
        ${s.name}<br>
        คะแนนสอบ: ${s.score}<br>
        <span class="fail">สอบไม่ผ่าน</span>
      `;
      box.className = "result fail";
    }
  }
</script>
</body>
</html>
