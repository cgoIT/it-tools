<script setup lang="ts">
import { AES, RC4, Rabbit, TripleDES, enc } from 'crypto-js';

const algos = { AES, TripleDES, Rabbit, RC4 };

const cypherInput = ref('Lorem ipsum dolor sit amet');
const cypherAlgo = ref<keyof typeof algos>('AES');
const cypherSecret = ref('my secret key');
const cypherOutput = computed(() => algos[cypherAlgo.value].encrypt(cypherInput.value, cypherSecret.value).toString());

const decryptInput = ref('U2FsdGVkX1/EC3+6P5dbbkZ3e1kQ5o2yzuU0NHTjmrKnLBEwreV489Kr0DIB+uBs');
const decryptAlgo = ref<keyof typeof algos>('AES');
const decryptSecret = ref('my secret key');
const decryptOutput = computed(() =>
  algos[decryptAlgo.value].decrypt(decryptInput.value, decryptSecret.value).toString(enc.Utf8),
);
</script>

<template>
  <c-card title="Encrypt">
    <div flex gap-3>
      <n-form-item label="Your text:" :show-feedback="false" flex-1>
        <n-input
          v-model:value="cypherInput"
          type="textarea"
          placeholder="The string to cypher"
          :autosize="{ minRows: 4 }"
          autocomplete="off"
          autocorrect="off"
          autocapitalize="off"
          spellcheck="false"
        />
      </n-form-item>
      <div flex flex-1 flex-col gap-2>
        <c-input-text v-model:value="cypherSecret" label="Your secret key:" clearable raw-text />

        <n-form-item label="Encryption algorithm:" :show-feedback="false">
          <n-select
            v-model:value="cypherAlgo"
            :options="Object.keys(algos).map((label) => ({ label, value: label }))"
          />
        </n-form-item>
      </div>
    </div>
    <n-form-item label="Your text encrypted:" :show-feedback="false" mt-5>
      <n-input
        :value="cypherOutput"
        type="textarea"
        placeholder="Your string hash"
        :autosize="{ minRows: 2 }"
        readonly
        autocomplete="off"
        autocorrect="off"
        autocapitalize="off"
        spellcheck="false"
      />
    </n-form-item>
  </c-card>
  <c-card title="Decrypt">
    <div flex gap-3>
      <n-form-item label="Your encrypted text:" :show-feedback="false" flex-1>
        <n-input
          v-model:value="decryptInput"
          type="textarea"
          placeholder="The string to cypher"
          :autosize="{ minRows: 4 }"
          autocomplete="off"
          autocorrect="off"
          autocapitalize="off"
          spellcheck="false"
        />
      </n-form-item>
      <div flex flex-1 flex-col gap-2>
        <c-input-text v-model:value="decryptSecret" label="Your secret key:" clearable raw-text />

        <n-form-item label="Encryption algorithm:" :show-feedback="false">
          <n-select
            v-model:value="decryptAlgo"
            :options="Object.keys(algos).map((label) => ({ label, value: label }))"
          />
        </n-form-item>
      </div>
    </div>
    <n-form-item label="Your decrypted text:" :show-feedback="false" mt-5>
      <n-input
        :value="decryptOutput"
        type="textarea"
        placeholder="Your string hash"
        :autosize="{ minRows: 2 }"
        readonly
        autocomplete="off"
        autocorrect="off"
        autocapitalize="off"
        spellcheck="false"
      />
    </n-form-item>
  </c-card>
</template>
