<template>
  <form>
    <input v-model="passwordValue" type="password" placeholder="Mot de passe" />
    <input
      v-model="validatePasswordValue"
      @blur="handleChange"
      type="password"
      placeholder="Vérifier le mot de passe"
    />
    <p v-if="confirmPasswordError">{{ confirmPasswordError }}</p>
  </form>
</template>

<script setup lang="ts">
import { useForm, useField } from 'vee-validate';
import { z } from 'zod';
import { toFormValidator } from '@vee-validate/zod';

const validationSchema = z
  .object({
    password: z.string(),
    validatePassword: z.string(),
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
</script>

<style scoped lang="scss"></style>
