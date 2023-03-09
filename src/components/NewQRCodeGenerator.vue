<template>
  <div class="qr-code-generator">
    <h1 class="qr-code-generator__title">QR Code Generator</h1>
    <form @submit.prevent="generateQRCode" class="qr-code-generator__form">
      <label for="url" class="qr-code-generator__label">Enter URL:</label>
      <input
        type="text"
        id="url"
        v-model="url"
        required
        class="qr-code-generator__input"
      />
      <button type="submit" class="qr-code-generator__button">
        Generate QR Code
      </button>
    </form>
    <div v-if="qrCode" class="qr-code-generator__result">
      <h2 class="qr-code-generator__subtitle">QR Code:</h2>
      <div class="qr-code-generator__image-container">
        <img
          :src="qrCode100"
          alt="QR Code (100px)"
          class="qr-code-generator__image qr-code-generator__image--100"
          @click="copyQRCode(100)"
        />
        <img
          :src="qrCode200"
          alt="QR Code (200px)"
          class="qr-code-generator__image qr-code-generator__image--200"
          @click="copyQRCode(200)"
        />
        <img
          :src="qrCode400"
          alt="QR Code (400px)"
          class="qr-code-generator__image qr-code-generator__image--400"
          @click="copyQRCode(400)"
        />
        <img
          :src="qrCode1000"
          alt="QR Code (1000px)"
          class="qr-code-generator__image qr-code-generator__image--1000"
          @click="copyQRCode(1000)"
        />
        <img
          :src="qrCode2000"
          alt="QR Code (2000px)"
          class="qr-code-generator__image qr-code-generator__image--2000"
          @click="copyQRCode(2000)"
        />
      </div>
      <div class="qr-code-generator__actions">
        <button
          @click="downloadQRCode(qrCode100, 'qr-code-100.png')"
          class="qr-code-generator__button"
        >
          Download (100px)
        </button>
        <button
          @click="downloadQRCode(qrCode200, 'qr-code-200.png')"
          class="qr-code-generator__button"
        >
          Download (200px)
        </button>
        <button
          @click="downloadQRCode(qrCode400, 'qr-code-400.png')"
          class="qr-code-generator__button"
        >
          Download (400px)
        </button>
        <button
          @click="downloadQRCode(qrCode1000, 'qr-code-1000.png')"
          class="qr-code-generator__button"
        >
          Download (1000px)
        </button>
        <button
          @click="downloadQRCode(qrCode2000, 'qr-code-2000.png')"
          class="qr-code-generator__button"
        >
          Download (2000px)
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import QRCode from "qrcode";

export default {
  name: "QRCodeGenerator",
  data() {
    return {
      url: "",
      qrCode: null,
    };
  },
  computed: {
    qrCode100() {
      return this.resizeQRCode(this.qrCode, 100);
    },
    qrCode200() {
      return this.resizeQRCode(this.qrCode, 200);
    },
    qrCode400() {
      return this.resizeQRCode(this.qrCode, 400);
    },
    qrCode1000() {
      return this.resizeQRCode(this.qrCode, 1000);
    },
    qrCode2000() {
      return this.resizeQRCode(this.qrCode, 2000);
    },
  },
  methods: {
    generateQRCode() {
      QRCode.toDataURL(this.url)
        .then((qrCode) => {
          this.qrCode = qrCode;
        })
        .catch((error) => {
          console.error(error);
        });
    },
    resizeQRCode(qrCode, size) {
      const canvas = document.createElement("canvas");
      const context = canvas.getContext("2d");
      const img = new Image();
      img.src = qrCode;
      return new Promise((resolve, reject) => {
        img.onload = () => {
          canvas.width = size;
          canvas.height = size;
          context.drawImage(img, 0, 0, size, size);
          resolve(canvas.toDataURL());
        };
        img.onerror = reject;
      });
    },
    downloadQRCode(qrCode, fileName) {
      const link = document.createElement("a");
      link.download = fileName;
      link.href = qrCode;
      link.click();
    },
    copyQRCode(size) {
      const qrCode = this["qrCode" + size];
      const canvas = document.createElement("canvas");
      const context = canvas.getContext("2d");
      const img = new Image();
      img.src = qrCode;
      img.onload = () => {
        canvas.width = img.width;
        canvas.height = img.height;
        context.drawImage(img, 0, 0);
        canvas.toBlob((blob) => {
          navigator.clipboard
            .write([new ClipboardItem({ "image/png": blob })])
            .catch((error) => {
              console.error(error);
            });
        });
      };
    },
  },
};
</script>

<style scoped>
.qr-code-generator {
  max-width: 400px;
  margin: 0 auto;
  text-align: center;
}

.qr-code-generator__title {
  font-size: 24px;
  margin-bottom: 20px;
}

.qr-code-generator__form {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.qr-code-generator__label {
  font-size: 16px;
  margin-bottom: 10px;
}

.qr-code-generator__input {
  width: 100%;
  padding: 10px;
  border: none;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.qr-code-generator__button {
  margin-top: 10px;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  background-color: #007bff;
  color: #fff;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease-in-out;
}

.qr-code-generator__button:hover {
  background-color: #0062cc;
}

.qr-code-generator__result {
  margin-top: 20px;
}

.qr-code-generator__subtitle {
  font-size: 18px;
  margin-bottom: 10px;
}

.qr-code-generator__image-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin-bottom: 10px;
}

.qr-code-generator__image {
  margin: 5px;
  cursor: pointer;
}

.qr-code-generator__image--100 {
  max-width: 100px;
  height: auto;
}

.qr-code-generator__image--200 {
  max-width: 200px;
  height: auto;
}

.qr-code-generator__image--400 {
  max-width: 400px;
  height: auto;
}

.qr-code-generator__image--1000 {
  max-width: 1000px;
  height: auto;
}

.qr-code-generator__image--2000 {
  max-width: 2000px;
  height: auto;
}

.qr-code-generator__actions {
  display: flex;
  justify-content: space-between;
}

@media (min-width: 576px) {
  .qr-code-generator {
    max-width: 600px;
  }

  .qr-code-generator__title {
    font-size: 36px;
  }

  .qr-code-generator__input {
    width: 80%;
  }
}
</style>
