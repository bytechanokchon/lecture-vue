# Create vue application
## The Application Instance
แอปพลิเคชัน Vue ทุกแอปเริ่มต้นด้วยการสร้างอินสแตนซ์แอปพลิเคชันใหม่ผ่านฟังก์ชัน createApp

    import { createApp } from 'vue'

    const app = createApp({
      /* root component options */
    })

## The Root Component
ออบเจกต์ที่เราส่งเข้าไปใน createApp นั้น แท้จริงแล้วคือคอมโพเนนต์หนึ่ง โดยทุกแอปพลิเคชันจำเป็นต้องมี "รูทคอมโพเนนต์" (root component) ซึ่งสามารถบรรจุคอมโพเนนต์อื่น ๆ ไว้ภายในในฐานะคอมโพเนนต์ลูกได้

    import { createApp } from 'vue'
    // import the root component App from a single-file component.
    import App from './App.vue'

    const app = createApp(App)

## Mounting the App
อินสแตนซ์ของแอปพลิเคชันจะยังไม่แสดงผลสิ่งใดออกมาจนกว่าจะมีการเรียกเมธอด .mount() โดยเมธอดนี้ต้องการอาร์กิวเมนต์เป็น "คอนเทนเนอร์" (container) ซึ่งอาจเป็นองค์ประกอบ DOM จริงๆ หรือเป็นสตริงตัวเลือก (selector string) ก็ได้

    <div id="app"></div>

    app.mount('#app')


## App Configurations
อินสแตนซ์ของแอปพลิเคชันจะมีออบเจกต์ `.config` ซึ่งช่วยให้เราสามารถกำหนดค่าตัวเลือกต่าง ๆ ในระดับแอปพลิเคชันได้ เช่น การกำหนดตัวจัดการข้อผิดพลาด (error handler) ในระดับแอปพลิเคชัน เพื่อดักจับข้อผิดพลาดที่เกิดขึ้นจากคอมโพเนนต์ลูกทั้งหมดที่อยู่ภายใต้แอปพลิเคชันนั้น

    app.config.errorHandler = (err) => {
        /* handle error */
    }

อินสแตนซ์ของแอปพลิเคชันยังมีเมธอดบางอย่างสำหรับลงทะเบียนทรัพยากรที่อยู่ในขอบเขตของแอปพลิเคชัน (app-scoped assets) ตัวอย่างเช่น การลงทะเบียนคอมโพเนนต์

    app.component('TodoDeleteButton', TodoDeleteButton)

วิธีนี้ทำให้ปุ่ม TodoDeleteButton สามารถใช้งานได้ทุกที่ในแอปของเรา

