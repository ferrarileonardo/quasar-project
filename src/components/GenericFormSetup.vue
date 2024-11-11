<template>
  <div class="q-pa-md" style="max-width: 400px">
    <q-form @submit="onSubmit" @reset="onReset" class="q-gutter-md">
      <q-input
        v-if="props.hasText"
        filled
        v-model="name"
        :label="props.nameLabel"
        hint="Name and surname"
        lazy-rules
        :rules="[
          (val) => (val && val.length > 0) || 'Please type something',
          (val) =>
            val.length <= props.textCount! ||
            `Exceeds max length of ${props.textCount} characters`,
        ]"
      />

      <q-input
        filled
        type="number"
        v-model="age"
        :label="props.ageLabel"
        lazy-rules
        :rules="[
          (val) => (val !== null && val !== '') || 'Please type your age',
          (val) => (val > 0 && val < 100) || 'Please type a real age',
        ]"
      />

      <q-toggle v-model="accept" :label="props.acceptLabel" />
      <div v-if="props.hasRadio">
        <q-radio
          v-for="option in radioOptions"
          :key="option.value"
          v-model="selectedRadio"
          :label="option.label"
          :val="option.value"
        />
      </div>
      <q-input
        v-if="props.hasTextArea"
        type="textarea"
        v-model="textarea"
        filled
        :label="props.textareaLabel || 'Your message'"
        lazy-rules
        :rules="[(val) => (val && val.length > 0) || 'Please type something']"
      />
      <q-uploader
        v-if="props.hasFile"
        v-model="file"
        label="Upload your file"
      />
      <q-uploader
        v-if="props.hasFile"
        v-model="file"
        label="Upload your file"
      />
      <div v-if="props.checkboxCount">
        <q-checkbox
          v-for="n in props.checkboxCount"
          :key="n"
          v-model="checkboxes[n - 1]"
          :label="`Checkbox ${n}`"
        />
      </div>
      <div>
        <q-btn label="Submit" type="submit" color="primary" />
        <q-btn
          label="Reset"
          type="reset"
          color="primary"
          flat
          class="q-ml-sm"
        />
      </div>
    </q-form>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import { useQuasar } from 'quasar';

const props = defineProps<{
  nameLabel: string;
  ageLabel: string;
  acceptLabel: string;
  textCount?: number;
  hasText: boolean;
  hasCheckbox?: boolean;
  hasPassword?: boolean;
  hasTextArea?: boolean;
  textAreaCount?: boolean;
  hasFile?: boolean;
  fileCount?: number;
  checkboxCount?: number;
  hasRadio?: boolean;
  radioCount?: number;
  textareaLabel?: string;
}>();

const $q = useQuasar();

const name = ref<string | null>(null);
const age = ref<number | null>(null);
const accept = ref<boolean>(false);
const checkbox = ref<boolean>(false);
const checkboxes = ref<Array<boolean>>(Array(props.checkboxCount).fill(false));
const selectedRadio = ref<string | null>(null);
const radioOptions = [
  { label: 'Option 1', value: '1' },
  { label: 'Option 2', value: '2' },
  { label: 'Option 3', value: '3' },
];
const file = ref<File | null>(null);
const textarea = ref<string | null>(null);

function onSubmit() {
  if (accept.value !== true) {
    $q.notify({
      color: 'red-5',
      textColor: 'white',
      icon: 'warning',
      message: 'You need to accept the license and terms first',
    });
  } else {
    $q.notify({
      color: 'green-4',
      textColor: 'white',
      icon: 'cloud_done',
      message: 'Submitted',
    });
  }
}

function onReset() {
  name.value = null;
  age.value = null;
  accept.value = false;
  checkbox.value = false;
  checkboxes.value = Array(props.checkboxCount).fill(false);
  selectedRadio.value = null;
  file.value = null;
  textarea.value = null;
}
</script>
