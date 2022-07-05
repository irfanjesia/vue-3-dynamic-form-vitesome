<script lang="ts">
import { defineComponent, reactive } from 'vue';
import { TextField, EmailField, PasswordField, CheckboxField, Validator, required, email, pattern } from '@asigloo/vue-dynamic-forms';

export default defineComponent({
  name: 'Home',
  setup() {
    const formValues = reactive({})

    const form = reactive({
      id: 'example-id',
      fields: {
        username: TextField({
          label: 'Username'
        }),
        email: EmailField({
          label: 'Email',
          validations: [
            Validator({ validator: required, text: 'This field is required' }),
            Validator({ validator: email, text: 'Format of email is incorrect' }),
          ],
        }),
        password: PasswordField({
          label: 'Password',
          validations: [
            Validator({
              validator: pattern(
                '^(?=.*[a-z])(?=.*[A-Z])(?=.*)(?=.*[#$^+=!*()@%&]).{8,10}$',
              ),
              text: 'Password must contain at least 1 Uppercase, 1 Lowercase, 1 number, 1 special character and min 8 characters max 10',
            }),
          ],
        }),
        remember: CheckboxField({
          label: 'Remember me'
        }),
        conditions: CheckboxField({
          label: 'I agree with the terms and conditions',
          condition: 'remember'
        }),
      },
    });

    // Comments are for submit button
    // function onFormSubmit(values) {
    //   console.log('Form submitted!', values)
    // }

    // function processErrors(errors) {
    //   console.log('Form error!', errors)
    // }

    function onFormChange(values) {
      Object.assign(formValues, values);
    }

    return {
      form,
      formValues,
      onFormChange,
      // onFormSubmit,
      // processErrors
    };
  },
});
</script>

<template>
  <div class="container max-w-3xl mx-auto mt-20">
    <dynamic-form :form="form" @change="onFormChange" />
    <!-- <dynamic-form :form="form" @submitted="onFormSubmit" @error="processErrors" /> -->
    <!-- <button class="bg-teal-400 text-light-50 px-2.5 py-1.5 rounded font-bold text-xs" submit="true"
      :form="form.id">Submit</button> -->
    <pre>{{ formValues }}</pre>
  </div>
</template>

<style>
</style>
