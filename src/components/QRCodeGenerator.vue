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
      <img :src="qrCode" alt="QR Code" class="qr-code-generator__image" />
      <div class="qr-code-generator__actions">
        <button @click="downloadQRCode" class="qr-code-generator__button">
          Download QR Code
        </button>
        <button @click="copyQRCode" class="qr-code-generator__button">
          Copy QR Code
        </button>
      </div>
    </div>
    <div v-if="notification" :class="{ notification: true, show: showNotification }">{{ notification }}</div>
  </div>
</template>

<script>
import QRCode from 'qrcode'

export default {
  name: 'QRCodeGenerator',
  data() {
    return {
      url: '',
      qrCode: null,
      notification: ''
    }
  },
  methods: {
    generateQRCode() {
      QRCode.toDataURL(this.url)
        .then(qrCode => {
          this.qrCode = qrCode
        })
        .catch(error => {
          console.error(error)
        })
    },
    downloadQRCode() {
      const link = document.createElement('a')
      link.download = 'qr-code.png'
      link.href = this.qrCode
      link.click()
      this.showNotification('QR Code downloaded successfully')
    },
    copyQRCode() {
      const canvas = document.createElement('canvas')
      const context = canvas.getContext('2d')
      const img = new Image()
      img.src = this.qrCode
      img.onload = () => {
        canvas.width = img.width
        canvas.height = img.height
        context.drawImage(img, 0, 0)
        canvas.toBlob(blob => {
          navigator.clipboard.write([new ClipboardItem({ 'image/png': blob })])
            .then(() => {
              this.showNotification('QR Code copied to clipboard')
              this.showNotification = true;
              setTimeout(() => {
        this.showNotification = false;
      }, 1500);
            })
            .catch(error => {
              console.error(error)
            })
        })
      }
    },
    showNotification(message) {
      this.notification = message
      setTimeout(() => {
        this.notification = ''
      }, 1500)
    }
  }
}
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

.qr-code-generator__image {
  max-width: 100%;
  height: auto;
  margin-bottom: 10px;
  max-height: 400px; /* Add this to limit the maximum height of the image */
  min-height: 200px;
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

  .qr-code-generator__image {
    max-width: 80%; /* Double the maximum width for larger screens */
  }
}
.notification {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 9999;
  background-color: #5cb85c;
  color: white;
  padding: 10px 20px;
  border-radius: 5px;
  font-size: 16px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  opacity: 0;
  transition: opacity 0.3s ease-in-out;
}

.notification.show {
  opacity: 1;
}
</style>
