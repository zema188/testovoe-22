<template>
    <form 
        class="form"
        :name="form.name"
        @submit.prevent="onSubmit($event)"
        @reset="resetForm($event)"
    >
        <div class="form__title">
            {{ form.name }}
        </div>
        <div
            v-for="(item, i) of form.items" :key="i"
            class="form__item"
        >

            <form-input
                v-if="item.type === 'input'"
                :label="item.label"
                :type="item.type"
                :name="item.name"
            />

            <form-select
                v-if="item.type === 'select'"
                :label="item.label"
                :options="item.additional.options"
                :name="item.name"
            />

            <form-radio
                v-if="item.type === 'radio'"
                :label="item.label"
                :options="item.additional.options"
                :name="item.name"
                :formIndex="formIndex"
            />

            <form-password
                v-if="item.type === 'password'"
                :label="item.label"
                :name="item.name"
            />
        
        </div>

        <div class="error-w">
            <div class="error" v-if="errorPasswords">
                Пароли должны совпадать
            </div>
        </div>


        <button class="btn btn_green" type="submit">
            Отправить
        </button>

        <button class="btn btn_grey"  type="reset">
            Стереть
        </button>
    </form>
</template>

<script>
import FormInput from "@/components/form-items/FormInput.vue";
import FormSelect from "@/components/form-items/FormSelect.vue";
import FormRadio from "@/components/form-items/FormRadio.vue";
import FormPassword from "@/components/form-items/FormPassword.vue";

export default {
    name: "TheForm",

    components: {FormPassword, FormRadio, FormSelect, FormInput},

    props: {
        form: {
            type: Object,
            required: true,
        },

        formIndex: {
            type: Number,
            required: true
        },

        formName: {
            type: String,
            required: true
        }
    },

    data() {
        return {
            errorPasswords: false
        }
    },

    methods: {

        async onSubmit(event) {
            event.preventDefault();

            const formData = new FormData(event.target);

            const childData = {
                name: formData.get('name'),
                gender: formData.get('gender'),
                age: parseInt(formData.get('age')),
                pass: formData.get('pass')
            };

            const jsonData = JSON.stringify({ [this.formName]: childData });

            try {
                const response = await fetch('test/api/form', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: jsonData,
                });

            } catch(err) {
                console.error('Ошибка загрузки отправки формы', err);
            }
        },


        resetForm(event) {
            event.target.reset();
            this.errorPasswords = false;
        }

    },

}
</script>

<style lang="scss" scoped>
.form {
    display: flex;
    flex-direction: column;
    gap: 10px;
    padding: 20px;
    border-radius: 8px;
    border: 1px solid #969696;
    flex: 1 0 calc(50% - 25px);
    @media (max-width: 539px) {
        flex: 1 0 100%;
    }
    &__title {
        font-size: 25px;
        font-weight: bold;
    }
    &__item {

    }
}

.error-w {
    min-height: 30px;
    display: flex;
    align-items: center;
}

.error {
    color: red;
    font-weight: bold;
}
</style>
