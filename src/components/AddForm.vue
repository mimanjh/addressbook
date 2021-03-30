<template>
   <div id="add-form">
      <form @submit.prevent="handleSubmit">
         <label>Name:</label>
         <input
            type="text"
            :class="{ 'has-error': submitting && invalidName }"
            v-model="contacts.name"
            @focus="clearStatus"
            @keypress="clearStatus"
            ref="first"
         />
         <label>Address:</label>
         <input
            v-model="contacts.address"
            type="text"
            :class="{ 'has-error': submitting && invalidAddress }"
            @focus="clearStatus"
            @keypress="clearStatus"
         />
         <label>Phone Number:</label>
         <input
            type="text"
            :class="{ 'has-error': submitting && invalidPhone }"
            v-model="contacts.phone_num"
            @focus="clearStatus"
            @keypress="clearStatus"
         />
         <label>Email:</label>
         <input
            type="text"
            :class="{ 'has-error': submitting && invalidEmail }"
            v-model="contacts.email"
            @focus="clearStatus"
            @keypress="clearStatus"
         />
         <p v-if="error && submitting" class="error-message">
            Please fill out all required fields
         </p>
         <p v-if="success" class="success-message">
            Contact successfully added
         </p>
         <button>ADD</button>
      </form>
   </div>
</template>

<script>
export default {
   name: "add-form",
   data() {
      return {
         submitting: false,
         error: false,
         success: false,
         contacts: [
            {
               id: "",
               name: "",
               address: "",
               phone_num: "",
               email: "",
            },
         ],
      };
   },
   methods: {
      handleSubmit() {
         this.submitting = true;
         this.clearStatus();
         if (
            this.invalidName ||
            this.invalidAddress ||
            this.invalidPhone ||
            this.invalidEmail
         ) {
            this.error = true;
            return;
         }

         this.$emit("add:contacts", this.contacts);
         this.$refs.first.focus();
         this.contacts = {
            id: "",
            name: "",
            address: "",
            phone_num: "",
            email: "",
         };
         this.error = false;
         this.success = true;
         this.submitting = false;
      },

      clearStatus() {
         this.success = false;
         this.error = false;
      },
   },
   computed: {
      invalidName() {
         return this.contacts.name === "";
      },
      invalidAddress() {
         return this.contacts.address === "";
      },
      invalidPhone() {
         return this.contacts.phone_num === "";
      },
      invalidEmail() {
         return this.contacts.email === "";
      },
   },
};
</script>

<style scoped>
form {
   margin-bottom: 2rem;
}
[class*="-message"] {
   font-weight: 500;
}

.error-message {
   color: #d33c40;
}

.success-message {
   color: #32a95d;
}
</style>
