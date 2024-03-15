<template>
  <div class="form-builder">
    <the-form
      v-for="(form, key, i) of config" :key="i"
      :form="form"
      :formIndex="i"
    />
  </div>
</template>

<script>
import TheForm from "@/components/TheForm.vue";

export default {
  name: "FormBuilder",

  components: { TheForm },

  data() {
    return {
      config: {}
    };
  },

  methods: {
    async getConfig() {
      try {

        const configPath = '/form-config.json';
        const response = await fetch(configPath);
        const data = await response.json();

        this.config = { ...data };
        
        console.log(data)
      } catch(err) {
        console.error("Ошибка загрузки файла:", err);
      }
    },
  },

  mounted() {
    this.getConfig();
  }
}
</script>

<style lang="scss" scoped>
.form-builder {
  display: flex;
  gap: 50px;
  max-width: 1200px;
  margin: 0 auto;
  padding: 50px 15px;
  @media (max-width: 539px) {
    padding: 20px 15px;
    gap: 20px;
    flex-wrap: wrap;
  }
}
</style>
