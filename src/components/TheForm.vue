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
        }
    },

    data() {
        return {
            errorPasswords: false
        }
    },

    methods: {

        async onSubmit(event) {
            const formData = new FormData(event.target);


            let pass = null;
            let repeatPass = null;

            for (const [name, value] of formData) {

                if(pass && repeatPass) return;

                if (name === 'pass') {
                    pass = value;
                }
                if (name === 'repeat-pass') {
                    repeatPass = value;
                }
            }

            if(pass !== repeatPass) {
                console.log('Пароли не совпадают')
                this.errorPasswords = true;
                return
            } else {
                this.errorPasswords = false;
            }

            formData.delete('repeat-pass');

            try {
                
                const response = await fetch('test/api/form', {
                    method: 'POST',
                    body: formData,
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
