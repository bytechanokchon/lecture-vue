# Introduction
## What is Vue ?
เป็น javascript framework สำหรับสร้างส่วนติดต่อกับผู้ใช้ (User Interface) โดยพัฒนามาจาก Html, Css, Javascript และการเขียนโปรแกรมเชิงประกาศ (Declarative Programming) ที่เน้นการแบ่งส่วนประกอบ (Component-based) ซึ่งช่วยให้สามารถพัฒนาส่วนประกอบที่ซับซ้อนได้มีประสิทธิภาพ

    import { createApp, ref } from 'vue'
    createApp({
      setup() {
        return {
          count: ref(0)
        }
      }
    }).mount('#app')

    <div id="app">
        <button @click="count++">
            Count is: {{ count }}
        </button>
    </div>

จากโค้ดตัวอย่างด้านบน จะมี 2 ส่วนหลักของ vue ดังนีี้
1. การใช้งานวัยกรแบบ template บน html ({{ count }}) ทำให้สามารถดึงสถานะจาก javascript มาแสดงผลได้
2. ติดตามการเปลี่ยนแปลงของสถานะใน javascript โดยอัตโนมัติ และแก้ไข dom อย่างมีประสิทธิภาพ

## Single-File Components (SFC)
เป็นโปรเจค vue ขนาดเล็กที่รวม html, css, javascript ไว้ในไฟล์เดียวกัน (ไฟล์นามสกุล .vue) 

    <script setup>
        import { ref } from 'vue'
        const count = ref(0)
    </script>

    <template>
        <button @click="count++">Count is: {{ count }}</button>
    </template>

    <style scoped>
        button {
          font-weight: bold;
        }
    </style>

## API Styles
ใน vue จะแบกรูปแบบการเขียนออกเป็น 2 แบบ คือ Option API และ Composition API

#### Option API
เป็นรูปแบบที่จัดโค้ดตามประเภทของความสามารถ เช่น data, methods, computed และ mounted

- data() ใช้เก็บ State
- methods ใช้เก็บ Function
- computed ใช้คำนวณค่าจาก State
- this ใช้อ้างอิงข้อมูลและ Methods ภายใน Component

####
    <script>
        export default {
            // Properties returned from data() become reactive state
            // and will be exposed on `this`.
            data() {
              return {
                count: 0
              }
            },

            // Methods are functions that mutate state and trigger updates.
            // They can be bound as event handlers in templates.
            methods: {
              increment() {
                this.count++
              }
            },

            // Lifecycle hooks are called at different stages
            // of a component's lifecycle.
            // This function will be called when the component is mounted.
            mounted() {
              console.log(`The initial count is ${this.count}.`)
            }
        }
    </script>

    <template>
        <button @click="increment">Count is: {{ count }}</button>
    </template>

#### Composition API
ใช้ฟังก์ชันต่าง ๆ เช่น ref(), computed() และ onMounted() เพื่อจัดการ State และ Lifecycle
จุดสำคัญคือ:
- ref() ใช้สร้าง Reactive State
- computed() ใช้คำนวณค่าจาก State
- สามารถประกาศ Function ได้โดยตรง
- ไม่จำเป็นต้องใช้ this
- `<script setup>` ช่วยให้เขียน Component ได้กระชับ

####
    <script setup>
        import { ref, onMounted } from 'vue'

        // reactive state
        const count = ref(0)

        // functions that mutate state and trigger updates
        function increment() {
          count.value++
        }

        // lifecycle hooks
        onMounted(() => {
          console.log(`The initial count is ${count.value}.`)
        })
    </script>

    <template>
        <button @click="increment">Count is: {{ count }}</button>
    </template>