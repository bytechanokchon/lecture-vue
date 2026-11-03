# Template Syntax
Vue ใช้ Template Syntax ซึ่งมีพื้นฐานจาก HTML เพื่อให้เราสามารถเชื่อมโยง (bind) DOM ที่แสดงผลบนหน้าเว็บเข้ากับข้อมูลของ Component ได้อย่างเป็นรูปแบบที่เข้าใจง่าย

Template ของ Vue เป็น HTML ที่ถูกต้องตามมาตรฐาน จึงสามารถถูกแปลความหมายโดย Browser และ HTML Parser ได้

เบื้องหลัง Vue จะนำ Template ไป Compile เป็น JavaScript ที่ผ่านการปรับประสิทธิภาพแล้ว เมื่อทำงานร่วมกับระบบ Reactivity ของ Vue จะช่วยให้ Vue สามารถ:
- ตรวจจับว่าข้อมูลส่วนใดเปลี่ยนแปลง
- คำนวณว่าต้อง Render Component ส่วนใดใหม่
- แก้ไข DOM เฉพาะส่วนที่จำเป็น

### Text Interpolation — การแสดงข้อความจากข้อมูล
รูปแบบการเชื่อมโยงข้อมูลที่ง่ายที่สุดคือการใช้ Mustache Syntax ซึ่งใช้วงเล็บปีกกาสองชั้น `{{ }}`

    <script setup>
        import { ref } from 'vue';
        const message = ref('Hello World from Vue 3!');
    </script>

    <template>
        <p>{{ message }}</p>
    </template>

### Security Warning
การนำ HTML จากแหล่งข้อมูลที่ไม่น่าเชื่อถือมาแสดงโดยตรงอาจทำให้เกิดช่องโหว่ XSS (Cross-Site Scripting)

ห้ามใช้ v-html กับข้อมูลที่ผู้ใช้ป้อนเข้ามาโดยตรง หากยังไม่ได้ตรวจสอบหรือทำ Sanitization อย่างเหมาะสม

### Attribute Bindings — การผูกข้อมูลเข้ากับ Attribute
เราไม่สามารถใช้ Mustache `{{ }}` ภายใน HTML Attribute ได้โดยตรง

    <div id="{{ dynamicId }}"></div>

Vue ใช้ Directive v-bind แทน

    <script setup>
        import { ref } from 'vue'

        const dynamicId = ref('product-list')
    </script>

    <template>
        <div v-bind:id="dynamicId"></div>
    </template>

Vue จะทำให้ Attribute id มีค่าตรงกับ dynamicId โดยอัตโนมัติ
หากค่าที่ Bind เป็น null หรือ undefined Vue จะลบ Attribute นั้นออกจาก Element

#### Shorthand — รูปแบบย่อ
เนื่องจาก v-bind ถูกใช้บ่อย Vue จึงมีรูปแบบย่อให้ใช้

    <div v-bind:id="dynamicId"></div>

สามารถเขียนเป็น

    <div :id="dynamicId"></div>

### Boolean Attributes — Attribute ที่มีค่า True/False
HTML มี Attribute บางประเภทที่ใช้บอกสถานะ เช่น
- disabled
- readonly
- checked
- selected

####
    const isButtonDisabled = ref(true)

    <button :disabled="isButtonDisabled">
        Submit
    </button>

หาก isButtonDisabled เป็น true ปุ่มจะถูก Disable
หากเป็น false Attribute disabled จะถูกนำออก
หลักการคือ
- true          เพิ่ม Attribute
- false         ลบ Attribute
- null          ลบ Attribute
- undefined     ลบ Attribute

### Dynamically Binding Multiple Attributes
หากต้องการ Bind หลาย Attribute พร้อมกัน สามารถใช้ Object ได้

    <script setup>
        const objectOfAttrs = {
          id: 'container',
          class: 'wrapper',
          style: 'background-color: green'
        }
    </script>

    <template>
        <div v-bind="objectOfAttrs"></div>
    </template>

ผลลัพธ์จะเทียบเท่ากับ

    <div
        id="container"
        class="wrapper"
        style="background-color: green"
    ></div>

แทนที่จะเขียน Attribute ทีละตัว เราสามารถใช้ v-bind กับ Object ได้โดยตรง

## Using JavaScript Expressions — การใช้ JavaScript Expression
Vue รองรับ JavaScript Expression ภายใน Data Binding ไม่ได้จำกัดแค่การเรียกตัวแปร

    <template>
      <p>{{ number + 1 }}</p>
    
      <p>{{ ok ? 'YES' : 'NO' }}</p>
    
      <p>{{ message.split('').reverse().join('') }}</p>
    
      <div :id="`list-${id}`"></div>
    </template>

สามารถใช้ JavaScript Expression ได้ใน
- Text Interpolation เช่น `{{ }}`
- Attribute Value ของ Directive เช่น :id, v-if

### Expressions Only
แต่ละ Data Binding ต้องประกอบด้วย Expression เพียงหนึ่งรายการ
Expression คือโค้ดที่สามารถประเมินผลและคืนค่าออกมาได้
วิธีสังเกตง่าย ๆ คือ ลองคิดว่าโค้ดนั้นสามารถนำไปวางหลัง return ได้หรือไม่

ตัวอย่างที่ถูกต้อง:

    <p>{{ number + 1 }}</p>

ตัวอย่างที่ไม่ถูกต้อง:

    <p>{{ var a = 1 }}</p>

เพราะ var a = 1 เป็น Statement ไม่ใช่ Expression

### Calling Functions — การเรียก Function
สามารถเรียก Function ที่ Component เปิดให้ Template ใช้งานได้

    <script setup>
    function formatDate(date) {
      return new Date(date).toLocaleDateString()
    }
    </script>

    <template>
      <time :datetime="date">
        {{ formatDate(date) }}
      </time>
    </template>

#### ข้อควรระวัง
Function ที่ถูกเรียกภายใน Template Expression อาจถูกเรียกทุกครั้งที่ Component Update

ดังนั้น Function ควรเป็น Function ที่ไม่มี Side Effects เช่น:
- ไม่เปลี่ยนแปลง State
- ไม่เรียก API
- ไม่ทำงานแบบ Asynchronous
ควรใช้ Function เพื่อคำนวณหรือจัดรูปแบบข้อมูลเท่านั้น

### Restricted Globals Access
Template Expression ของ Vue ทำงานภายใต้ระบบที่จำกัดการเข้าถึง Global Variables

Vue อนุญาตให้ใช้ Global Variables ที่พบได้ทั่วไป เช่น:
- Math
- Date
แต่ Global Variables ที่ไม่ได้อยู่ในรายการอนุญาต เช่น Property ที่กำหนดเองบน window จะไม่สามารถเรียกใช้ได้โดยตรงจาก Template Expression

หากต้องการเพิ่ม Global Properties ให้กับ Vue สามารถกำหนดผ่าน app.config.globalProperties ได้

## Directives — คำสั่งพิเศษของ Vue
Directive คือ Attribute พิเศษที่ Vue จัดเตรียมไว้ โดยชื่อจะขึ้นต้นด้วย v-

ตัวอย่าง:
- v-html
- v-bind
- v-if
- v-for
- v-on
- v-model

Directive มีหน้าที่ทำให้ DOM เปลี่ยนแปลงตามค่าของ Expression แบบ Reactive

    <script setup>
        import { ref } from 'vue'

        const seen = ref(true)
    </script>

    <template>
        <p v-if="seen">
          Now you see me
        </p>
    </template>

เมื่อ seen เป็น true ข้อความจะแสดง
เมื่อ seen เป็น false Element `<p>` จะถูกนำออกจาก DOM

### Arguments — อาร์กิวเมนต์ของ Directive
Directive บางประเภทสามารถรับ Argument ได้ โดยใช้เครื่องหมาย `:` หลังชื่อ Directive

    <a v-bind:href="url">Link</a>

ในตัวอย่างนี้:
- v-bind คือ Directive
- href คือ Argument
- url คือ Expression ที่ต้องการ Bind

สามารถเขียนแบบย่อได้:

    <a :href="url">Link</a>

ตัวอย่าง v-on สำหรับ Event:

    <a v-on:click="doSomething">Click me</a>

สามารถเขียนแบบย่อได้:

    <a @click="doSomething">Click me</a>

#### สรุป
- v-bind `(:)` ทำหน้าที่ Bind Attribute
- v-on `(@)` ทำหน้ที่รับ Event

### Dynamic Arguments — Argument แบบไดนามิก
Vue รองรับการกำหนด Argument ด้วย JavaScript Expression โดยใช้เครื่องหมาย []

    <script setup>
        import { ref } from 'vue'

        const attributeName = ref('href')
        const url = ref('https://vuejs.org')
    </script>

    <template>
        <a v-bind:[attributeName]="url">
            Vue Website
        </a>
    </template>

ในตัวอย่างนี้ attributeName มีค่าเป็น "href" ดังนั้น

    <a v-bind:[attributeName]="url">

จะมีค่าเท่ากับ

    <a v-bind:href="url">

สามารถใช้ Dynamic Argument กับ Event ได้เช่นกัน:

    <script setup>
        const eventName = 'focus'

        function doSomething() {
          console.log('Event triggered')
        }
    </script>

    <template>
        <input @[eventName]="doSomething" />
    </template>

### Modifiers — ตัวปรับพฤติกรรมของ Directive
Modifiers คือส่วนเพิ่มเติมของ Directive ที่ใช้เครื่องหมายจุด .

    <form @submit.prevent="onSubmit">
      <button type="submit">
        Submit
      </button>
    </form>

Modifier .prevent จะสั่งให้ Vue เรียก

    event.preventDefault()

เมื่อเกิด Event ที่กำหนด
ปกติการ Submit Form จะทำให้ Browser Reload หน้าเว็บ แต่เมื่อใช้ .prevent จะป้องกันพฤติกรรมดังกล่าว

ตัวอย่าง Modifier ที่จะพบในบทเรียนถัดไป
- @click.stop
- @submit.prevent
- v-model.trim
- v-model.number

## สรุป Template Syntax ที่ควรจำ
- `{{ value }}`	แสดงข้อมูล
- `v-html`	แสดง HTML จริง
- `:id="value"`	Bind Attribute
- `:disabled="isDisabled"`	Bind Boolean Attribute
- `v-bind="object"`	Bind หลาย Attribute
- `v-if="condition"`	แสดง Element ตามเงื่อนไข
- `@click="handler"`	รับ Event
- `@[eventName]="handler"`	รับ Event แบบไดนามิก
- `.prevent`	ป้องกัน Default Event