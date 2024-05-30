<template>
  <div>
    <form class="container">
      <h1>โปรแกรมคำนวณการซื้อของที่ร้านค้า</h1>
      <div>
        <h3>เงื่อนไขของร้านค้า</h3>
        <p>ร้านที่ 1 ราคาสินค้าชิ้นละ 25 บาท เมื่อซื้อ 10 ชิ้นขึ้นไปลดให้ 20%</p>
        <p>ร้านที่ 2 ราคาสินค้าชิ้นละ 30 บาท เมื่อซื้อสินค้า 3 ชิ้น แถมให้ 1 ชิ้น</p>
      </div>
      <div class="money-input">
        <label for="money">กรุณาป้อนจำนวนเงินที่ต้องการ: </label>
        <input type="number" v-model.lazy="money">
      </div>
      <button @click.prevent="calculate" class="submit">ยืนยัน</button>
      <button @click="reset" class="reset">ล้างค่า</button>
    </form>

    <div v-if="result" class="content">
      <h2>ผลลัพธ์:</h2>
      <p>มีเงิน: {{ money }}</p>
      <p>ซื้อร้านที่ 1 ได้ {{ result.result1.amount }} ชิ้น เหลือเงิน {{ result.result1.Change }} บาท</p>
      <p>ซื้อร้านที่ 2 ได้ {{ result.result2.amount }} ชิ้น เหลือเงิน {{ result.result2.Change }} บาท</p>
      <p>{{ recommendation }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      money: null,
      result: null,
      recommendation: ""
    }
  },
  methods: {
    calculate() {
      if (this.money <= 0) {
        alert("กรุณาป้อนจำนวนเงินที่มีค่ามากกว่า 0")
        this.reset()
      } else {
        const money = this.money
        let store1_Price = 25
        const store2_Price = 30

        const store1_Discount = 0.2

        // ร้านที่ 1
        let store1_Amount_Check = Math.floor(money / store1_Price) //ซื้อสินค้าได้กี่ชิ้น
        let store1_Change = money % store1_Price //เงินที่เหลือ
        let store1_Amount

        if (store1_Amount_Check >= 10) {
          const store1_Price_Discount = store1_Price * store1_Discount
          this.store1_Price = store1_Price - store1_Price_Discount
          store1_Amount = Math.floor(money / this.store1_Price)
          store1_Change = money % this.store1_Price

        } else {
          store1_Amount = Math.floor(money / store1_Price)
        }


        //ร้านที่ 2
        const store2_Amount = Math.floor(money / store2_Price) //ซื้อสินค้าได้กี่ชิ้น
        const store2_Change = money % store2_Price //เงินที่เหลือ
        const store2_Amount_Free = Math.floor(store2_Amount / 3);
        const store2_Total_Amount = store2_Amount + store2_Amount_Free

        this.result = {
          result1: { amount: store1_Amount, Change: store1_Change },
          result2: { amount: store2_Total_Amount, Change: store2_Change }
        }

        if (store1_Amount > store2_Total_Amount) {
          this.recommendation = "แนะนำให้ซื้อร้านที่ 1 จะคุ้มที่สุด"
        } else if (store2_Total_Amount > store1_Amount || store2_Change > store1_Change) {
          this.recommendation = "แนะนำให้ซื้อร้านที่ 2 จะคุ้มที่สุด"
        } else if (store1_Change > store2_Change) {
          this.recommendation = "แนะนำให้ซื้อร้านที่ 1 จะคุ้มที่สุด"
        } else if (store2_Change > store1_Change) {
          this.recommendation = "แนะนำให้ซื้อร้านที่ 2 จะคุ้มที่สุด"
        } else {
          this.recommendation = "ทั้งสองร้านมีความคุ้มค่าเท่ากัน"
        }
      }
    },
    reset() {
      this.money = null
      this.result = null
      this.recommendation = ""
    }
  }
}
</script>

<style scoped>
*{
  font-family: Kanit;
}

.container {
  display: block;
  text-align: center;
  margin: 2rem auto;
  max-width: 40rem;
  border-radius: 12px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.25);
  padding: 2rem;
  background: #fff;
}
.money-input{
  text-align: start;
  padding: 0.5rem 0.5rem;
  margin: 0.5rem 0.5rem;
}

label{
  font-size: 18px;
  font-weight: bold;
  margin-right: 1rem;
}

input{
  display: block;
  width: 100%;
  margin-top: 0.5rem;
}

button {
  display: inline-block;
  font: inherit;
  color: white;
  cursor: pointer;
  padding: 0.75rem 2rem;
  margin: 0.5rem;
  border-radius: 15px;
}

.reset {
  background: red;
}

.reset:hover {
  background: orangered;
}

.submit {
  background: green;
}
.submit:hover {
  background: rgb(0, 150, 0);
}

.content{
  display: block;
  text-align: center;
  margin: 2rem auto;
  max-width: 40rem;
  border-radius: 12px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.25);
  padding: 2rem;
  background: #fff;
}

p{
  font-size: 18px;
}
</style>