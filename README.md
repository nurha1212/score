<body>
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <title>ระบบตรวจสอบคะแนนสอบกลางภาค</title>
  <style>
    body {
      font-family: 'Sarabun', sans-serif;
      max-width: 500px;
      margin: 40px auto;
      padding: 20px;
      background: #ffffff;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    input, button {
      width: 100%;
      padding: 12px;
      margin-top: 10px;
      font-size: 16px;
      border: 1px solid #ccc;
      border-radius: 5px;
    }
    button {
      background-color: #00796b;
      color: white;
      cursor: pointer;
    }
    button:hover {
      background-color: #058973;
    }
    #result {
      margin-top: 20px;
      font-size: 18px;
      color: #333;
    }
  </style>
</head>
<body>
  <h2>ระบบตรวจสอบคะแนนสอบกลางภาค</h2>
  <p>กรอกเลขประจำตัวนักเรียน</p>

  <input type="text" id="studentId" placeholder="กรอกเลขประจำตัว">
  <button onclick="checkScore()">ดูคะแนน</button>

  <div id="result"></div>

  <script>
    // ข้อมูลนักเรียนในระบบ
    const students = [
      { id: "38171", name: "ด.ช.กรวิทย์ แสงแก้ว", score: 3 },
      { id: "38172", name: "ด.ช.กฤดาการ พงษ์ศรีเกิด", score: 7 },
      { id: "38173", name: "ด.ช.กิตติภูมิ คล้ายแก้ว", score: 6 },
      { id: "38174", name: "ด.ช.จตุภัทธิ์ นุ้ยประสาท", score: 2 },
      { id: "38175", name: "ด.ช.เฉลิมชัย จันทร์พูล", score: 9 },
      { id: "38176", name: "ด.ช.ณรงค์ชัย บางพงศ์", score: 7 },
      { id: "38177", name: "ด.ช.ต้นธันวา บุตรร่ม", score: 5 },
      { id: "38179", name: "ด.ช.ธนกร มั่นดี", score: 2 },
      { id: "38180", name: "ด.ช.ธนกิต แดงมณี", score: 2 },
      { id: "38182", name: "ด.ช.ธันวา ขุนทอง", score: 8 },
      { id: "38183", name: "ด.ช.ธีภพ พวงพุ่ม", score: 4 },
      { id: "38185", name: "ด.ช.นราวิชญ์ เดชแก้ว", score: 6 },
      { id: "38186", name: "ด.ช.ปริญญา ขุนเพชร", score: 4 },
      { id: "38187", name: "ด.ช.ภควัต จันทร์เลื่อน", score: 4 },
      { id: "38188", name: "ด.ช.ภวัต มณี", score: 7 },
      { id: "38189", name: "ด.ช.เมธพนธ์ จันทร์เลื่อน", score: 8 },
      { id: "38190", name: "ด.ช.วชิรวิทย์ บัวสด", score: 8 },
      { id: "38191", name: "ด.ช.สืบสกุล ชายแก้ว", score: 7 },
      { id: "38192", name: "ด.ช.สุกลวัฒน์ หมื่นสิทธิ์", score: 3 },
      { id: "38193", name: "ด.ช.สุวรินทร์ นิรันดร์พุฒ", score: 4 },
      { id: "38194", name: "ด.ช.อมัส มุนีหมะ", score: 8 },
      { id: "38195", name: "ด.ช.อานัส สำราญวงษ์", score: 6 },
      { id: "38504", name: "ด.ช.อัฟฟาน มามะ", score: 4 },
      { id: "38196", name: "ด.ญ.กัญญาภัค ทิพย์ยอแล๊ะ", score: 8 },
      { id: "38197", name: "ด.ญ.ซาร่า ประทานกุล", score: 8 },
      { id: "38198", name: "ด.ญ.ณัฐชยา แก้วมณี", score: 9 },
      { id: "38199", name: "ด.ญ.ปนิตา กรธัชฐลิ้ม", score: 9 },
      { id: "38200", name: "ด.ญ.ปริยฉัตร สุวรรณาคม", score: 7 },
      { id: "38201", name: "ด.ญ.ปูริดา นวลปาน", score: 9 },
      { id: "38202", name: "ด.ญ.เปมิศา ยิ้มหิ้น", score: 9 },
      { id: "38203", name: "ด.ญ.พิชามญช์ ประทีป", score: 9 },
      { id: "38204", name: "ด.ญ.วรกานต์ แซ่ตัน", score: 8 },
      { id: "38205", name: "ด.ญ.วรัญญา เปพันดุง", score: 7 },
      { id: "38206", name: "ด.ญ.หทัยภัทร จานลาน", score: 8 },
      { id: "38207", name: "ด.ญ.ฮาซิลา นิยม", score: 7 },
    ];

    function checkScore() {
      const id = document.getElementById("studentId").value.trim();
      const resultDiv = document.getElementById("result");

      if (!id) {
        resultDiv.innerHTML = `<span style="color:red;">⚠️ กรุณากรอกเลขประจำตัวนักเรียน</span>`;
        return;
      }

      const student = students.find(s => s.id === id);

      if (student) {
        resultDiv.innerHTML = `
          ✅ <strong>ชื่อ :</strong> ${student.name}<br>
          📊 <strong>คะแนน :</strong> ${student.score} คะแนน
        `;
      } else {
        resultDiv.innerHTML = `<span style="color:red;">❌ ไม่พบเลขประจำตัวนี้ในระบบ</span>`;
      }
    }
  </script>
</body>
</html>
