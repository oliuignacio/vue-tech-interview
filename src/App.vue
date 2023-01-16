<script setup lang="ts">
import { ref } from "vue";

const customerName = ref("");
const age = ref("");
const formUpdate = ref(false);
const updateId = ref(-1);
const updateName = ref("");
const updateAge = ref("");
const customers = ref ([]);
const orderList = ref(true);
const id = ref(-1);

function sanitize (input) {

  if ((input.name).length > 50 || Number(input.age) > 130) {
    alert("You can't add such a long value!");
    return false;
  }
  if ((input.name).length < 1 || Number(input.age) < 1) {
    alert("You must enter a name and age should be more than cero!");
    return false;
  }

  return true;

}


function createCustomer() {

  const input = {
    name: customerName.value,
    age: age.value,
  }

  id.value++;

  if (sanitize(input)) {
  
    const formInput = {
      name: customerName.value,
      age: age.value,
      id: id.value,
    };
  
    (customers.value as any).push(formInput);
  
    
  
    console.log("Form submitted!", customers.value[0].id, customers.value[0].name, customers.value[0].age);
    customerName.value="";
    age.value="";
    return;
  
  } 

};

function update(index) {
  console.log("updating!", index);
  updateId.value=index //customers.value[index].id;
  formUpdate.value=true;

};

function save(index) {

  const input = {
    name: updateName.value,
    age: updateAge.value,
  };

  if (sanitize(input)) {
    console.log("saving!", index);
    const updatedCustomer = customers.value[index];//customers.value.filter((customer) => customer.id === updateId.value);

    updatedCustomer.name = input.name;
    updatedCustomer.age = input.age;

    customers.value.splice(index, 1, updatedCustomer);

    updateAge.value = "";
    updateName.value = "";

    formUpdate.value = false;
    updateId.value = -1;
    return;

  }

};

function erase(index) {
  console.log("erasing!", index);
  customers.value.splice(index, 1);
  return;
  
};

function orderByAge() {
  console.log("Ordering List!", orderList.value);
  
  let sortedListCustomers =[];

  if (orderList.value) {
    sortedListCustomers = customers.value.sort((a, b) => a.age - b.age);
  } else {
    sortedListCustomers = customers.value.sort((a, b) => a.id - b.id);
  }
  
  
  customers.value = sortedListCustomers;
  orderList.value = !orderList.value;
  return;

};



</script>

<template>
  <body >
    <div class="container">
      
      
      <section class="form">
        <form action="#" @submit.prevent="createCustomer" class="text-center">
          <input class="form-control mb-1" v-model="customerName" type="text" required name="customerName" id="customerName" />
          <input class="form-control mb-1" v-model="age" type="number" min="1" required name="age" id="age" />
          <input @click="createCustomer" type="submit" value="Add Customer" class="btn btn-success  mr-1">
          <input @click="orderByAge(orderList)" type="button" value="orderByAge" class="btn btn-success">
        </form>
        
      </section>
      <section class="data">
       
        <caption>Customer</caption>
            <table class="table">
                <thead>
                    <tr>
                        <th scope="col">ID</th>
                        <th scope="col">NAME</th>
                        <th scope="col">AGE</th>

                        <th></th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(customer, index) in customers" >
                        <td><p v-if="customer.id" >{{ customer.id }}</p>
                            <p v-if="customer.id==0" >0</p>
                        </td>
                        <td>
                            <span v-if="formUpdate && updateId == index">
                                <!-- Formulario para actualizar -->
                                <input v-model="updateName" type="text" class="form-control">
                            </span>
                            <span v-else>
                                <p v-if="customer.name" v-bind:class="{color_red : customer.name.charAt(0) == 'M', color_blue : customer.name.charAt(0) == 'K'}">{{ customer.name }}</p>
                            </span>
                        </td>
                        <td>
                            <span v-if="formUpdate && updateId == index">
                                <!-- Formulario para actualizar -->
                                <input v-model="updateAge" type="text" class="form-control">
                            </span>
                            <span v-else>
                                <!-- Dato edad -->
                                {{ customer.age }}
                            </span>
                        </td>
                        <td>
                            <!-- Botón para guardar la información actualizada -->
                            <span v-if="formUpdate && updateId == index">
                                <button @click="save(index)" class="btn btn-success">SAVE</button>
                            </span>
                            <span v-else>
                                <!-- Botón para mostrar el formulario de actualizar -->
                                <button @click="update(index)" class="btn btn-warning mr-2 mb-2">UPDATE</button>
                                <!-- Botón para borrar -->
                                <button @click="erase(index)" class="btn btn-danger">ERASE</button>
                            </span>
                        </td>
                    </tr>
                </tbody>
            </table>
        
      </section>
      
    </div>
  </body>

</template>

<style scoped>

.color_red {
  color: red;
}
.color_blue {
  color: blue;
}

</style>
