<template>
    <div>
      <div v-if="currentStep">
        <h2 class="text-2xl font-bold">{{ currentStep.title }}</h2>
        <p class="text-gray-600">{{ currentStep.description }}</p>
        <form @submit.prevent="nextStep">
          <div v-for="field in currentStep.fields" :key="field.label" class="my-4">
            <label :for="field.label" class="block text-gray-700">{{ field.label }}</label>
            <input v-if="field.type === 'textfield'" 
                   :id="field.label"
                   v-model="formData[field.label]"
                   :placeholder="field.placeholder" 
                   :required="field.required" 
                   class="w-full px-4 py-2 border rounded-md" />
            <textarea v-if="field.type === 'textarea'"
                      :id="field.label"
                      v-model="formData[field.label]"
                      :placeholder="field.placeholder"
                      :required="field.required"
                      class="w-full px-4 py-2 border rounded-md"></textarea>
            <div v-if="field.type === 'radio'">
              <label v-for="option in field.options" :key="option.value" class="inline-flex items-center mt-2">
                <input type="radio"
                       :name="field.label"
                       :value="option.value"
                       v-model="formData[field.label]"
                       :required="field.required"
                       class="form-radio h-4 w-4" />
                <span class="ml-2">{{ option.label }}</span>
              </label>
            </div>
            <div v-if="field.type === 'autocomplete'">
              <select v-model="formData[field.label]" :id="field.label" class="w-full px-4 py-2 border rounded-md">
                <option v-for="option in field.options" :key="option" :value="option">{{ option }}</option>
              </select>
            </div>
          </div>
          <button type="submit" class="px-6 py-2 bg-blue-600 text-white rounded-md">Next</button>
        </form>
      </div>
      <div v-else>
        <p>Form completed! Here is your data:</p>
        <pre>{{ formData }}</pre>
      </div>
    </div>
  </template>
  
  <script>
  import formConfig from '../data/response.json';
  
  export default {
    data() {
      return {
        formConfig: formConfig,
        currentStepIndex: 0,
        formData: {}
      };
    },
    computed: {
      currentStep() {
        return this.formConfig[this.currentStepIndex];
      }
    },
    methods: {
      nextStep() {
        if (this.currentStepIndex < this.formConfig.length - 1) {
          this.currentStepIndex++;
        } else {
          this.currentStepIndex = null; // Form completed
        }
      }
    }
  };
  </script>
  
  <style scoped>
  </style>
  