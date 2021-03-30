<template>
   <div id="app" class="small-container">
      <h2 class="header">Contacts</h2>
      <add-form @add:contacts="addContact" />
      <address-table
         :contacts="contacts"
         @delete:contact="deleteContact"
         @edit:contact="editContact"
      />
   </div>
</template>

<script>
import AddressTable from "@/components/AddressTable.vue";
import AddForm from "@/components/AddForm.vue";
import axios from "axios";


export default {
   name: "App",
   components: {
      AddressTable,
      AddForm,
   },
   data() {
      return {
         contacts: [
            // {
            //   id: 1,
            //   name: 'Jacob Hunsaker',
            //   address: '257 Wymount Terrace, Provo, Utah',
            //   phone_num: '123-456-7890',
            //   email: 'jacob.hunsaker96@gmail.com',
            // },
         ],
      };
   },
   mounted() {
      this.getContacts();
   },
   methods: {
      async getContacts() {
         try {
            const response = await fetch("http://localhost:3000/address");
            const data = await response.json();
            this.contacts = data;
         } catch (error) {
            console.error(error);
         }
      },
      addContact(contact) {
         const { id, name, address, phone_num, email } = contact;
         axios
            .post("http://localhost:3000/address", {
               id,
               name,
               address,
               phone_num,
               email,
            })
            .then((res) => (this.contacts = [...this.contacts, res.data]))
            .catch((err) => console.log(err));
      },
      async deleteContact(id) {
         try {
            await fetch(`http://localhost:3000/address/${id}`, {
               method: "DELETE",
            });
            this.contacts = this.contacts.filter(
               (contact) => contact.id !== id
            );
         } catch (error) {
            console.error(error);
         }
      },
      async editContact(id, updatedContact) {
         try {
            const response = await fetch(
               `http://localhost:3000/address/${id}`,
               {
                  method: "PATCH",
                  body: JSON.stringify(updatedContact),
                  headers: {
                     "Content-type": "application/json; charset=UTF-8",
                  },
               }
            );
            const data = await response.json();
            this.contacts = this.contacts.map((contact) =>
               contact.id === id ? data : contact
            );
         } catch (error) {
            console.error(error);
         }
      },
   },
};
</script>

<style>
#app {
   font-family: Avenir, Helvetica, Arial, sans-serif;
   -webkit-font-smoothing: antialiased;
   -moz-osx-font-smoothing: grayscale;
   text-align: center;
   color: #2c3e50;
   margin-top: 30px;
}
button {
   display: inline-block;
   border: none;
   background: #555;
   color: #fff;
   padding: 7px 20px;
   cursor: pointer;
   text-align: center;
}
button:hover {
   background: #666;
   border: none;
}
.header {
   background: #333;
   color: #fff;
   text-align: center;
   padding: 10px;
}
.header a {
   color: #fff;
   text-decoration: none;
}
</style>
