<script>
export default {
   name: "address-table",
   props: {
      contacts: Array,
   },
   data() {
      return {
         editing: null,
      };
   },
   methods: {
      editMode(id) {
         this.editing = id;
      },
      editContact(contact) {
         if (
            contact.name === "" ||
            contact.address === "" ||
            contact.phone_num === "" ||
            contact.email === "" ||
            contact.category === ""
         )
            return;
         this.$emit("edit:contact", contact.id, contact);
         this.editing = null;
      },
      cancelEdit(contact) {
         Object.assign(contact, this.cachedContact);
         this.editing = null;
      },
   },
};
</script>

<template>
   <div id="address-table">
      <p v-if="contacts.length < 1" class="empty-table">
         No Contacts
      </p>
      <table v-else>
         <thead>
            <tr>
               <th>Name</th>
               <th>Address</th>
               <th>Phone #</th>
               <th>Email</th>
               <th>Actions</th>
            </tr>
         </thead>
         <tbody>
            <tr v-for="contact in contacts" :key="contact.id">
               <td v-if="editing === contact.id">
                  <input type="text" v-model="contact.name" />
               </td>
               <td v-else>{{ contact.name }}</td>
               <td v-if="editing === contact.id">
                  <input type="text" v-model="contact.address" />
               </td>
               <td v-else>{{ contact.address }}</td>
               <td v-if="editing === contact.id">
                  <input type="text" v-model="contact.phone_num" />
               </td>
               <td v-else>{{ contact.phone_num }}</td>
               <td v-if="editing === contact.id">
                  <input type="text" v-model="contact.email" />
               </td>
               <td v-else>{{ contact.email }}</td>
               <td v-if="editing === contact.id">
                  <button @click="editContact(contact)">
                     Save
                  </button>
                  <button @click="editing = null">Cancel</button>
               </td>
               <td v-else>
                  <button @click="editMode(contact.id)">
                     Edit
                  </button>
                  <button @click="$emit('delete:contact', contact.id)">
                     Delete
                  </button>
               </td>
            </tr>
         </tbody>
      </table>
   </div>
</template>

<style scoped>
button {
   margin: 0.5rem 0.5rem 0 0;
}
</style>
