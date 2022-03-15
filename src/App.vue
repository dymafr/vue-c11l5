<template>
  <form>
    <div>
      <input
        v-model="passwordValue"
        type="password"
        placeholder="Mot de passe"
      />
      <input
        v-model="validatePasswordValue"
        @blur="handleChange"
        type="password"
        placeholder="Vérifier le mot de passe"
      />
    </div>
    <p v-if="confirmPasswordError">{{ confirmPasswordError }}</p>
    <div>
      <input v-model="emailValue" type="email" placeholder="Email" />
    </div>
  </form>
</template>

<script setup lang="ts">
import { useForm, useField } from 'vee-validate';
import { z } from 'zod';
import { toFormValidator } from '@vee-validate/zod';

const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve(false);
  }, 3000);
});

const validationSchema = z
  .object({
    password: z.string(),
    validatePassword: z.string(),
    email: z
      .string()
      .refine(async (data) => await promise, { message: 'Email non valide' }),
  })
  .refine((data) => data.password === data.validatePassword, {
    path: ['validatePassword'],
    message: 'Les mots de passe ne correspondent pas',
  });

useForm({
  validationSchema: toFormValidator(validationSchema),
});

const { value: passwordValue } = useField('password');
const {
  value: validatePasswordValue,
  handleChange,
  errorMessage: confirmPasswordError,
} = useField('validatePassword', null, { validateOnValueUpdate: false });
const { value: emailValue } = useField('email');
</script>

<style scoped lang="scss"></style>
