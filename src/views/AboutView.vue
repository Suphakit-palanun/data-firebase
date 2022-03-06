<template>
  <div class="about">
    <div class="">
      <p>Date : <input type="text" v-model="date" /></p>
      <p>Name : <input type="text" v-model="name" /></p>
      <p>ATK : test results <input type="text" v-model="atk" /></p>
      <p>Tel : <input type="text" v-model="tel" /></p>
      <p>Email : <input type="text" v-model="email" /></p>
      <button @click="addData()">ADD DATA</button>
      <button @click="readData()">Read DATA</button>
    </div>
  </div>
  <div>
    <table class="table table-bordered border-primary bg-white container">
      <thead>
        <tr>
          <th scope="col">DATE</th>
          <th scope="col">F-L NAME</th>
          <th scope="col">result ATK</th>
          <th scope="col">Phone number</th>
          <th scope="col">Email</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in csDoc" :key="index" align="center">
          <td>{{ item.resdate }}</td>
          <td>{{ item.resname }}</td>
          <td>{{ item.resatk }}</td>
          <td>{{ item.phonenumber }}</td>
          <td>{{ item.resemail }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { collection, addDoc, getDocs } from "firebase/firestore";
import db from "../plugins/firebaselnit";

export default {
  data() {
    return {
      csDoc: [],
      name: "",
      email: "",
      atk: "",
      tel: "",
      date: "",
    };
  },
  methods: {
    async addData() {
      try {
        const docRef = await addDoc(collection(db, "covidtable"), {
          resname: this.name,
          resemail: this.email,
          resatk: this.atk,
          phonenumber: this.tel,
          resdate: this.date,
        });
        console.log("Document written with ID: ", docRef.id);
      } catch (e) {
        console.error("Error adding document: ", e);
      }
    },
    async readData() {
      const querySnapshot = await getDocs(collection(db, "covidtable"));
      querySnapshot.forEach((doc) => {
        console.log(`${doc.id} => ${doc.data()}`);
        this.csDoc.push(doc.data());
      });
    },
  },
};
</script>

<style>
</style>
