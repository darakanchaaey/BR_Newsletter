<html lang="th">
<head>
  <meta charset="UTF-8" />
  <title>BR Newsletter Editor</title>
  <!-- ใช้ฟอนต์ระบบทั่วไปให้โหลดเร็ว -->
  <style>
    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      background: #f3f5f9;
      color: #222;
      display: flex;
      min-height: 100vh;
    }

    .app {
      display: flex;
      flex: 1;
      padding: 16px;
      gap: 16px;
    }

    /* แผงควบคุมด้านซ้าย */
    .controls {
      width: 360px;
      max-width: 100%;
      background: #ffffff;
      border-radius: 16px;
      box-shadow: 0 10px 25px rgba(0,0,0,0.08);
      padding: 18px 18px 14px;
      display: flex;
      flex-direction: column;
      gap: 14px;
    }

    .controls-header {
      border-bottom: 1px solid #eee;
      padding-bottom: 6px;
      margin-bottom: 4px;
    }

    .controls-header h1 {
      font-size: 18px;
      margin: 0;
      font-weight: 700;
      color: #a00013; /* โทนแดง BR */
    }

    .controls-header p {
      margin: 2px 0 6px;
      font-size: 12px;
      color: #666;
    }

    .field {
      margin-bottom: 4px;
    }

    .field label {
      display: flex;
      justify-content: space-between;
      align-items: baseline;
      font-size: 13px;
      font-weight: 600;
      margin-bottom: 4px;
    }

    .hint {
      font-size: 11px;
      color: #888;
      margin-top: 1px;
    }

    textarea,
    input[type="text"],
    input[type="file"] {
      width: 100%;
      font-family: inherit;
      font-size: 13px;
      border-radius: 10px;
      border: 1px solid #d0d4dd;
      padding: 8px 10px;
      outline: none;
      transition: border-color 0.15s, box-shadow 0.15s, background 0.15s;
      background: #fafbff;
    }

    textarea:focus,
    input[type="text"]:focus,
    input[type="file"]:focus {
      border-color: #a00013;
      box-shadow: 0 0 0 2px rgba(160,0,19,0.12);
      background: #ffffff;
    }

    textarea {
      min-height: 130px;
      max-height: 220px;
      resize: vertical;
    }

    .char-counter {
      font-size: 11px;
      color: #666;
    }

    .char-counter span {
      font-weight: 600;
      color: #a00013;
    }

    .thumbs {
      display: grid;
      grid-template-columns: repeat(4, minmax(0, 1fr));
      gap: 6px;
      margin-top: 6px;
    }

    .thumb {
      position: relative;
      padding-top: 100%;
      border-radius: 8px;
      overflow: hidden;
      background: #f2f2f7;
      border: 1px dashed #d0d4dd;
    }

    .thumb img {
      position: absolute;
      inset: 0;
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .thumb-index {
      position: absolute;
      top: 2px;
      left: 4px;
      font-size: 10px;
      padding: 1px 4px;
      border-radius: 999px;
      background: rgba(0,0,0,0.55);
      color: #fff;
    }

    .buttons-row {
      display: flex;
      gap: 8px;
      margin-top: 6px;
    }

    button {
      border: none;
      border-radius: 999px;
      padding: 7px 14px;
      font-size: 13px;
      font-weight: 600;
      cursor: pointer;
      display: inline-flex;
      align-items: center;
      gap: 6px;
      white-space: nowrap;
      transition: transform 0.1s, box-shadow 0.1s, background 0.1s, opacity 0.1s;
    }

    button:active {
      transform: translateY(1px);
      box-shadow: none;
    }

    .btn-primary {
      background: #a00013;
      color: #fff;
      box-shadow: 0 6px 14px rgba(160,0,19,0.35);
    }

    .btn-primary:hover {
      background: #b61427;
    }

    .btn-secondary {
      background: #f1f3f7;
      color: #444;
    }

    .btn-secondary:hover {
      background: #e4e7f0;
    }

    .btn-ghost {
      background: transparent;
      color: #777;
    }

    .btn-ghost:hover {
      background: #f5f5f7;
    }

    .warning {
      font-size: 11px;
      color: #a00013;
      margin-top: 2px;
    }

    /* พื้นที่แสดงตัวอย่างด้านขวา */
    .preview-wrapper {
      flex: 1;
      display: flex;
      align-items: center;
      justify-content: center;
      min-width: 0;
    }

    .preview-panel {
      background: #ffffff;
      border-radius: 18px;
      box-shadow: 0 12px 30px rgba(0,0,0,0.1);
      padding: 12px 16px 18px;
      display: flex;
      flex-direction: column;
      gap: 10px;
      align-items: center;
      max-width: 780px;
      width: 100%;
    }

    .preview-title {
      align-self: stretch;
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 13px;
      color: #555;
      padding-inline: 4px;
    }

    .preview-title span {
      font-weight: 600;
      color: #a00013;
    }

    .preview-scroll {
      width: 100%;
      overflow: auto;
      display: flex;
      justify-content: center;
      padding: 4px;
    }

    /* กล่องจดหมายข่าวบน template */
    .newsletter {
      position: relative;
      width: 650px;
      max-width: 100%;
      aspect-ratio: 768 / 1085; /* ปรับอัตราส่วนตามรูปต้นฉบับ */
      border-radius: 8px;
      overflow: hidden;
      background-image: url("br-news-template.png"); /* <--- เปลี่ยนชื่อไฟล์ได้ตามต้องการ */
      background-size: cover;
      background-position: top center;
      background-repeat: no-repeat;
    }

    /* พื้นที่ด้านใน (ช่องว่างสีขาว) */
    .newsletter-content {
      position: absolute;
      top: 23%;
      left: 7%;
      right: 7%;
      bottom: 10%;
      display: flex;
      flex-direction: column;
      justify-content: flex-start;
      gap: 10px;
      padding: 10px;
      overflow: hidden;
      backdrop-filter: none;
    }

    .newsletter-text {
      flex: 0 0 auto;
      max-height: 50%;
      font-size: 14px;
      line-height: 1.5;
      color: #333;
      padding: 8px 10px;
      border-radius: 10px;
      background: rgba(255,255,255,0.9);
      overflow: hidden;
      word-wrap: break-word;
      white-space: pre-wrap;
    }

    .newsletter-placeholder {
      color: #a0a4b8;
      font-style: italic;
    }

    .newsletter-gallery {
      flex: 1 1 auto;
      margin-top: 6px;
      padding: 6px;
      border-radius: 10px;
      background: rgba(255,255,255,0.88);
      display: grid;
      grid-template-columns: repeat(3, minmax(0, 1fr));
      grid-auto-rows: 1fr;
      gap: 6px;
      overflow: hidden;
    }

    .newsletter-gallery.empty {
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 12px;
      color: #b0b3c4;
      text-align: center;
      padding: 10px;
    }

    .newsletter-photo {
      position: relative;
      border-radius: 8px;
      overflow: hidden;
      background: #f3f3f7;
      border: 1px solid rgba(0,0,0,0.05);
    }

    .newsletter-photo img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      display: block;
    }

    .newsletter-photo-badge {
      position: absolute;
      bottom: 4px;
      right: 4px;
      font-size: 10px;
      padding: 1px 4px;
      border-radius: 999px;
      background: rgba(0,0,0,0.6);
      color: #fff;
    }

    /* ปรับจอเล็ก */
    @media (max-width: 900px) {
      .app {
        flex-direction: column;
      }
      .controls {
        width: 100%;
      }
      .preview-panel {
        max-width: 100%;
      }
    }

    /* สำหรับสั่งพิมพ์ให้มีเฉพาะจดหมายข่าว */
    @media print {
      body {
        background: #fff;
      }
      .app {
        padding: 0;
      }
      .controls,
      .preview-title,
      .buttons-row-print {
        display: none !important;
      }
      .preview-panel {
        box-shadow: none;
        padding: 0;
      }
      .newsletter {
        width: 100%;
        border-radius: 0;
      }
    }
  </style>

  <!-- html2canvas สำหรับบันทึกเป็นรูปภาพ -->
  <script src="https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js"></script>
</head>
<body>
<div class="app">
  <!-- แผงควบคุม -->
  <aside class="controls">
    <div class="controls-header">
      <h1>BR Newsletter Editor</h1>
      <p>กรอกข้อความประชาสัมพันธ์และอัปโหลดรูปภาพกิจกรรมของโรงเรียน</p>
    </div>

    <!-- ข้อความประชาสัมพันธ์ -->
    <div class="field">
      <label for="contentInput">
        ข้อความประชาสัมพันธ์
        <span class="char-counter"><span id="charCount">0</span>/1000 ตัวอักษร</span>
      </label>
      <textarea id="contentInput"
                maxlength="1000"
                placeholder="ใส่ข้อความประชาสัมพันธ์กิจกรรม&nbsp;ทั้งภาษาไทยและภาษาอังกฤษ (ไม่เกิน 1,000 ตัวอักษร)&#10;เช่น&#10;โรงเรียนเบญจมราชาลัย ในพระบรมราชูปถัมภ์ จัดกิจกรรม... / Benjamarachalai School organized ...">
      </textarea>
      <div class="hint">
        รองรับทั้งภาษาไทยและภาษาอังกฤษ ข้อความที่พิมพ์จะแสดงในช่องว่างตรงกลางของ template
      </div>
    </div>

    <!-- อัปโหลดรูป -->
    <div class="field">
      <label for="imageInput">
        รูปภาพกิจกรรม
        <span style="font-size:11px;font-weight:400;color:#666;">อัปโหลดได้สูงสุด 9 รูป</span>
      </label>
      <input type="file" id="imageInput" accept="image/*" multiple />
      <div class="hint">
        แนะนำให้ใช้รูปแนวนอนหรือจัตุรัส ขนาดใกล้เคียงกัน เพื่อให้จัดเรียงสวยงามในตาราง 3×3
      </div>
      <div id="imageWarning" class="warning" style="display:none;">
        * เลือกรูปได้ไม่เกิน 9 รูป ระบบจะใช้เฉพาะ 9 รูปแรก
      </div>
      <div class="thumbs" id="thumbList">
        <!-- แสดงรูปตัวอย่างเล็ก ๆ -->
      </div>
    </div>

    <!-- ปุ่มการทำงาน -->
    <div class="buttons-row">
      <button class="btn-primary" id="downloadBtn" type="button">
        💾 บันทึกเป็นรูปภาพ (PNG)
      </button>
      <button class="btn-secondary" id="printBtn" type="button">
        🖨️ พิมพ์ / บันทึกเป็น PDF
      </button>
    </div>
    <div class="buttons-row">
      <button class="btn-ghost" id="clearTextBtn" type="button">
        ล้างข้อความ
      </button>
      <button class="btn-ghost" id="clearImagesBtn" type="button">
        ล้างรูปภาพ
      </button>
    </div>

  </aside>

  <!-- ส่วนแสดงตัวอย่าง -->
  <main class="preview-wrapper">
    <div class="preview-panel">
      <div class="preview-title">
        <span>ตัวอย่างจดหมายข่าว BR NEWS</span>
        <span>Preview Only</span>
      </div>
      <div class="preview-scroll">
        <div class="newsletter" id="newsletter">
          <div class="newsletter-content">
            <div class="newsletter-text" id="previewText">
              <span class="newsletter-placeholder">
                ข้อความประชาสัมพันธ์ของคุณจะแสดงในส่วนนี้
                (ไทย/อังกฤษ ไม่เกิน 1,000 ตัวอักษร)
              </span>
            </div>

            <div class="newsletter-gallery empty" id="previewImages">
              ยังไม่มีรูปภาพกิจกรรม · สามารถอัปโหลดได้สูงสุด 9 รูป
            </div>
          </div>
        </div>
      </div>
      <div class="buttons-row buttons-row-print">
        <button class="btn-secondary" id="fitTextBtn" type="button">
          🔍 ปรับให้ข้อความอ่านง่าย
        </button>
        <button class="btn-secondary" id="resetZoomBtn" type="button">
          รีเซ็ตขนาดตัวอย่าง
        </button>
      </div>
    </div>
  </main>
</div>

<script>
  (function () {
    const maxChars = 1000;
    const maxImages = 9;

    const contentInput = document.getElementById("contentInput");
    const charCount = document.getElementById("charCount");
    const previewText = document.getElementById("previewText");

    const imageInput = document.getElementById("imageInput");
    const previewImages = document.getElementById("previewImages");
    const thumbList = document.getElementById("thumbList");
    const imageWarning = document.getElementById("imageWarning");

    const downloadBtn = document.getElementById("downloadBtn");
    const printBtn = document.getElementById("printBtn");
    const clearTextBtn = document.getElementById("clearTextBtn");
    const clearImagesBtn = document.getElementById("clearImagesBtn");

    const fitTextBtn = document.getElementById("fitTextBtn");
    const resetZoomBtn = document.getElementById("resetZoomBtn");
    const newsletter = document.getElementById("newsletter");

    /* -------- จัดการข้อความ -------- */
    function updateText() {
      const text = contentInput.value.slice(0, maxChars);
      if (text !== contentInput.value) {
        contentInput.value = text;
      }
      charCount.textContent = text.length.toString();
      if (text.trim().length === 0) {
        previewText.innerHTML =
          '<span class="newsletter-placeholder">' +
          'ข้อความประชาสัมพันธ์ของคุณจะแสดงในส่วนนี้ ' +
          '(ไทย/อังกฤษ ไม่เกิน 1,000 ตัวอักษร)' +
          "</span>";
      } else {
        previewText.textContent = text;
      }
    }

    contentInput.addEventListener("input", updateText);
    updateText();

    /* -------- จัดการรูปภาพ -------- */
    function clearImages() {
      previewImages.innerHTML = "";
      thumbList.innerHTML = "";
      previewImages.classList.add("empty");
      previewImages.textContent =
        "ยังไม่มีรูปภาพกิจกรรม · สามารถอัปโหลดได้สูงสุด 9 รูป";
      imageWarning.style.display = "none";
      imageInput.value = ""; // เคลียร์ input
    }

    function handleImages(files) {
      if (!files || files.length === 0) {
        clearImages();
        return;
      }

      const fileArray = Array.from(files);
      const usingFiles = fileArray.slice(0, maxImages);

      imageWarning.style.display =
        fileArray.length > maxImages ? "block" : "none";

      previewImages.innerHTML = "";
      previewImages.classList.remove("empty");
      thumbList.innerHTML = "";

      usingFiles.forEach((file, index) => {
        const reader = new FileReader();
        reader.onload = (e) => {
          const src = e.target.result;

          // สร้างรูปในตารางตัวอย่างบน template
          const photoWrapper = document.createElement("div");
          photoWrapper.className = "newsletter-photo";
          const img = document.createElement("img");
          img.src = src;
          const badge = document.createElement("div");
          badge.className = "newsletter-photo-badge";
          badge.textContent = index + 1;
          photoWrapper.appendChild(img);
          photoWrapper.appendChild(badge);
          previewImages.appendChild(photoWrapper);

          // สร้าง thumbnail ด้านซ้าย
          const thumb = document.createElement("div");
          thumb.className = "thumb";
          const tImg = document.createElement("img");
          tImg.src = src;
          const indexBadge = document.createElement("div");
          indexBadge.className = "thumb-index";
          indexBadge.textContent = index + 1;
          thumb.appendChild(tImg);
          thumb.appendChild(indexBadge);
          thumbList.appendChild(thumb);
        };
        reader.readAsDataURL(file);
      });
    }

    imageInput.addEventListener("change", (e) => {
      handleImages(e.target.files);
    });

    /* -------- ปุ่มล้างข้อมูล -------- */
    clearTextBtn.addEventListener("click", () => {
      contentInput.value = "";
      updateText();
    });

    clearImagesBtn.addEventListener("click", () => {
      clearImages();
    });

    /* -------- บันทึกเป็นรูปภาพ (PNG) -------- */
    downloadBtn.addEventListener("click", () => {
      html2canvas(newsletter, {
        useCORS: true,
        scale: 2
      }).then((canvas) => {
        const link = document.createElement("a");
        link.download = "BR-Newsletter.png";
        link.href = canvas.toDataURL("image/png");
        link.click();
      });
    });

    /* -------- พิมพ์ / PDF -------- */
    printBtn.addEventListener("click", () => {
      window.print();
    });

    /* -------- ปุ่มปรับขนาด preview (ง่าย ๆ ) -------- */
    let zoomed = false;
    fitTextBtn.addEventListener("click", () => {
      newsletter.style.transformOrigin = "top center";
      newsletter.style.transform = "scale(1.12)";
      zoomed = true;
    });

    resetZoomBtn.addEventListener("click", () => {
      newsletter.style.transform = "scale(1)";
      zoomed = false;
    });
  })();
</script>
</body>
</html>
