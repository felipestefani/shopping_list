<script setup>
  import { nextTick, ref } from "vue";
  import 'primeicons/primeicons.css';

  const newItem = ref('')
  const editando = ref(false)
  const list = ref([])
  const newId = ref(1)

  const estaEditando = e => {
    editando.value = e
  }

  const addItem = () => {
    if(newItem.value.length < 3){
      return
    }
    list.value = [...list.value, {id: newId.value,name: newItem.value, comprado: false, ehTexto: true}]
    newId.value++
    newItem.value = ''
  }

  const excluirItem = item => {
    const newList = [...list.value].filter((el) => {return el != item})
    list.value = newList
  }

  const riscar = item => {item.comprado = !item.comprado}

  const editar = item => {
    item.ehTexto = !item.ehTexto
    nextTick(() => {
      document.getElementById(item.id).focus()
    })
  } 

</script>

<template>
  <div class="container">
    <h1>Lista de compras</h1>
    <section>
      <button class="primary" v-if="!editando" @click="estaEditando(true)">Editar</button>
      <button class="close" v-else @click="estaEditando(false)">Fechar</button>
    </section>
    <main>
      <form v-if="editando" @submit.prevent="addItem">
        <input type="text" placeholder="Insira um item" v-model="newItem">
        <button class="primary" type="submit" :disabled="newItem.length < 3">Adicionar item</button>
      </form>
      <ul>
        <li v-for="(item) in list">
          <p :class="{risc: item.comprado}" @click="editar(item)" :key="item.id" v-if="item.ehTexto">{{ item.name }}</p>
          <input :id=item.id type="text" v-model="item.name" :key="item.id" :hidden="item.ehTexto" @blur="item.ehTexto=!item.ehTexto" >
          <div class="icons">
            <i class="pi pi-check" style="color: green;" @click="riscar(item)" v-if="!item.comprado && editando"></i>
            <i class="pi pi-times" style="color: red" @click="riscar(item)" v-if="item.comprado && editando"></i>
            <i class="pi pi-trash" @click="excluirItem(item)" v-if="editando" style="color: hsl(208, 82%, 15%);"></i>
          </div>   
        </li>
      </ul>
      <p v-if="list.length <= 0 && !editando">Nenhum item na lista!</p>
    </main>
  </div>
</template>

<style>
  *{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body{
    background-color: antiquewhite;
  }

  .container {
    padding-top: 2em;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  section {
    width: 100%;
    display: flex;
    justify-content: center;
    padding: 1em 0;
  }

  main {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  form {
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 1em;
  }

  h1 {
    text-align: center;
    font-family: Arial, Helvetica, sans-serif;
  }

  button, input {
    border: none;
    height: 3em;
    width: 100%;
    border-radius: 10px;
    font-size: 14px;
    font-family:Verdana, Geneva, Tahoma, sans-serif
  }

  button {
    margin: 1em;
    padding: 1em;
    box-shadow: 5px 5px 5px rgba(173, 216, 230, 0.5);
  }

  input{
    box-shadow: 0px 0px 20px rgba(169, 169, 169, 0.2);
    padding-left: 1em;
    background-color: rgb(243, 243, 243);
    margin-left: 1em;
  }

  .primary {
    background-color: lightblue;
  }

  .close {
    background-color: lightcoral;
  }

  ul{
    width: 100%;
    list-style-type: none;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  li {
    width: 80%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 20px;
    list-style: none;
    margin-bottom: 0.5em;
  }

  p {
    padding: 0;
    margin: 0.5em 0;
    font-family:Verdana, Geneva, Tahoma, sans-serif;
    color:hsl(208, 82%, 15%)
  }

  i {
    margin-left: 1em;
  }

  .risc {
    text-decoration: line-through;
  }

  @media (min-width: 701px) {
    button, input, li {
      width: 18em
    }
  }
 
  
</style>
